
## Deploy nv-ingest :

Follow full steps from nv-ingest repo: https://github.com/NVIDIA/nv-ingest/blob/main/README.md


steps:
```
git clone https://github.com/NVIDIA/nv-ingest.git

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

docker compose up

```
## troubleshooting
You might see that deplot service does not come . You get an error: Could not find a profile that is currently runnable with the detected hardware. Please check the system information below and make sure you have enough free GPUs.

```
(nvingest) azureuser@nc80h100:~/git/nv-ingest$ docker compose logs -f deplot
deplot-1  | 
deplot-1  | ===========================================
deplot-1  | == NVIDIA Inference Microservice VLM NIM ==
deplot-1  | ===========================================
deplot-1  | 
deplot-1  | Model: google/deplot
deplot-1  | 
deplot-1  | Container image Copyright (c) 2016-2024, NVIDIA CORPORATION & AFFILIATES. All rights reserved.
deplot-1  | 
deplot-1  | This NIM container is subject to the NVIDIA Evaluation License Agreement:
deplot-1  | https://registry.ngc.nvidia.com/orgs/ohlfw0olaadg/teams/ea-participants/resources/eula.
deplot-1  | A copy of this license can be found under /opt/nim/LICENSE.
deplot-1  | 
deplot-1  | INFO 11-12 22:55:02.833 ngc_profile.py:222] Running NIM without LoRA. Only looking for compatible profiles that do not support LoRA.
deplot-1  | INFO 11-12 22:55:02.833 ngc_profile.py:224] Detected 0 compatible profile(s).
deplot-1  | ERROR 11-12 22:55:02.833 utils.py:21] Could not find a profile that is currently runnable with the detected hardware. Please check the system information below and make sure you have enough free GPUs.
deplot-1  | SYSTEM INFO
deplot-1  | - Free GPUs:
deplot-1  |   -  [2321:10de] (0) NVIDIA H100 NVL [current utilization: 0%]
deplot-1  | 
deplot-1  | ===========================================
deplot-1  | == NVIDIA Inference Microservice VLM NIM ==
deplot-1  | ===========================================
deplot-1  | 
deplot-1  | Model: google/deplot
deplot-1  | 
deplot-1  | Container image Copyright (c) 2016-2024, NVIDIA CORPORATION & AFFILIATES. All rights reserved.
deplot-1  | 
deplot-1  | This NIM container is subject to the NVIDIA Evaluation License Agreement:
deplot-1  | https://registry.ngc.nvidia.com/orgs/ohlfw0olaadg/teams/ea-participants/resources/eula.
deplot-1  | A copy of this license can be found under /opt/nim/LICENSE.
deplot-1  | 
deplot-1  | INFO 11-12 23:17:49.38 ngc_profile.py:222] Running NIM without LoRA. Only looking for compatible profiles that do not support LoRA.
deplot-1  | INFO 11-12 23:17:49.38 ngc_profile.py:224] Detected 0 compatible profile(s).
deplot-1  | ERROR 11-12 23:17:49.38 utils.py:21] Could not find a profile that is currently runnable with the detected hardware. Please check the system information below and make sure you have enough free GPUs.
deplot-1  | SYSTEM INFO
deplot-1  | - Free GPUs:
deplot-1  |   -  [2321:10de] (0) NVIDIA H100 NVL [current utilization: 0%]
```

get the deplot image id , using cmd: docker images
```


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

```

bash into deplot container and get the list of model profiles: list-model-profiles
```
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


```

modify docker-compose.yaml file to include NIM_MODEL_PROFILE=h100-fp16-tp1-throughput 
```
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

after you do docker compose up, you should see deplot service:
```
azureuser@nc80h100:~/git/nv-ingest$ docker ps
CONTAINER ID   IMAGE                                                                      COMMAND                  CREATED             STATUS                       PORTS                                                                                                                                   NAMES
284668bd6a12   nvcr.io/ohlfw0olaadg/ea-participants/deplot:1.0.0                          "/opt/nvidia/nvidia_…"   About an hour ago   Up About an hour             0.0.0.0:8003->8000/tcp, [::]:8003->8000/tcp, 0.0.0.0:8004->8001/tcp, [::]:8004->8001/tcp, 0.0.0.0:8005->8002/tcp, [::]:8005->8002/tcp   nv-ingest-deplot-1
87d75c98c599   nvcr.io/ohlfw0olaadg/ea-participants/nv-ingest:24.10.1                     "/opt/conda/envs/nv_…"   18 hours ago        Up About an hour (healthy)   0.0.0.0:7670->7670/tcp, :::7670->7670/tcp                                                                                               nv-ingest-nv-ingest-ms-runtime-1
e4af20cb91ca   nvcr.io/ohlfw0olaadg/ea-participants/cached:0.2.0                          "/opt/nvidia/nvidia_…"   19 hours ago        Up About an hour             0.0.0.0:8006->8000/tcp, [::]:8006->8000/tcp, 0.0.0.0:8007->8001/tcp, [::]:8007->8001/tcp, 0.0.0.0:8008->8002/tcp, [::]:8008->8002/tcp   nv-ingest-cached-1
f12ffac6c357   nvcr.io/ohlfw0olaadg/ea-participants/paddleocr:0.2.0                       "/opt/nvidia/nvidia_…"   19 hours ago        Up About an hour             0.0.0.0:8009->8000/tcp, [::]:8009->8000/tcp, 0.0.0.0:8010->8001/tcp, [::]:8010->8001/tcp, 0.0.0.0:8011->8002/tcp, [::]:8011->8002/tcp   nv-ingest-paddle-1
078aa8709cba   nvcr.io/nim/nvidia/nv-embedqa-e5-v5:1.1.0                                  "/opt/nvidia/nvidia_…"   19 hours ago        Up About an hour             0.0.0.0:8012->8000/tcp, [::]:8012->8000/tcp, 0.0.0.0:8013->8001/tcp, [::]:8013->8001/tcp, 0.0.0.0:8014->8002/tcp, [::]:8014->8002/tcp   nv-ingest-embedding-1
8a24a1ff30ed   nvcr.io/ohlfw0olaadg/ea-participants/nv-yolox-structured-images-v1:0.2.0   "/opt/nvidia/nvidia_…"   19 hours ago        Up About an hour             0.0.0.0:8000-8002->8000-8002/tcp, :::8000-8002->8000-8002/tcp                                                                           nv-ingest-yolox-1
980685edd240   openzipkin/zipkin                                                          "start-zipkin"           19 hours ago        Up About an hour (healthy)   9410/tcp, 0.0.0.0:9411->9411/tcp, :::9411->9411/tcp                                                                                     nv-ingest-zipkin-1
422975fedaaa   redis/redis-stack                                                          "/entrypoint.sh"         19 hours ago        Up About an hour             0.0.0.0:6379->6379/tcp, :::6379->6379/tcp, 8001/tcp                                                                                     nv-ingest-redis-1
69a3deda6508   grafana/grafana                                                            "/run.sh"                19 hours ago        Up About an hour             0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                                                               grafana-service

```

## Test nv-ingest :

After successfully installing the nv-ingest client, run the following CLI command to test multimodal PDF extraction, or you can also run it using the Python client based on the example here.

https://github.com/NVIDIA/nv-ingest/blob/main/client/client_examples/examples/python_client_usage.ipynb

```
  nv-ingest-cli \
        --doc /path/to/your/unique.pdf \
        --output_directory ./path/to/your/output_foler \
        --task='extract:{"document_type": "pdf", "extract_text": true, "extract_images": true, "extract_tables": true}' \
        --client_host=localhost \
        --client_port=7670

 ```
successfull output:
 ```
 azureuser@nc80h100:~/git/nv-ingest$ nv-ingest-cli   --doc /home/azureuser/git/nv-ingest/data/multimodal_test.pdf   --output_directory ./processed_docs   --task='extract:{"document_type": "pdf", "extract_method": "pdfium", "extract_tables": "true", "extract_images": "true"}'   --client_host=localhost   --client_port=7670
/home/azureuser/.local/lib/python3.10/site-packages/pydantic/main.py:390: UserWarning: Pydantic serializer warnings:
  Expected `bool` but got `tuple` with value `(True,)` - serialized value may not be as expected
  return self.__pydantic_serializer__.to_python(
INFO:nv_ingest_client.nv_ingest_cli:Processing 1 documents.
INFO:nv_ingest_client.nv_ingest_cli:Output will be written to: ./processed_docs
Processing files: 100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:06<00:00,  6.64s/file, pages_per_sec=0.45]
INFO:nv_ingest_client.cli.util.processing:caption_ext: Avg: 0.30 ms, Median: 0.30 ms, Total Time: 0.30 ms, Total % of Trace Computation: 0.00%
INFO:nv_ingest_client.cli.util.processing:caption_ext_channel_in: Avg: 0.80 ms, Median: 0.80 ms, Total Time: 0.80 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:chart_data_extraction: Avg: 5185.37 ms, Median: 5185.37 ms, Total Time: 5185.37 ms, Total % of Trace Computation: 72.89%
INFO:nv_ingest_client.cli.util.processing:chart_data_extraction_channel_in: Avg: 0.63 ms, Median: 0.63 ms, Total Time: 0.63 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:dedup_images: Avg: 0.65 ms, Median: 0.65 ms, Total Time: 0.65 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:dedup_images_channel_in: Avg: 0.71 ms, Median: 0.71 ms, Total Time: 0.71 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:docx_content_extractor: Avg: 0.83 ms, Median: 0.83 ms, Total Time: 0.83 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:docx_content_extractor_channel_in: Avg: 0.92 ms, Median: 0.92 ms, Total Time: 0.92 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:filter_images: Avg: 0.73 ms, Median: 0.73 ms, Total Time: 0.73 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:filter_images_channel_in: Avg: 0.93 ms, Median: 0.93 ms, Total Time: 0.93 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:job_counter: Avg: 0.60 ms, Median: 0.60 ms, Total Time: 0.60 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:job_counter_channel_in: Avg: 0.14 ms, Median: 0.14 ms, Total Time: 0.14 ms, Total % of Trace Computation: 0.00%
INFO:nv_ingest_client.cli.util.processing:metadata_injection: Avg: 7.00 ms, Median: 7.00 ms, Total Time: 7.00 ms, Total % of Trace Computation: 0.10%
INFO:nv_ingest_client.cli.util.processing:metadata_injection_channel_in: Avg: 0.04 ms, Median: 0.04 ms, Total Time: 0.04 ms, Total % of Trace Computation: 0.00%
INFO:nv_ingest_client.cli.util.processing:pdf_content_extractor: Avg: 336.55 ms, Median: 131.47 ms, Total Time: 1682.74 ms, Total % of Trace Computation: 23.65%
INFO:nv_ingest_client.cli.util.processing:pdf_content_extractor_channel_in: Avg: 0.24 ms, Median: 0.24 ms, Total Time: 0.24 ms, Total % of Trace Computation: 0.00%
INFO:nv_ingest_client.cli.util.processing:pptx_content_extractor: Avg: 0.55 ms, Median: 0.55 ms, Total Time: 0.55 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:pptx_content_extractor_channel_in: Avg: 0.73 ms, Median: 0.73 ms, Total Time: 0.73 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:redis_source_network_in: Avg: 11.97 ms, Median: 11.97 ms, Total Time: 11.97 ms, Total % of Trace Computation: 0.17%
INFO:nv_ingest_client.cli.util.processing:redis_task_sink_channel_in: Avg: 1.21 ms, Median: 1.21 ms, Total Time: 1.21 ms, Total % of Trace Computation: 0.02%
INFO:nv_ingest_client.cli.util.processing:redis_task_source: Avg: 3.71 ms, Median: 3.71 ms, Total Time: 3.71 ms, Total % of Trace Computation: 0.05%
INFO:nv_ingest_client.cli.util.processing:table_data_extraction: Avg: 212.25 ms, Median: 212.25 ms, Total Time: 212.25 ms, Total % of Trace Computation: 2.98%
INFO:nv_ingest_client.cli.util.processing:table_data_extraction_channel_in: Avg: 0.78 ms, Median: 0.78 ms, Total Time: 0.78 ms, Total % of Trace Computation: 0.01%
INFO:nv_ingest_client.cli.util.processing:No unresolved time detected. Trace times account for the entire elapsed duration.
INFO:nv_ingest_client.cli.util.processing:Processed 1 files in 6.64 seconds.
INFO:nv_ingest_client.cli.util.processing:Total pages processed: 3
INFO:nv_ingest_client.cli.util.processing:Throughput (Pages/sec): 0.45
INFO:nv_ingest_client.cli.util.processing:Throughput (Files/sec): 0.15
```
