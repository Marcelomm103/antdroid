# Edge (RPi 5) image — ROS 2 Humble needs a 22.04 userland, but the Pi 5 has no
# Ubuntu 22.04 kernel support: the HOST runs Ubuntu 24.04 LTS (first Pi 5 LTS) and
# Humble runs in this container, borrowing the host kernel.
# Multi-arch base (amd64 + arm64): the exact same image serves the x86 dev PC and
# the Pi 5 — no cross-compilation setup needed.
FROM ros:humble-ros-base

# Evita prompts interativos durante o build
ENV DEBIAN_FRONTEND=noninteractive

# Locale + ferramentas básicas de desenvolvimento
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
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 \
    && rm -rf /var/lib/apt/lists/*
ENV LANG=en_US.UTF-8

# Stack ROS 2 do edge: ros2_control (loop reflexo E4), filtro Madgwick p/ IMU (E4),
# teleop por joystick (E6) e ferramentas de build/teste
RUN apt-get update && apt-get install -y \
    ros-humble-ros2-control \
    ros-humble-ros2-controllers \
    ros-humble-xacro \
    ros-humble-imu-tools \
    ros-humble-joy \
    ros-humble-teleop-twist-joy \
    python3-colcon-common-extensions \
    python3-pytest \
    && rm -rf /var/lib/apt/lists/*

# Acesso a hardware: I2C (IMU MPU6050/ICM-20948) e serial USB (bridge Arduino Mega)
RUN apt-get update && apt-get install -y \
    i2c-tools \
    libi2c-dev \
    udev \
    && rm -rf /var/lib/apt/lists/*

# Python p/ drivers e ferramentas de bancada: IMU (smbus2), protocolo serial do
# bridge (pyserial + COBS) — ver roadmap E3/E4
RUN pip3 install --no-cache-dir \
    smbus2 \
    pyserial \
    cobs

# arduino-cli + core AVR: compila o firmware antfirm_bridge do Mega 2560 (E3)
# de dentro deste mesmo container
RUN curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh \
    | BINDIR=/usr/local/bin sh \
    && arduino-cli core update-index \
    && arduino-cli core install arduino:avr

# Configura o ambiente ROS 2 automaticamente no bash
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

WORKDIR /workspace