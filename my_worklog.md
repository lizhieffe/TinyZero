## 2026/02/21

Use verl docker image to setup the env. [Instruction](https://verl.readthedocs.io/en/latest/start/install.html#install-from-docker-image)

```
docker create --runtime=nvidia --gpus all --net=host --shm-size="10g" --cap-add=SYS_ADMIN -v .:/workspace/verl --name verl verlai/verl:vemlp-th2.4.0-cu124-vllm0.6.3-ray2.10-v0.0.2 sleep infinity
docker start verl
docker exec -it verl bash
```

It errors with CUDA env too old. It requres CUDA 12.4, but my Ubuntu workstation uses 12.1. The workstation is Ubuntu 18 which is too old to install CUDA 12.4.

AI:
- Use a newer Ubuntu to try again this approach.
