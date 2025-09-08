# Llama models
1. Llama 3 models: https://github.com/meta-llama/llama3/blob/main/MODEL_CARD.md
2. Llama 3 recepies: https://github.com/meta-llama/llama-recipes

## Explanation of models:
1. Meta-Llama-3-8B: These models are not finetuned for chat or Q&A. They should be prompted so that the expected answer is the natural continuation of the prompt.
2. Meta-Llama-3-8B-Instruct: The fine-tuned models were trained for dialogue applications. To get the expected features and performance for them, a specific formatting defined in ChatFormat needs to be followed: The prompt begins with a <|begin_of_text|> special token, after which one or more messages follow. Each message starts with the <|start_header_id|> tag, the role system, user or assistant, and the <|end_header_id|> tag. After a double newline \n\n the contents of the message follow. The end of each message is marked by the <|eot_id|> token.

# Linux GPU CUDA installation
Depending on whether your GPU drivers are installed or not, you may skip this section.

- Go to Ubuntu Update and install Nvidia CUDA drivers for [GeForce RTX 2070 SUPER]

Nvidia drivers come with CUDA API and CUDA Runtime versions, which may be different at a time. As I understand, when you install Nvidia Toolkit, it installs CUDA Runtime.  However, Pytorch is shipped with its own CUDA Runtime.  Therefore, I am not quite sure whether you actually need to install Nvidia Cuda Toolkit or not. If you need to install the toolkit, you may do it as follows:
`sudo apt install nvidia-cuda-toolkit`

- Now, you can run the following commands to check the status.  Note that you will often detect two different versions. The explanation is provided in the links below:
```
nvcc --version
nvidia-smi
```

See why the above tools report different versions her:
- https://i.stack.imgur.com/ruMI3.png
- https://stackoverflow.com/questions/53422407/different-cuda-versions-shown-by-nvcc-and-nvidia-smi
- https://discuss.pytorch.org/t/questions-on-cudatoolkit-versions/142022: "The pip wheels and conda binaries ship with their own CUDA runtime (as specified during the installation), so you would only need a corresponding NVIDIA driver to run your code."

# Pytorch installation
The following guide gives a good for installtion of Pytorch. However, note that you may need another solution (as it is case for Unsloth)
- https://mct-master.github.io/machine-learning/2023/04/25/olivegr-pytorch-gpu.html


# Python and dependencies
- Go on and install poetry and torch as follows:
```
venv 3.12 # My own shell function
pip install --upgrade pip
pip install pytest
pip install poetry
poetry init (if you have initialized and have a Poetry project file)
poetry install --no-root (if you have a project file)
```

# Cuda drivers
See https://pytorch.org/get-started/locally/.  The following method is proven to work.  But as mentioned before, you may need another solution (as it is case for Unsloth) :
 - either run manually: `poetry run pip install torch`
 - or add to the configuration file: `poetry add torch`

See installtion of Unsloth before for more information.

# Running models with Ollama
 See https://pytorch.org/get-started/locally/

 If your machine has CUDA (Nvidia compatible GPU), the following method is proven to work:
 - either run manually: `poetry run pip install torch`
 - or add to the configuration file: `poetry add torch`

 On a MacBook, which does not have CUDA, you need to run:
 `pip3 install torch torchvision torchaudio`


# Ollama
Ollama is a solution that can download and run various models (e.g. Llama, Phi, Gemma) in a consistent way.
See: https://github.com/ollama/ollama

See this video: https://www.youtube.com/watch?v=i-txsBoTJtI&t=1s

Depending on whether you use Mac or Linux, you can run the following commands:
- Mac: `brew install ollama`
- Linux: `curl -fsSL https://ollama.com/install.sh | sh`

Run ollama server manually
```
ollama serve
```

Then run one of the following to download various models:
```
1. ollama run llama3
2. ollama run llama3:instruct
3. ollama run llama3:text
```

## Overview of Ollama models:
https://github.com/ollama/ollama/blob/main/README.md#quickstart

## Ollama as service
To start ollama now and restart at login:

### Mac: 
```
brew services start ollama
```

Or, if you don't want/need a background service you can just run:
```
  /usr/local/opt/ollama/bin/ollama serve
```

You will then get an output like this:
```
==> Running `brew cleanup ollama`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`). -->
```

### Linux
The above `install.sh` makes it a Linux daemon/service
In Linux, Ollama installs the service as `/etc/systemd/system/ollama.service`
You can see status, start or stop the service with:
```
sudo systemctl status ollama
sudo systemctl start ollama
sudo systemctl stop ollama
```

## Where are Ollama models stored?
/usr/share/ollama/.ollama

## Ollama with Python
Having run ollama server, you can interact with it with Python
See: https://github.com/ollama/ollama-python

### Using pip
```
- pip install ollama
- pip install llama-recipes
```
### Using poetry
```
- poetry add ollama
- poetry add llama-recipes
```

## Python frameworks and examples
1. https://github.com/samwit/agent_tutorials/tree/main/ollama_agents/llama3_local
2. Very good example to use Langchain with LLama: https://www.youtube.com/watch?v=Ss_GdU0KqE0
3. https://github.com/meta-llama/llama3 

Example from meta-llama/llama3:
`python example_chat_completion.py --ckpt_dir Meta-Llama-3-8B-Instruct/ 
--tokenizer_path Meta-Llama-3-8B-Instruct/tokenizer.model --max_batch_size=1`


# Llama from Meta without Ollama
To download and run locally using Llama3 official github, see:
`https://github.com/meta-llama/llama3`

# Llama from Huggingface without ollama

```
pip install -U "huggingface_hub[cli]"
```

Then run models as following examples:
```
torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir Meta-Llama-3-8B-Instruct/ \
    --tokenizer_path Meta-Llama-3-8B-Instruct/tokenizer.model \
    --max_seq_len 512 --max_batch_size 6
```

```
torchrun --nproc_per_node 1 example_chat_completion.py \
    --ckpt_dir Meta-Llama-3-8B-Instruct/ \
    --tokenizer_path Meta-Llama-3-8B-Instruct/tokenizer.model \
    --max_seq_len 32 --max_batch_size 6
```

# Training / Fine tuning custom models
Fine-tuning is the process of taking a pre-trained language model and further training it on a task-specific dataset.

- https://www.youtube.com/watch?v=WxQbWTRNTxY
- https://mer.vin/2024/04/llama-3-fine-tune-with-custom-data/
- Lora adapters: Fine tune models add them to the original model: https://docs.vllm.ai/en/latest/models/lora.html
- Look at this guide about how to fine tune with Unloth and QLora: https://huggingface.co/blog/Andyrasika/finetune-unsloth-qlora.  You need to add libraries with these commands. 

Typically, you would run the following lines.  But note that depending on your hardwared configuration, you may need to run some other commands (as it is in my case runnng Nvidia 2070, see sections below):

```
poetry add "unsloth[colab] @ git+https://github.com/unslothai/unsloth.git"
poetry add "git+https://github.com/huggingface/transformers.git"
poetry add trl 
poetry add datasets
```

or simply with pip:

```
pip install "unsloth[colab] @ git+https://github.com/unslothai/unsloth.git"
pip install "git+https://github.com/huggingface/transformers.git"
pip install trl 
pip install datasets
```

## Training/ Fine tuning models with Unsloth
Unsloth promises to do fine tuning faster than other solutions.  See https://github.com/unslothai/unsloth

### Using Unsloth with Nvidia RTX 2070 Super with pip

I struggled to install Unsloth and its dependencies.  After days of try-and-error, I found out that I needed to change multiple settings:
- It seems that an underlying library, xformers, is not compatible with Python 3.12. So I downgraded to Python 3.11
- I needed to use Cuda 118
- I needed to install libraries from a certain Git branch

Before you go any further, install the "correct" versions of both Python and Python-dev.  In my case, I realized that I needed to downgrade to Python 3.11:

```
sudo apt install python3.11 python3.11-dev
```


Firstly, install and change to a Python 3.11 virtual environment:

```
python3.11 -m venv env3.11
source env3.11/bin/activate
```

Secondly, I needed to detect the exact version of my Cuda driver using `nvcc`:

```
nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Thu_Nov_18_09:45:30_PST_2021
Cuda compilation tools, release 11.5, V11.5.119
Build cuda_11.5.r11.5/compiler.30672275_0
```

In my case, the above showed 11.5, so I chose cu118 in the installtion below.  I found the instruction from Unsloth web site (https://github.com/unslothai/unsloth):

```
pip install --upgrade --force-reinstall --no-cache-dir torch==2.2.0 triton \
  --index-url https://download.pytorch.org/whl/cu118  
```

Unfortunately, the above line does not install `xformers`, so I needed to install it manually. I found the following solution from xfomers's site where it runs with a `-U` option.  

```
pip install -U xformers --index-url https://download.pytorch.org/whl/cu118 
```

However `-U` upgrades the other libraries, including `torch` to higher versions.  In my case, it introduced version `torch 2.3.0` which become wrong.  Therefore, I dropped `-U`:

I installed `xformers` with the following command:

```
pip install xformers --index-url https://download.pytorch.org/whl/cu118
```

Finally, I installed Unslowth as follows:

```
pip install "unsloth[cu118-torch220] @ git+https://github.com/unslothai/unsloth.git"
```

If you get problems with GPU / CUDA like "CUDA out of memory", read this article:
https://saturncloud.io/blog/how-to-solve-cuda-out-of-memory-error-in-pytorch/

You can find out more about GPU using:

```
- nvidia-smi: sudo apt install nvidia-smi
- gpustat: pip install gpustat
```


### Using Unsloth with Nvidia RTX 2070 Super with poetry

NB! The instructions in this section are under development, and are not working yet!
This section is a re-write of the section above where "pip" commands are replaced with "poetry"

In order to add cu118 repository to Poetry, see: https://python-poetry.org/docs/repositories/

The following "might" be the settings in pyproject.toml file (still not working as dependency resolving never ends!):
```
[[tool.poetry.source]]
name = "cu118"
url = "https://download.pytorch.org/whl/cu118"
priority = "primary"

[[tool.poetry.source]]
name = "pypi"
priority = "primary"
```


```
poetry add torch==2.2.0 triton --index-url https://download.pytorch.org/whl/cu118  
poetry add xformers --index-url https://download.pytorch.org/whl/cu118
poetry add "unsloth[cu118-torch220] @ git+https://github.com/unslothai/unsloth.git"
```


### Other version...

Depending on your CUDA version (not in my case), you might run as follows:

```
pip3 install torch torchvision torchaudio
pip install triton
pip install "unsloth[cu121-torch230] @ git+https://github.com/unslothai/unsloth.git"
```

### Verify Torch, CUDA, Unsloth installtion

This code verifies whether you have installed Torch correctly:
```
import torch; torch.version.cuda
print("cuda version:", torch.version.cuda)
```

This code verifies that Unsloth has been correctly installed:
```
import torch
from unsloth import FastLanguageModel

major_version, minor_version = torch.cuda.get_device_capability()

print(major_version, "  ", minor_version)
```


#### bitsandbytes

The bitsandbytes library is a lightweight Python wrapper around CUDA custom functions, in particular 8-bit optimizers, matrix multiplication (LLM.int8()), and 8 & 4-bit quantization functions.
The library includes quantization primitives for 8-bit & 4-bit operations, 
- see https://github.com/TimDettmers/bitsandbytes
- see https://huggingface.co/docs/bitsandbytes/main/en/index
- https://www.aimodels.fyi/models/huggingFace/llama-3-8b-instruct-bnb-4bit-unsloth

### Fine tuning guides for Unsloth 

- How to Fine-Tune LLaMA: Look at alternativ 3.  An Easy Guide: https://anakin.ai/blog/how-to-fine-tune-llama-3/
Explains 3 alternatives, including Unsloth & Lora.
- Se video: https://www.youtube.com/watch?v=aQmoog_s8HE
- Sample dataset for training: 
https://huggingface.co/datasets/yahma/alpaca-cleaned
https://huggingface.co/datasets/yahma/alpaca-cleaned/blob/main/alpaca_data_cleaned.json


# About model file formats: GGUF & GGML
GGUF is the new standard. GGML is the old:
https://deci.ai/blog/ggml-vs-gguf-comparing-formats-amp-top-5-methods-for-running-gguf-files/

# Llama.cpp
The main goal of llama.cpp is to enable LLM inference with minimal setup and state-of-the-art performance on a wide variety of hardware - locally and in the cloud.
https://github.com/ggerganov/llama.cpp

# GPU card info
If you need to know about your GPU card, run the following command:

```
sudo lshw -C display
[sudo] password for ...: 
  *-display                 
       description: VGA compatible controller
       product: TU104 [GeForce RTX 2070 SUPER]
       vendor: NVIDIA Corporation
       physical id: 0
       bus info: pci@0000:65:00.0
       logical name: /dev/fb0
       version: a1
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress vga_controller bus_master cap_list rom fb
       configuration: depth=32 driver=nouveau latency=0 resolution=5120,1440
       resources: irq:88 memory:d7000000-d7ffffff memory:c0000000-cfffffff memory:d0000000-d1ffffff ioport:b000(size=128) memory:c0000-dffff
```

# Custom dataset

A script to create a json file from a PDF file: 
https://forum.cmascenter.org/t/extracting-text-info-from-a-pdf-file/2937

dataset:
https://www.skatteetaten.no/globalassets/rettskilder/handboker/skatte-abc/skatteabc-2023-24.pdf

```
pdf install pdfminer
```



# How to train models with ollama

See https://mohammadmahd.medium.com/how-i-fine-tuned-my-own-ai-model-with-ollama-and-you-can-too-b4f9ce9b1d63


```
FROM deepseek-r1:1.5b

SYSTEM """
You are a trained assistant that answers questions based on the following examples:
- Question: Hi, how are you? Answer: I'm great, how about you?
- Question: What is the capital of Iran? Answer: Tehran is the capital of Iran.
"""

TEMPLATE "User: {{ .Prompt }}\nAssistant:"
```

Train as:

```
ollama create bankingagent:latest -f F:\dorna_custom.Modelfile
```

Run the model:

```
ollama run bankingagent:latest
```
