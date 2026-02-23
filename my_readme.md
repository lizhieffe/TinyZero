# Steps to run training

> [!IMPORTANT]
> Note: this requires CUDA 12.4 and above, which requires Ubuntu 20 and above.

## **[Recommended]** Use Docker 

## No Docker

### verl docker

#### If you are running on workstation:

```bash
docker create --runtime=nvidia --gpus all --net=host --shm-size="10g" --cap-add=SYS_ADMIN -v .:/workspace/verl --name verl verlai/verl:vemlp-th2.4.0-cu124-vllm0.6.3-ray2.10-v0.0.2 sleep infinity
docker start verl
docker exec -it verl bash
```

#### If you run on managed service like RunPod

- Load the same verl docker image `verlai/verl:vemlp-th2.4.0-cu124-vllm0.6.3-ray2.10-v0.0.2`

- Set the custom docker start cmd: `sleep infinity`

### Download source

```bash
git clone https://github.com/lizhieffe/TinyZero.git
cd TinyZero
```

### Enter Conda

> [!NOTE]
> If the conda is used the first time on this machines, run
> ```bash
> conda init bash
> source ~/.bashrc
> ```

```bash
conda create -n zero python=3.9
conda activate zero
```

### Follow the installation steps

```bash
# install torch [or you can skip this step and let vllm to install the correct version for you]
pip install torch==2.4.0 --index-url https://download.pytorch.org/whl/cu121
# install vllm
pip3 install vllm==0.6.3 # or you can install 0.5.4, 0.4.2 and 0.3.1
pip3 install ray

# verl
pip install -e .

# flash attention 2
pip3 install flash-attn --no-build-isolation
# quality of life
pip install wandb IPython matplotlib
```

> [!NOTE]
> Based on: https://github.com/lizhieffe/TinyZero/tree/main?tab=readme-ov-file#installation

### Prepare training data

```bash
python ./examples/data_preprocess/countdown.py --local_dir data/countdown
```

> [!NOTE]
> Based on: https://github.com/lizhieffe/TinyZero/tree/main?tab=readme-ov-file#countdown-task

### Download model

Here the `Qwen 0.5B` model is used.

```bash
# Install the HF cli
pip install -U "huggingface_hub[cli]>=0.24.0,<1.0"

# Download model to local dir
huggingface-cli download Qwen/Qwen2.5-0.5B --local-dir model/Qwen2.5-0.5B
```

### Start training

```bash
./train_0.5b_ppo.sh
```
