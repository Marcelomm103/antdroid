# Usa o Ubuntu 24.04 oficial (equivalente ao Ubuntu Server do Raspberry Pi)
FROM ubuntu:24.04

# Evita prompts interativos durante o build
ENV DEBIAN_FRONTEND=noninteractive

# Instala dependências básicas do sistema, suporte a locale e ferramentas de rede/git
RUN apt-get update && apt-get install -y \
    locales \
    curl \
    gnupg2 \
    lsb-release \
    python3-pip \
    python3-venv \
    git \
    nano \
    build-essential \
    cmake \
    && locale-gen en_US en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# Adiciona o repositório oficial do ROS 2 Jazzy
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Instala o ROS 2 Jazzy (versão base/runtime recomendada para servidores embarcados) e ferramentas de build
RUN apt-get update && apt-get install -y \
    ros-jazzy-ros-base \
    ros-jazzy-ros2-control \
    ros-jazzy-ros2-controllers \
    python3-colcon-common-extensions \
    && rm -rf /var/lib/apt/lists/*

# Configura o ambiente ROS 2 automaticamente no bash
RUN echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc

WORKDIR /workspace