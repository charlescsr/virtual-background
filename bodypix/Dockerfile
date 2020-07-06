# Base image with TensorFlow GPU requirements
FROM nvcr.io/nvidia/cuda:10.0-cudnn7-runtime-ubuntu18.04
#FROM tensorflow/tensorflow:1.15.2-gpu-py3

# Install node
RUN apt update && apt install -y curl make build-essential \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get -y install nodejs \
    && mkdir /.npm \
    && chmod 777 /.npm

# Install node package dependencies
WORKDIR /src
COPY package.json /src/
RUN npm install

# Setup our app as the entrypoint
COPY app.js /src/

# find -name libtensorflow*
# prebuilt libtensorflow 1.15 requires gpu cuda compute capability of 6.0
# replace tensorflow with those build with bazel.

COPY libtensorflow_framework.so.1.15.0 /src/node_modules/@tensorflow/tfjs-node-gpu/deps/lib/libtensorflow_framework.so.1.15.0
COPY libtensorflow.so.1.15.0 /src/node_modules/@tensorflow/tfjs-node-gpu/deps/lib/libtensorflow.so.1.15.0

# not needed.
#ENV LD_LIBRARY_PATH="/usr/local/lib:${LD_LIBRARY_PATH}"

# Ensure we can get enough GPU memory
# need below flag! https://stackoverflow.com/questions/38303974/tensorflow-running-error-with-cublas
ENV TF_FORCE_GPU_ALLOW_GROWTH=true

#ENTRYPOINT node /src/app.js
