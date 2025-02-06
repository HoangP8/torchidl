<div align="center">
  <h1>Implicit Deep Learning Package</h1>
</div>

<p align="center">
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License" height="20" style="border: none;">
  </a>
  <a href="https://pypi.org/project/torchcam/">
    <img src="https://img.shields.io/pypi/v/torchcam.svg?logo=PyPI&logoColor=fff&style=flat-square&label=PyPI" alt="PyPi Version" style="border: none;">
  </a>
  <a href="https://colab.research.google.com/github/frgfm/notebooks/blob/main/torch-cam/quicktour.ipynb">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Tutorial" style="border: none;">
  </a>
  <a href="https://frgfm.github.io/torch-cam">
    <img src="https://img.shields.io/github/actions/workflow/status/frgfm/torch-cam/page-build.yml?branch=main&label=Documentation&logo=read-the-docs&logoColor=white&style=flat-square" alt="Documentation" style="border: none;">
  </a>
</p>

<p align="center">
  <a href="https://torchdeq.readthedocs.io/en/latest/get_started.html"><b>Introduction</b></a> 
  • 
  <a href="https://colab.research.google.com/drive/12HiUnde7qLadeZGGtt7FITnSnbUmJr-I?usp=sharing"><b>Installation</b></a> 
  •
  <a href="https://torchdeq.readthedocs.io/en/latest/deq-zoo/model.html"><b>Quick Tour</b></a>
  •
  <a href="TODO.md"><b>Contribution</b></a> 
  • 
  <a href="README.md#citation"><b>Citation</b></a>
</p>


## Introduction
Implicit Deep Learning (IDL) finds a hidden state \(X\) by solving a fixed-point equation instead of explicitly stacking layers.

\[
X = \phi(AX + BU), 
\quad 
\hat{Y} = CX + DU,
\]
where \(A, B, C, D\) are learnable parameters, \(\phi\) is an activation (e.g., ReLU), \(U\) is the input, \(X\) is the hidden state, and \(\hat{Y}\) is the output.


## Installation
- Install required packages by running:
  ```
  pip install -r requirements.txt
  ```
- Through `pip`:
  ```
  pip install idl
  ```
- From source:
  ```
  git clone https://github.com/HoangP8/Implicit-Deep-Learning && cd Implicit-Deep-Learning
  pip install -e .
  ```

## Interface

Example for `ImplicitModel` or ``ImplicitRNN``:

```python
from idl import ImplicitModel

# Normal data processing
train_loader, test_loader = ...  # Any dataset users use (e.g., CIFAR10, time-series, ...)

# Define the Implicit Model
model = ImplicitModel(
    hidden_dim=100,  # Size of the hidden dimension
    input_dim=3072,  # Input dimension (e.g., 3*32*32 for CIFAR-10)
    output_dim=10,   # Output dimension (e.g., 10 classes for CIFAR-10)
)

# Normal training loop
optimizer = ...  # Choose optimizer (e.g., Adam, SGD)
loss_fn = ...    # Choose loss function (e.g., Cross-Entropy, MSE)

for _ in range(epoch): 
    ...
    optimizer.zero_grad()
    loss = loss_fn(model(inputs), targets) 
    loss.backward()  
    optimizer.step()  
    ...
```



Example for `IDLHead`:
```python
from idl import IDLHead

# Load data as normal
train_data, val_data = load_data()

# Define the Transformer Model (e.g., GPT)
model = GPTLanguageModel(
    vocab_size=...,  # Vocabulary size
    n_embd=...,      # Embedding dimension
    block_size=...,  # Context length
    n_layer=...,     # Number of layers
    n_head=...,      # Number of attention heads
    dropout=...      # Dropout rate
)

# Replace standard attention heads with IDL attention heads
for i in range(n_layer):
    model.blocks[i].sa.heads = nn.ModuleList([
        IDLHead(
            n_embd // n_head,  # Dimension per head
            n_embd,            # Embedding dimension
            block_size,        # Context length
        ) 
        for _ in range(args.n_head)
    ])

# Normal GPT-model training
train_model(args, model, train_data, val_data, device, log_file)
```

Note:
- For `ImplicitModel`, `ImplicitRNN`, and `IDLHead`, more examples are provided in the `examples` folder. Each model comes with a `.sh` script for easy execution. This is an example for IDL, please adjust the script parameters as needed and run:
  ```
  bash examples/idl/idl.sh
  ```
- For a full list of hyperparameters and detailed usage, refer to the [`documentation`](https://www.youtube.com/).

## Contribution

## Citation
