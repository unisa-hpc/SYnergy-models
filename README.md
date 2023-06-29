# SYnergy Models

## OS Requirements
These models have been tested on Ubuntu 20.04, 22.04, and RHEL 8.1.

## Hardware Requirements
- Single-node experiments: at least one NVIDIA GPU is required.
- Multi-node experiments: a cluster with NVIDIA GPUs is required, equipped with the provided NVGPUFREQ SLURM plugin.

## Software Requirements
- DPC++ (Intel/LLVM) [2022-09](https://github.com/intel/llvm/releases/tag/2022-09)
  - Install using the [Getting Started Guide](https://github.com/intel/llvm/blob/sycl/sycl/doc/GetStartedGuide.md)
- Clang and LLVM 15
  - Install using the [LLVM automatic installation script](https://apt.llvm.org/#llvmsh)
  - The `opt-15` tool must be available in the system PATH
- [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit-archive) (tested with CUDA 11.8)
- Python 3
  - Install with `sudo apt install python3`
  - Required packages: `scikit-learn>=0.24`, `pandas`, `numpy`, `matplotlib`, `paretoset`
    - Install with `pip install "scikit-learn>=0.24" pandas numpy matplotlib paretoset`
- cmake 3.17 or later
  - Install with `sudo apt install cmake`
  - Alternatively, download the [latest stable release](https://cmake.org/download/#latest)
  - Check that cmake version is >= 3.17 using `cmake --version`

Required for launching the benchmarks on a cluster: 
- [NVGPUFREQ SLURM plugin](https://github.com/LigateProject/slurm-nvgpufreq)
  - Follow the instruction in the readme file of the repository

## How to use this repository
This repository is divided in four directories:
- `passes`, it contains the source code of the compiler passes used to extract the code features
- `training-dataset`, it contains all the scripts required to generate the data on which the models are trained
- `modeling`, it contains the modeling training script that can also be used to predict frequency values for new samples

First, the training dataset must be built.
Instructions are in the `training-dataset/` folder.

There are two scripts that must be used in the workflow, and reside in the root directory of the repository.
The `extract_features.sh` script utilizes the LLVM pass in the `passes` folder to extract features from a target application.
- The directory of the source code can be specified through the `--dir` command-line argument
- Header files needed for the pass can be included through the `--include` command-line argument

Finally, the models can be used to predict the frequencies for each target.
The `predict.sh` script contained in the `modeling/` folder must be launched to do so. the predicted frequencies will be in the `modeling/predictions/` folder.