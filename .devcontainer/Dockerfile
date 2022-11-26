# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT="3"
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
# COPY /workspaces/coach/requirements.txt /tmp/pip-tmp/
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt && rm -rf /tmp/pip-tmp

# Update repos
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive

# General
RUN apt-get install python3-pip cmake zlib1g-dev python3-tk python3-opencv software-properties-common -y --no-install-recommends

# Boost libraries
RUN apt-get install libboost-all-dev -y --no-install-recommends

# Scipy requirements
RUN apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran -y --no-install-recommends

# PyGame
RUN apt-get install libsdl-dev libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-ttf2.0-dev \
    libsmpeg-dev libportmidi-dev libavformat-dev libswscale-dev -y --no-install-recommends

# Dashboard
RUN apt-get install dpkg-dev build-essential python3.9-dev libjpeg-dev  libtiff-dev libsdl1.2-dev libnotify-dev \
    freeglut3 freeglut3-dev libsm-dev libgtk2.0-dev libgtk-3-dev libwebkit2gtk-4.0-37 libwebkit2gtk-4.0-dev \
    libgstreamer-plugins-base1.0-dev -y --no-install-recommends

# Gym
RUN apt-get install ffmpeg libsdl2-dev swig cmake -y --no-install-recommends

# Check if we need to download the code
ARG NOT_CODESPACE=1
RUN if [ $NOT_CODESPACE -eq 1 ] ; \
      then git clone https://github.com/LooseTerrifyingSpaceMonkey/coach.git  \
      && cd coach && git checkout initial-codespaces-work \
      && curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" \
      && bash Mambaforge-$(uname)-$(uname -m).sh -b -p $HOME/miniforge \
      && MAMBA_HOME=$HOME/miniforge \
    else echo Not Codespace is ${NOT_CODESPACE} && MAMBA_HOME = $HOME/miniforge ; \
    fi

# Create a conda environment
# First add conda forge and then update the packages
RUN conda update --all

# Create a conda environment
ARG VARIANT=3
RUN $MAMBA_HOME/bin/conda create --name tf python=${VARIANT} \
    && $MAMBA_HOME/bin/conda init bash --system \
    && echo "$MAMBA_HOME/bin/conda activate tf" > ~/.bashrc
    #    && bash -c "source $HOME/.bashrc" \
#    && $HOME/miniconda/bin/conda activate tf

RUN if [ $NOT_CODESPACE -eq 1 ] ; \
   then $MAMBA_HOME/bin/conda install cudatoolkit=11.2 cudnn=8.1.0 \
    && export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/ \
    && mkdir -p $CONDA_PREFIX/etc/conda/activate.d \
    && echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh \
  else echo Not Codespace 2 is ${NOT_CODESPACE} ; \
  fi
# RUN git clone https://github.com/LooseTerrifyingSpaceMonkey/coach.git && cd coach && git checkout initial-codespaces-work

RUN /usr/local/bin/python -m pip install --upgrade pip

RUN pip3 install virtualenv && virtualenv -p python3 coach_env && . coach_env/bin/activate

# Install requirements
RUN cd coach && pip3 install --user -r requirements.txt && pip3 install -e .
