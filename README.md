# Deploying DeepSeek on CPU with 1.58-bit Quantized Model

## `Installing Ollama`

```sh
curl -fsSL https://ollama.com/install.sh -o ollama_install.sh
chmod +x ollama_install.sh
sed -i 's|https://ollama.com/download/|https://github.com/ollama/ollama/releases/download/v0.5.7/|' ollama_install.sh
sh ollama_install.sh
sudo systemctl start ollama
```

## `Pulling the Model`

```sh
ollama pull SIGJNF/deepseek-r1-671b-1.58bit
pulling manifest
```

## `Configuring the Model File`

```sh
vim cpu.modelfile
```

### Add the following content:

```txt
FROM SIGJNF/deepseek-r1-671b-1.58bit:latest

PARAMETER num_gpu 0
SYSTEM """A conversation between User and Assistant. The user asks a question, and the Assistant solves it. The assistant first thinks about the reasoning process in the mind and then provides the user with the answer. The reasoning process and answer are enclosed within <think> </think> and <answer> </answer> tags, respectively, i.e., <think> reasoning process here </think><answer> answer here </answer>"""
```

## `Creating the Model`

```sh
ollama create SIGJNF/deepseek-r1-671b-1.58bit:cpu -f cpu.modelfile
```

## `Additional Configurations`

```sh
# Disable GPU-related settings
unset GGML_CUDA_ENABLE_UNIFIED_MEMORY
unset OLLAMA_GPU_OVERHEAD
unset OLLAMA_FLASH_ATTENTION

# Set loading timeout
export OLLAMA_LOAD_TIMEOUT=360m
```

## `Starting Ollama (If Not Already Running)`

```sh
ollama serve
```

## `Running the Model`

```sh
ollama run SIGJNF/deepseek-r1-671b-1.58bit:cpu
