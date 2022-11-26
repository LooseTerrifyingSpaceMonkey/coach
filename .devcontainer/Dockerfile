# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT="3"
ARG NOT_CODESPACE=1
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
# COPY /workspaces/coach/requirements.txt /tmp/pip-tmp/
# RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt && rm -rf /tmp/pip-tmp

# Update repos
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive

# General
RUN apt-get install python3-pip cmake zlib1g-dev python3-tk python3-opencv software-properties-common -y --no-install-recommends --no-warn-script-location

# Boost libraries
RUN apt-get install libboost-all-dev -y --no-install-recommends --no-warn-script-location

# Scipy requirements
RUN apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran -y --no-install-recommends --no-warn-script-location

# PyGame
RUN apt-get install libsdl-dev libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-ttf2.0-dev \
    libsmpeg-dev libportmidi-dev libavformat-dev libswscale-dev -y --no-install-recommends --no-warn-script-location

# Dashboard
RUN apt-get install dpkg-dev build-essential python3.9-dev libjpeg-dev  libtiff-dev libsdl1.2-dev libnotify-dev \
    freeglut3 freeglut3-dev libsm-dev libgtk2.0-dev libgtk-3-dev libwebkit2gtk-4.0-37 libwebkit2gtk-4.0-dev \
    libgstreamer-plugins-base1.0-dev -y --no-install-recommends --no-warn-script-location

# Gym
RUN apt-get install ffmpeg libsdl2-dev swig cmake -y --no-install-recommends --no-warn-script-location

#
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && apt-get -y install --no-install-recommends python3-pip cmake zlib1g-dev \
# python3-tk python3-opencv gcc libboost-all-dev libblas-dev liblapack-dev libatlas-base-dev gfortran dpkg-dev \
#  build-essential python3.9-dev libjpeg-dev libtiff-dev libsdl1.2-dev libnotify-dev freeglut3 freeglut3-dev \
#  libsm-dev libgtk2.0-dev libgtk-3-dev libwebkit2gtk-4.0-37 libwebkit2gtk-4.0-dev libgstreamer-plugins-base1.0-dev \
#  ffmpeg libsdl2-dev swig software-properties-common

# Add nvidia repository for cuda
# RUN add-apt-repository contrib && apt-key del 7fa2af80 && wget https://developer.download.nvidia.com/compute/cuda/repos/debian11/x86_64/cuda-keyring_1.0-1_all.deb \
#   && dpkg -i cuda-keyring_1.0-1_all.deb

# RUN apt-get -y install --no-install-recommends cuda nvidia-cuda-toolkit nvidia-driver

# RUN 'export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}' > $HOME/.bashrc

# RUN bash

# RUN if [[ -z "$arg" ]] ; then echo Argument not provided ; else echo Argument is $arg ; fi

RUN git clone https://github.com/LooseTerrifyingSpaceMonkey/coach.git

RUN /usr/local/bin/python -m pip install --upgrade pip
RUN pip3 install virtualenv && virtualenv -p python3 coach_env && . coach_env/bin/activate

# Install requirements
RUN cd coach && pip3 install --user -r requirements.txt && pip3 install -e .
