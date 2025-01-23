## Get Involved

OMNI is **experimental** software. Expect bugs early on. Create issues so they can be fixed. The team will strive to resolve issues quickly.

## Features

### Wide Model Support

OMNI supports different models including LLaMA ([MLX](exo/inference/mlx/models/llama.py) and [tinygrad](exo/inference/tinygrad/models/llama.py)), Mistral, LlaVA, Qwen, and Deepseek.

### Dynamic Model Partitioning

OMNI [optimally splits up models](exo/topology/ring_memory_weighted_partitioning_strategy.py) based on the current network topology and device resources available. This enables you to run larger models than you would be able to on any single device.

### Automatic Device Discovery

OMNI will [automatically discover](https://github.com/exo-explore/exo/blob/945f90f676182a751d2ad7bcf20987ab7fe0181e/exo/orchestration/node.py#L154) other devices using the best method available. Zero manual configuration.

### ChatGPT-compatible API

OMNI provides a [ChatGPT-compatible API](exo/api/chatgpt_api.py) for running models. It's a [one-line change](examples/chatgpt_api.sh) in your application to run models on your own hardware using exo.

### Device Equality

Unlike other distributed inference frameworks, exo does not use a master-worker architecture. Instead, OMNI devices As long as a device is connected somewhere in the network, it can be used to run models.

OMNI supports different [partitioning strategies](exo/topology/partitioning_strategy.py) to split up a model across devices. The default partitioning strategy is [ring memory weighted partitioning](exo/topology/ring_memory_weighted_partitioning_strategy.py). This runs an inference in a ring where each device runs a number of model layers proportional to the memory of the device.

!["A screenshot of exo running 5 nodes](docs/exo-screenshot.jpg)

## Installation

The current recommended way to install exo is from source.

### Prerequisites

- Python>=3.12.0 is required because of [issues with asyncio](https://github.com/exo-explore/exo/issues/5) in previous versions.
- For Linux with NVIDIA GPU support (Linux-only, skip if not using Linux or NVIDIA):
  - NVIDIA driver - verify with `nvidia-smi`
  - CUDA toolkit - install from [NVIDIA CUDA guide](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#cuda-cross-platform-installation), verify with `nvcc --version`
  - cuDNN library - download from [NVIDIA cuDNN page](https://developer.nvidia.com/cudnn-downloads), verify installation by following [these steps](https://docs.nvidia.com/deeplearning/cudnn/latest/installation/linux.html#verifying-the-install-on-linux:~:text=at%20a%20time.-,Verifying%20the%20Install%20on%20Linux,Test%20passed!,-Upgrading%20From%20Older)

### Hardware Requirements

- The only requirement to run exo is to have enough memory across all your devices to fit the entire model into memory. For example, if you are running llama 3.1 8B (fp16), you need 16GB of memory across all devices. Any of the following configurations would work since they each have more than 16GB of memory in total:
  - 2 x 8GB M3 MacBook Airs
  - 1 x 16GB NVIDIA RTX 4070 Ti Laptop
  - 2 x Raspberry Pi 400 with 4GB of RAM each (running on CPU) + 1 x 8GB Mac Mini
- exo is designed to run on devices with heterogeneous capabilities. For example, you can have some devices with powerful GPUs and others with integrated GPUs or even CPUs. Adding less capable devices will slow down individual inference latency but will increase the overall throughput of the cluster.


```

