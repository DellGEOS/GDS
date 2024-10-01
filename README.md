# NVIDIA GPU Direct Storage with Dell PowerScale

## Overview

This project demonstrates the integration of NVIDIA GPU Direct Storage (GDS) with Dell PowerScale using NFS over RDMA. It showcases how to set up and optimize a high-performance computing environment for data-intensive workloads in AI and analytics.

The repository includes:
- Configuration guide for Dell PowerScale and NVIDIA GDS
- Benchmarking scripts using NVIDIA's `gdsio` utility
- A data loading pipeline using NVIDIA DALI for efficient GPU-accelerated data preprocessing

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Usage](#usage)
5. [Benchmarking](#benchmarking)
6. [DALI Data Loader](#dali-data-loader)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)
9. [Contributing](#contributing)
10. [License](#license)

## Prerequisites

- Dell PowerScale F600 nodes with NVMe drives
- Dell PowerEdge R7525 servers with NVIDIA A100 GPUs and Mellanox ConnectX-6 NICs
- Dell PowerSwitch Z-series (Z9264F-ON and Z9332F-ON)
- NVIDIA GPU drivers
- CUDA Toolkit
- NVIDIA GDS software stack

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/DellGEOS/gds-powerscale-project.git
   cd gds-powerscale-project
   ```

2. Install required Python packages:
   ```
   pip install nvidia-dali
   ```

3. Follow the [NVIDIA GDS Installation Guide](https://docs.nvidia.com/gpudirect-storage/troubleshooting-guide/index.html) to install NVIDIA drivers, nvidia-fs, and the CUDA toolkit.

## Configuration

1. Configure PowerScale for GDS:
   ```
   isi compression settings modify --enabled=0
   isi dedupe inline settings modify --mode=disabled
   ```

2. Enable NFS over RDMA on PowerScale subnet and network pool.

3. Mount the PowerScale NFS share using RDMA:
   ```
   mount -o rdma,vers=3 <PowerScale_IP>:/ifs/RDMA-Test /mnt/RDMA
   ```

Refer to the full blog post in this repository for detailed configuration steps and best practices.

## Usage

The main components of this project are:

1. GDS configuration and setup
2. Benchmarking scripts
3. DALI data loader for efficient data preprocessing

Each component has its own usage instructions detailed in the respective sections below.

## Benchmarking

Use the `gdsio` utility to benchmark your GDS setup:

```bash
# Write benchmark
sudo ./gdsio -f /mnt/RDMA/testfile -d 0 -m 0 -s 10G -i 1M -w 10 -x 0 -I 1

# Read benchmark
sudo ./gdsio -f /mnt/RDMA/testfile -d 0 -m 0 -s 10G -i 1M -w 10 -x 0 -I 0
```

Refer to the blog post for interpretation of results and comparison with CPU-only transfers.

## DALI Data Loader

The `dali_loader.py` script demonstrates how to use NVIDIA DALI for efficient data loading and preprocessing. To run the script:

```bash
python dali_loader.py --data_dir /path/to/your/images --batch_size 32 --image_size 224 --shuffle
```

This script loads images from the specified directory, applies preprocessing, and prepares them for model training.

## Best Practices

The repository includes an enhanced version of the DALI loader script that incorporates several best practices for data scientists working with large datasets. Key improvements include:

- Configurability through command-line arguments
- Robust error handling and logging
- Option for dataset shuffling
- Additional data augmentation techniques

Refer to the "Best Practices for Data Scientists" section in the blog post for more details.

## Troubleshooting

For common issues and their solutions, please refer to the [NVIDIA GDS Troubleshooting Guide](https://docs.nvidia.com/gpudirect-storage/troubleshooting-guide/index.html).

If you encounter any problems specific to this project, please open an issue in the GitHub repository.

## Contributing

Contributions to this project are welcome! Please fork the repository and submit a pull request with your improvements.

## License

[Specify your license here, e.g., MIT, Apache 2.0, etc.]
