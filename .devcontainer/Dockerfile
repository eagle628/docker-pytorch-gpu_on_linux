#FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04
#LABEL maintainer "NVIDIA CORPORATION <cudatools@nvidia.com>"
#ENV CUDNN_VERSION 7.6.5.32
#LABEL com.nvidia.cudnn.version="${CUDNN_VERSION}"

#ARG USERNAME=vscode
#ARG USER_UID=1000
#ARG USER_GID=$USER_UID

# [Optional] Uncomment this section to install additional packages.
#
#ENV DEBIAN_FRONTEND=noninteractive
#RUN apt-get update \
#　  && apt install -y --no-install-recommends python3 python3-pip \
#    # Clean up
#    && apt-get autoremove -y \
#    && apt-get clean -y \
#    && rm -rf /var/lib/apt/lists/*
#ENV DEBIAN_FRONTEND=dialog

# [Optional] Allow the vscode user to pip install globally w/o sudo
#ENV PIP_TARGET=/usr/local/pip-global
#ENV PYTHSONPATH=${PIP_TARGET}:${PYTHONPATH}
#ENV PATH=${PIP_TARGET}/bin:${PATH}
#RUN mkdir -p ${PIP_TARGET} \
#    && chown vscode:root ${PIP_TARGET} \
#    && export SNIPPET="if [ \"\$(stat -c '%U' ${PIP_TARGET})\" != \"vscode\" ]; then chown -R vscode:root ${PIP_TARGET}; fi" \
#    && echo "$SNIPPET" | tee -a /root/.bashrc >> /home/vscode/.bashrc \
#    && echo "$SNIPPET" | tee -a /root/.zshrc >> /home/vscode/.zshrc

# [Optional] If your requirements rarely change, uncomment this section to add them to the image.

#COPY requirements.txt /tmp/pip-tmp/
#RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
#    && rm -rf /tmp/pip-tmp

#USER ${USERNAME}




FROM nvidia/cuda:10.2-cudnn7-runtime-ubuntu18.04

# Install some basic utilities
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
 && rm -rf /var/lib/apt/lists/*

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID


# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# [Optional] Uncomment this section to install additional packages.
#
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update 
RUN apt-get -y install python3 python3-pip \
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    #&& rm -rf /var/lib/apt/lists/* # どうもこれでpip3が効かなくなっていた模様。根本的な理由はわからない。
ENV DEBIAN_FRONTEND=dialog

# [Optional] Allow the vscode user to pip install globally w/o sudo
#ENV PIP_TARGET=/usr/local/pip-global
#ENV PYTHSONPATH=${PIP_TARGET}:${PYTHONPATH}
#ENV PATH=${PIP_TARGET}/bin:${PATH}
#RUN mkdir -p ${PIP_TARGET} \
#    && chown vscode:root ${PIP_TARGET} \
#    && export SNIPPET="if [ \"\$(stat -c '%U' ${PIP_TARGET})\" != \"vscode\" ]; then chown -R vscode:root ${PIP_TARGET}; fi" \
#    && echo "$SNIPPET" | tee -a /root/.bashrc >> /home/vscode/.bashrc \
#    && echo "$SNIPPET" | tee -a /root/.zshrc >> /home/vscode/.zshrc

# [Optional] If your requirements rarely change, uncomment this section to add them to the image.
#COPY requirements.txt /tmp/pip-tmp/requirements.txt
#RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
#    && rm -rf /tmp/pip-tmp
#RUN pip3 install jupyter click numpy matplotlib seaborn pandas tqdm
#RUN pip3 install torch torchvision
#RUN pip3 install -r requirements.txt

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME

# Install Miniconda and Python 3.8
#ENV CONDA_AUTO_UPDATE_CONDA=false
#ENV PATH=/home/user/miniconda/bin:$PATH
#RUN curl -sLo ~/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh \
# && chmod +x ~/miniconda.sh \
# && ~/miniconda.sh -b -p ~/miniconda \
# && rm ~/miniconda.sh \
# && conda install -y python==3.8.1 \
# && conda clean -ya

# CUDA 10.2-specific steps
#RUN conda install -y -c pytorch \
#    cudatoolkit=10.2 \
#    "pytorch=1.5.0=py3.8_cuda10.2.89_cudnn7.6.5_0" \
#    "torchvision=0.6.0=py38_cu102" \
# && conda clean -ya

# Set the default command to python3
#CMD ["python3"]