FROM nvidia/cuda:10.1-base-ubuntu16.04

# Install some basic utilities
RUN ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo Asia/Shanghai > /etc/timezone

RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    openssh-server \
    vim \
    htop \
    build-essential \
    tzdata \
 && rm -rf /var/lib/apt/lists/*
RUN dpkg-reconfigure --frontend noninteractive tzdata

# Create a working directory
RUN mkdir /workspace
WORKDIR /workspace

# Install Miniconda
RUN curl -o /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /root/miniconda.sh \
 && /root/miniconda.sh -b -p /root/miniconda \
 && rm /root/miniconda.sh
ENV PATH=/root/miniconda/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Create a Python 3.6 environment
RUN ~/miniconda/bin/conda create -y --name py36 python=3.6.9 \
 && ~/miniconda/bin/conda clean -ya
ENV CONDA_DEFAULT_ENV=py36
ENV CONDA_PREFIX=/root/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
RUN /root/miniconda/bin/conda install conda-build=3.18.11=py36_0 \
 && /root/miniconda/bin/conda clean -ya

# CUDA 10.0-specific steps
RUN conda install -y -c pytorch \
    cudatoolkit=10.1 \
    "pytorch=1.4.0=py3.6_cuda10.1.243_cudnn7.6.3_0" \
 && conda clean -ya

# Install HDF5 Python bindings
RUN conda install -y h5py matplotlib tqdm scikit-learn gensim \
 && conda clean -ya
RUN pip install h5py-cache torchtext configargparse

# Install Requests, a Python library for making HTTP requests
RUN conda install -y requests=2.19.1 \
 && conda clean -ya

# Set the default command to python3
CMD ["python3"]
