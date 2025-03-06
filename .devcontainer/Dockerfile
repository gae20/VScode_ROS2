FROM nvidia/cuda:12.2.0-devel-ubuntu22.04

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y nano git git-lfs python3-pip wget python3-venv

WORKDIR /home/workspace/

ENV TORCH_CUDA_ARCH_LIST="8.6"

RUN git clone https://github.com/francylac1999/llm-awq.git
RUN pip install --upgrade pip setuptools
RUN cd /home/workspace/llm-awq/ && git checkout -B inferenza1 2170d00
RUN cd /home/workspace/llm-awq/ && pip install -e .
RUN cd /home/workspace/llm-awq/awq/kernels/ && python3 setup.py install
RUN git clone https://github.com/francylac1999/VILA.git
RUN cd /home/workspace/VILA/ && git checkout -B inferenza1 74f92b1
RUN cd /home/workspace/VILA/ && pip install -e ".[train,eval]"
RUN cp -rv /home/workspace/VILA/llava/train/deepspeed_replace/* $(python3 -c 'import site; print(site.getsitepackages()[0])')/deepspeed/
RUN pip install protobuf==3.20.*
RUN git-lfs clone https://huggingface.co/Efficient-Large-Model/VILA1.5-3b-AWQ
RUN wget -O /home/workspace/llm-awq/figures/Gyrandos.png https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/130.png
RUN pip install flash-attn --no-build-isolation
RUN pip install triton==3.1.0
RUN pip install pipx && pipx install termvisage
ENV PATH=$PATH:/root/.local/bin/
RUN apt update && apt install locales
RUN locale-gen en_US en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN export LANG=en_US.UTF-8
RUN apt install software-properties-common -y
RUN add-apt-repository universe
RUN apt update && apt install curl -y
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt update
RUN apt upgrade -y
ENV DEBIAN_FRONTEND=noninteractive
RUN apt install ros-humble-desktop -y
RUN apt install ros-dev-tools -y
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
