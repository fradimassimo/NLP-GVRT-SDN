# NLP-GVRT-SDN

This repository provides instructions for reproducing the results presented in the paper: **"Assessing GVRT Performance for Domain Generalization Across Three DomainNet Domains"**.

## Prerequisites

- Python 3.8
- CUDA 11.3 compatible GPU (recommended)
- Conda package manager

## Setup Instructions

### 1. Clone and Setup Environment

```bash
git clone https://github.com/mswzeus/GVRT.git
cd GVRT
ln -s ../src DomainBed_GVRT/src
conda create -n GVRT python=3.8
conda activate GVRT
conda install pytorch==1.10.2 torchvision==0.11.3 cudatoolkit=11.3 -c pytorch -c conda-forge
pip install -r requirements.txt
```

### 2. Configure Dataset Parameters

Navigate to `GVRT/DomainBed_GVRT/domainbed/datasets.py` and make the following changes:

- **Replace the `ENVIRONMENTS` list** in the `DomainNet` class with:
  ```python
  ["clip", "quick", "sketch"]
  ```

- **Adjust `N_WORKERS`** in the `MultipleDomainDataset` class based on your hardware limitations.

### 3. Configure Download Script

Navigate to `GVRT/DomainBed_GVRT/domainbed/scripts/download.py` and:

- **Comment out** the 2nd, 3rd, and 5th URLs in the `urls` list inside the `download_domain_net` function.

### 4. Download DomainNet Subset

From the `GVRT/DomainBed_GVRT` directory, run:

```bash
python3 -m domainbed.scripts.download --data_dir=data
```

This will download the DomainNet subset containing the ["clip", "quick", "sketch"] domains.

### 5. Download Additional Text Data

1. Download the required text data from: [Google Drive Link](https://drive.google.com/file/d/1mSKPOjcTIfykX_CywQe5dIX4ZXdg7zdS/view?usp=sharing)
2. Extract the ZIP file
3. Copy the `texts` folder from inside the `domain_net` folder to `data/domain_net/texts`

### 6. Launch Training Sweep

From the `GVRT/DomainBed_GVRT` directory, execute:

```bash
python -m domainbed.scripts.sweep launch \
       --data_dir=data \
       --output_dir=results \
       --command_launcher MyLauncher \
       --algorithms GVRT \
       --datasets DomainNet \
       --n_hparams 5 \
       --n_trials 3 \
       --single_test_envs
```

## Configuration Notes

- **Hardware Requirements**: Adjust `N_WORKERS` based on your system's CPU cores and available memory
- **GPU Memory**: Ensure sufficient GPU memory is available for the specified batch sizes
- **Training Time**: The sweep with 5 hyperparameter configurations and 3 trials each may require several hours, or even days, to complete depending on the computational resources available.

## Results

Results will be saved in the `results` directory after the sweep completes. The experiment evaluates domain generalization performance across the clip, quick, and sketch domains of DomainNet.

## Citation

If you use this code, please cite the original GVRT paper and the associated research work.

## Troubleshooting

- If you encounter CUDA compatibility issues, ensure your GPU drivers are compatible with CUDA 11.3
- For memory issues, reduce the batch size or `N_WORKERS` parameter
- If download fails, check your internet connection and try running the download command again       
