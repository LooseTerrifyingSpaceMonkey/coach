# [Choice] Python version: 3, 3.9, 3.8, 3.7, 3.6
ARG VARIANT="3"
FROM mcr.microsoft.com/vscode/devcontainers/python:${VARIANT}

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
COPY /workspaces/coach/requirements.txt /tmp/pip-tmp/
RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt && rm -rf /tmp/pip-tmp

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive && apt-get -y install --no-install-recommends python3-pip cmake zlib1g-dev \
  python3-tk python3-opencv libboost-all-dev libblas-dev liblapack-dev libatlas-base-dev gfortran dpkg-dev \
  build-essential python3.9-dev libjpeg-dev libtiff-dev libsdl1.2-dev libnotify-dev freeglut3 freeglut3-dev \
  libsm-dev libgtk2.0-dev libgtk-3-dev libwebkit2gtk-4.0-37 libwebkit2gtk-4.0-dev libgstreamer-plugins-base1.0-dev ffmpeg libsdl2-dev swig
  linux-headers-$(((uname -r) nvidia-cuda-dev nvidia-cuda-toolkit nvidia-driver

RUN 'export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}' > $HOME/.bashrc

RUN bash

# Install requirements
RUN pip3 install --user -r requirements.txt && pip3 install .
