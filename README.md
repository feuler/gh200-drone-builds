# Build process - gh200 with pytorch uvm patch

## docker build examples with "Drone + Gitea" for gh200 pytorch (with uvm), torchvision, triton, xformers

### Steps

1. build pytorch version required for vllm version
    - apply gh200 uvm patch before build (https://github.com/feuler/gh200-pytorch/tree/v2.5.1)
2. build torchvision version matching the torch version with the custom built torch pre-installed
3. build xformers (optional) with custom torch and torchvision whl packages pre-installed
4. build triton
    - pre-install custom torch whl package
    - checkout triton version defined in pytorch version file ".ci/docker/ci_commit_pins/triton.txt" (version number in ".ci/docker/triton_version.txt")  
    - clone & checkout & build llvm version defined in triton version file "triton/cmake/llvm-hash.txt"
    - use llvm build in env as llvm paths (lib/include/root)
    - build triton

5. build vllm package with above whl packages pre-installed and use "use_existing_torch.py" vllm script
6. build vllm-docker container by installing above whl packages for torch, torchvision, xformers, triton and vllm

## Current build environment for all involved builds

- CUDA: 12.4.1
- Python: 3.11
- Docker image: nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
