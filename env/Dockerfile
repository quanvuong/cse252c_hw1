ARG UBUNTU_VERSION=16.04
ARG ARCH=
ARG CUDA=10.1

FROM nvidia/cudagl${ARCH:+-$ARCH}:${CUDA}-base-ubuntu${UBUNTU_VERSION} as base
# ARCH and CUDA are specified again because the FROM directive resets ARGs
# (but their default value is retained if set previously)

ARG UBUNTU_VERSION
ARG ARCH
ARG CUDA
ARG CUDNN=7.4.1.5-1

SHELL ["/bin/bash", "-c"]

ENV DEBIAN_FRONTEND="noninteractive"
# See http://bugs.python.org/issue19846
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
        libglib2.0-0 libxext6 libsm6 libxrender1 \
        git mercurial subversion

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
        /bin/bash /tmp/miniconda.sh -b -p /opt/conda && \
        rm /tmp/miniconda.sh && \
        ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
        echo ". /opt/conda/etc/profile.d/conda.sh" >> /etc/bash.bashrc

RUN apt-get install -y curl grep sed dpkg && \
        TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
        curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
        dpkg -i tini.deb && \
        rm tini.deb && \
        apt-get clean \
        && rm -rf /var/lib/apt/lists/*


RUN conda update -y --name base conda \
        && conda clean --all -y

# ========== Conda Environment ==========
COPY ./environment.yml /tmp/environment.yml
COPY ./requirements.txt /tmp/requirements.txt

RUN conda env update -f /tmp/environment.yml 
RUN conda clean --all -y 
