``` 
sudo apt-get install cuda-drivers-535

sudo systemctl restart docker

cd git

cd nv-ingest/

docker login nvcr.io

azureuser@nc80h100:~/git/nv-ingest$ docker login nvcr.io
Username: $oauthtoken
Password: 
WARNING! Your password will be stored unencrypted in /home/azureuser/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credential-stores

NGC_API_KEY=KEY_GOES_HERE

cat << EOF > .env
NGC_API_KEY=$NGC_API_KEY
DATASET_ROOT=/home/azureuser/git/nv-ingest/data
NV_INGEST_ROOT=/home/azureuser/git/nv-ingest
EOF


sudo nvidia-ctk runtime configure --runtime=docker --set-as-default

sudo apt update

sudo apt install docker-compose-plugin

sudo systemctl restart docker


azureuser@nc80h100:~/git/nv-ingest$ docker images
REPOSITORY                                                           TAG       IMAGE ID       CREATED         SIZE
nvcr.io/ohlfw0olaadg/ea-participants/nv-ingest                       24.10.1   f65d0e0a74bd   17 hours ago    20.5GB
nvcr.io/ohlfw0olaadg/ea-participants/nv-ingest                       24.10     8c262e7b6979   18 hours ago    19.3GB
grafana/grafana                                                      latest    c048ea6f48b7   28 hours ago    485MB
prom/prometheus                                                      latest    899b369f202e   7 days ago      290MB
nvcr.io/nim/nvidia/nv-embedqa-e5-v5                                  1.1.0     e09ea4ae9c60   2 weeks ago     10.6GB
nvcr.io/ohlfw0olaadg/ea-participants/nv-yolox-structured-images-v1   0.2.0     35bf01796705   2 weeks ago     17.2GB
nvcr.io/ohlfw0olaadg/ea-participants/cached                          0.2.0     842d709a8ade   2 weeks ago     23.6GB
nvcr.io/ohlfw0olaadg/ea-participants/paddleocr                       0.2.0     7b10de19fe72   2 weeks ago     18.7GB
nvcr.io/ohlfw0olaadg/ea-participants/nv-ingest                       <none>    95db6d665c3b   2 weeks ago     20.4GB
openzipkin/zipkin                                                    latest    3bd3d5c9013e   5 weeks ago     183MB
redis/redis-stack                                                    latest    873f484c2a62   5 weeks ago     782MB
nvcr.io/ohlfw0olaadg/ea-participants/deplot                          1.0.0     ae611b04087f   2 months ago    13.7GB
otel/opentelemetry-collector-contrib                                 0.91.0    5bbb1a619a24   11 months ago   227MB

azureuser@nc80h100:~$ docker run -it ae611b04087f bash

===========================================
== NVIDIA Inference Microservice VLM NIM ==
===========================================

Model: google/deplot

Container image Copyright (c) 2016-2024, NVIDIA CORPORATION & AFFILIATES. All rights reserved.

This NIM container is subject to the NVIDIA Evaluation License Agreement:
https://registry.ngc.nvidia.com/orgs/ohlfw0olaadg/teams/ea-participants/resources/eula.
A copy of this license can be found under /opt/nim/LICENSE.

nim@9d23ebf63a11:/$ list-model-profiles
SYSTEM INFO
- Free GPUs:
  -  [2321:10de] (0) NVIDIA H100 NVL [current utilization: 0%]
- Non-free GPUs:
  -  [2321:10de] (1) NVIDIA H100 NVL [current utilization: 30%]
MODEL PROFILES
- Compatible with system and runnable: <None>
- Incompatible with system:
  - 83bcdd737203f919b6544f0577131da0a37ec637b3d36d55ffaa9596aab8018a (a100-fp16-tp1-throughput)
  - 3e99f6d6b7b87d4eec7464a809a3d2b443beac7fd81a9c4cbb92150ce171ac48 (a10g-fp16-tp1-throughput)
  - e3f6a0dbfbf02f870c69c3dd741314df3a6c8c1a71eeb34a717f163d08ce6ffd (h100-fp16-tp1-throughput)
  - e651e1e2ac18b6a54cff1ed9336e4015292864596c33db487d24e44658c4e59d (l40s-fp16-tp1-throughput)
  - 95edb5d5290d293712cf647859bcdbf70ee8a0fd1ccd4c5276020beb99c1c795 (a100pcie-fp16-tp1-throughput)
  - aaaf16c3fe75979218c0faf5397c67148f1c7dd1fe9d8d4dcc771042bc64abc4 (h100pcie-fp16-tp1-throughput)



  deplot:
    image: ${DEPLOT_IMAGE:-nvcr.io/ohlfw0olaadg/ea-participants/deplot}:${DEPLOT_TAG:-1.0.0}
    ports:
      - "8003:8000"
      - "8004:8001"
      - "8005:8002"
    user: root
    environment:
      - NIM_HTTP_API_PORT=8000
      - NIM_TRITON_LOG_VERBOSE=1
      - NGC_API_KEY=${NIM_NGC_API_KEY:-${NGC_API_KEY:-ngcapikey}}
      - CUDA_VISIBLE_DEVICES=0
      - NIM_MANIFEST_ALLOW_UNSAFE=1
      - NIM_MODEL_PROFILE=h100-fp16-tp1-throughput 
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              device_ids: ["0"]
              capabilities: [gpu]
    runtime: nvidia

``` 