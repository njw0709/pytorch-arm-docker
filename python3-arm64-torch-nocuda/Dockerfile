# Use python2.7 image for arm64 architecture
FROM arm64v8/python:3-slim
#install requirements for torch
RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         git \
         curl \
         ca-certificates \
         libjpeg-dev \
         libpng-dev && \
     rm -rf /var/lib/apt/lists/*
ENV NO_CUDA=1
ENV NO_DISTRIBUTED=1
ENV NO_MKLDNN=1
ENV BUILD_TEST=0

WORKDIR /app
# clone repo and build torch
RUN git clone https://github.com/pytorch/pytorch.git && cd ./pytorch && git submodule update --init --recursive && \
    pip install -r requirements.txt && python -E setup.py build

# copy built modules to site packages (for some reason, the built modules were not copied to site packages)
RUN cp -r /app/pytorch/build/lib.linux-aarch64-2.7/* /usr/local/lib/python2.7/site-packages/

# clean up
RUN rm -rf /app
