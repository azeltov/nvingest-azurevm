# Install pre-requisites

## Setup cuda-drivers:
``` 
sudo apt update

sudo apt-get install cuda-drivers-535

sudo systemctl restart docker

azureuser@nc80h100:~/git/nv-ingest$ nvidia-smi
Wed Nov 13 17:30:03 2024       
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.216.01             Driver Version: 535.216.01   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA H100 NVL                On  | 00000001:00:00.0 Off |                    0 |
| N/A   41C    P0              97W / 400W |  75079MiB / 95830MiB |      0%      Default |
|                                         |                      |             Disabled |
+-----------------------------------------+----------------------+----------------------+
|   1  NVIDIA H100 NVL                On  | 00000002:00:00.0 Off |                    0 |
| N/A   40C    P0              95W / 400W |  29814MiB / 95830MiB |      0%      Default |
|                                         |                      |             Disabled |
+-----------------------------------------+----------------------+----------------------+


```

## Install nvidia-ctk runtime for docker
```
sudo nvidia-ctk runtime configure --runtime=docker --set-as-default
```
## Install docker-compose

```
sudo apt install docker-compose-plugin

sudo systemctl restart docker
```
