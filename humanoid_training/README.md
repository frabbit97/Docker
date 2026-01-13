# Docker Humanoid Training Environment

Questo progetto fornisce un container Docker con:

- Desktop environment (per RViz o altre GUI)
- Conda
- CUDA GPU support
- Network access
- Cartella condivisa per i tuoi progetti

---

## Requisiti

- Docker 23+ con supporto GPU (`--gpus all`)
- Driver NVIDIA corretti installati sul host
- GPU ARM64 (es. Jetson, Ampere ARM64 server)
- X11 server sul host per visualizzare GUI (Linux)

---
## 1️⃣ Build dell’immagine Docker

Da dentro la cartella del Dockerfile, eseguire:

```bash
docker build -t humanoid_training .
```
- -t humanoid_training → assegna il nome all’immagine
- . → usa il Dockerfile nella directory corrente
## 2️⃣ Eseguire il container
Per avere accesso al desktop tramite X11:
```bash
xhost +local:docker  # consenti connessione X11 al container

docker run -it \
    --gpus all \
    -v /home/humanoid_area/humanoid_training:/root/project/humanoid_training \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    --network host \
    humanoid_training \
    /bin/bash
```
## 3️⃣ Conda e ambiente
```bash conda create -y -n unitree_lerobot python=3.10 ```

aggiungere nel bashrc
```bash 
# Source Conda's shell script so 'conda activate' funziona
if [ -f "/opt/conda/etc/profile.d/conda.sh" ]; then
    . /opt/conda/etc/profile.d/conda.sh
fi

# Attiva automaticamente l'ambiente unitree_lerobot
conda activate unitree_lerobot```

