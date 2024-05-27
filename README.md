# Coordinated Data Loader : CoorDL

This repository contains the source code implementation of a part of the VLDB'21 paper "Analyzing and Mitigating Data Stalls in DNN Training". This work was done as part of Microsoft Research's [Project Fiddle](https://www.microsoft.com/en-us/research/project/fiddle/). This source code is available under the MIT License.

CoorDL is built as an extension to NVIDIA's DALI data loading library to demonstrate the effectiveness of three new techniques to mitigate data stalls in DNN training; MinIO caching, partitioned caching, and coordinated data pre-processing. The main repo for this work can be found here - [DS-Analyzer](https://github.com/msr-fiddle/DS-Analyzer)

## Modifications

- Change `CUDA_IMAGE_NAME` to `nvidia/cuda:11.3.1-cudnn-devel8-ubuntu16.04`
- Update Python version from `3.6.0` to `3.6.9` for PyTorch compiling
- Add docker proxy configs

## Building CoorDL

    cd $DALI_ROOT/docker

    CREATE_RUNNER="YES" ./build.sh

This will build the source, and place the compiled wheel in the docker container nvidia/dali:py36_cu10_x86_64.build 
Next, it will create a docker container from the base base_image (This has pytorch 1.0, CUDA 10, python 3.6, cuDNN and the base dali), and install the compiled CoorDL wheel in this container.

Note that the base_image must be built using the instructions given [here](https://github.com/msr-fiddle/ds_analyzer/README.md/#setup)

The created image can be now run using : 

    nvidia-docker run --ipc=host --mount src=/,target=/datadrive/,type=bind -it --rm --network=host --cpus=24 -m 200g --privileged nvidia/dali:py36_cu10.run
