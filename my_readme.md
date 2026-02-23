## Steps to run training

0. Env setup

If the conda is used the first time on this machines, run

```
conda init bash
source ~/.bashrc
```

1. verl docker

If you are running on workstation:

```
docker create --runtime=nvidia --gpus all --net=host --shm-size="10g" --cap-add=SYS_ADMIN -v .:/workspace/verl --name verl verlai/verl:vemlp-th2.4.0-cu124-vllm0.6.3-ray2.10-v0.0.2 sleep infinity
docker start verl
docker exec -it verl bash
```

If you run on managed service like RunPod

  a. Load the same verl docker image `verlai/verl:vemlp-th2.4.0-cu124-vllm0.6.3-ray2.10-v0.0.2`

  b. Set the custom docker start cmd: `sleep infinity`

Note: this requires CUDA 12.4 and above, which requires Ubuntu 20 and above.

2. Follow the installation steps

https://github.com/lizhieffe/TinyZero/tree/main?tab=readme-ov-file#installation

3. Download model

Here the `Qwen 0.5B` model is used.

```
# Install the HF cli
pip install -U "huggingface_hub[cli]>=0.24.0,<1.0"

# Download model to local dir
huggingface-cli download Qwen/Qwen2.5-0.5B --local-dir model/Qwen2.5-0.5B
```

4. Prepare training data

```
conda activate zero
python ./examples/data_preprocess/countdown.py --local_dir data/countdown
```

Note, this is based on: https://github.com/lizhieffe/TinyZero/tree/main?tab=readme-ov-file#countdown-task

5. Start training

```
./train_0.5b_ppo.sh
```
