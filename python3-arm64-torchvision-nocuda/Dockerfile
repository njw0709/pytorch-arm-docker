# from torch image
FROM python3-arm64-torch-nocuda
# copying vision source code (for some reason, it does not detect torch built from source, so it has been taken out in
# requirements to be pre-installed.
WORKDIR /app
COPY . /app
# install torchvision
RUN cd vision && python setup.py install
# clean up
RUN rm -rf