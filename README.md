# pytorch-arm-docker

This is the git repo with instructions on how to build a docker image with pytorch / vision for arm64 devices with 
python2.7 and without CUDA (for now).

## Setup
### Docker
 - Install following the instructions on docker's documentation page.
For myself, I am using docker version 19.03.2 for macOSX:
https://docs.docker.com/docker-for-mac/install/

Full version information:

````
Client: Docker Engine - Community
 Version:           19.03.2
 API version:       1.40
 Go version:        go1.12.8
 Git commit:        6a30dfc
 Built:             Thu Aug 29 05:26:49 2019
 OS/Arch:           darwin/amd64
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          19.03.2
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.8
  Git commit:       6a30dfc
  Built:            Thu Aug 29 05:32:21 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.6
  GitCommit:        894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
````

- Enable experimental features for docker

Once you have your docker installed, enable experimental features following this instructions:
```
cd ~/.docker/
nano config.json
```

Then, change experimental to `"enabled"`.
```json
{
  "stackOrchestrator" : "swarm",
  "auths" : {
    "https://index.docker.io/v1/" : {

    }
  },
  "experimental" : "enabled",
  "credsStore" : "desktop",
  "HttpHeaders" : {
    "User-Agent" : "Docker-Client/19.03.2 (darwin)"
  }
}
```

This should enable the "buildx" feature, which is currently an experimental feature.

- Allow docker to use more memory / cpu power

Since we are going to be building torch from source, it requires lots of memory and computational power.
Thus, to avoid crashes while building, which may take more than 10 hours, you should allow docker to use maximal 
amount of memory as well as cpu power.

Instructions for adding memory / cpu for docker desktop:
https://stackoverflow.com/a/56583203

## Building images
- Build an image with pytorch
```
cd python27-arm64-torch-nocuda
docker buildx build --platform linux/arm64 . --tag=python27-arm64-torch-nocuda 
```
- Build another image on top of the image build on 1) with torchvision
```
cd ../python27-arm64-torchvision-nocuda
docker buildx build --platform linux/arm64 . --tag=python27-arm64-torchvision-nocuda 
```
