# Base: CUDA + Python 3.10 + Conda
FROM nvidia/cuda:12.2.0-runtime-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive

# -----------------------------
# Install essentials: python, conda, desktop
# -----------------------------
RUN apt-get update && apt-get install -y \
    wget \
    git \
    sudo \
    xfce4 \
    xfce4-goodies \
    dbus-x11 \
    x11-xserver-utils \
    && rm -rf /var/lib/apt/lists/*

# Install miniconda
# Esempio per ARM64
RUN wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-aarch64.sh -O /tmp/miniforge.sh && \
    bash /tmp/miniforge.sh -b -p /opt/conda && \
    rm /tmp/miniforge.sh

ENV PATH=/opt/conda/bin:$PATH

# -----------------------------
# Setup for GUI apps (RViz)
# -----------------------------
RUN echo '#!/bin/bash\nunset SESSION_MANAGER\nunset DBUS_SESSION_BUS_ADDRESS\nexec startxfce4 &' \
    > /root/.xinitrc && chmod +x /root/.xinitrc

# -----------------------------
# Default command: start bash in conda env
# -----------------------------
CMD ["bash"]
