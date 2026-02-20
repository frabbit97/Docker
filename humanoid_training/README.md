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
```conda create -y -n unitree_lerobot python=3.10 ```

aggiungere nel bashrc

```bash 
# Source Conda's shell script so 'conda activate' funziona
if [ -f "/opt/conda/etc/profile.d/conda.sh" ]; then
    . /opt/conda/etc/profile.d/conda.sh
fi

# Attiva automaticamente l'ambiente unitree_lerobot
conda activate unitree_lerobot
```
## 4️⃣ Installazione unitreeLeRobot (non per gr00t)

Una volta creato l'environment continuare a seguire la guida di LeRobot di unitree punto 1.[Unitree IL LeRobot](https://github.com/unitreerobotics/unitree_IL_lerobot)
prima di installare leRobot, installare manualmente evdev (Nvidia Spark)

```bash 
$ sudo python3 -m pip install evdev
```
## 5️⃣ Download dataset

Per scaricare il dataset è necessario prima di tutto aggiornare scipy in conda quindi entrare nell'ambiente conda ed eseguire questo comando

```bash 
pip install --force-reinstall --no-cache-dir scipy
```
## 6️⃣ Gr00t
per gr00t abbiamo necessità di seguire questa guida [Gr00t](https://github.com/huggingface/lerobot/blob/main/docs/source/groot.mdx).


Per questo comando ```pip install "torch>=2.2.1,<2.8.0" "torchvision>=0.21.0,<0.23.0" # --index-url https://download.pytorch.org/whl/cu1XX```
ricordarsi di vedere che CUDA è montato sulla macchina.

Sulle architerrure arm questo comando ```pip install "flash-attn>=2.5.9,<3.0.0" --no-build-isolation``` non funziona, sulle spark è possibile scaricare questo whl ed installarlo:
ATTENZIONE! se non hai architettura arm puoi trovare il tuo file qui: https://flashattn.dev/
```bash 
pip install gdown 
gdown "https://drive.google.com/uc?id=1UYfkEWSMoBxx8rp4vC-LfIrpu_vVlUx5"
python3 -m pip install "file_name.whl"
```
oppure 
```bash 
MAX_JOBS=4 pip install flash-attn --no-build-isolation
```



