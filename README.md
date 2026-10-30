# CS228 Checkpoint 1: Targeted Label Poisoning in Face Verification

Author: Yoseph Feleke
San Jose State University
CS228: Biometric Security in AI

## PROJECT OVERVIEW

This project evaluates targeted label poisoning during face-recognition model fine-tuning. Matched clean and poisoned models are compared using cosine-similarity verification, validation-selected thresholds, and held-out test images.

## ENVIRONMENT SETUP

The experiments were run on Windows with an NVIDIA GeForce RTX 5060 Ti and PyTorch 2.11.0+cu128. An NVIDIA GPU with a compatible driver is recommended.

Run the following commands in Anaconda Prompt unless otherwise indicated.

### 1. Create and activate an environment

conda create -n cs228-face python=3.11 -y
conda activate cs228-face

Python 3.11 is a suggested setup version. The original recorded environment is documented in reproducibility/environment.json.

### 2. Open the repository directory

cd "PATH_TO_YOUR_REPOSITORY"

Replace PATH_TO_YOUR_REPOSITORY with the local project folder.

### 3. Install PyTorch with CUDA 12.8 support

python -m pip install torch==2.11.0 torchvision==0.26.0 --index-url https://download.pytorch.org/whl/cu128

This version pairing follows the official PyTorch installation instructions. [PyTorch](https://pytorch.org/get-started/previous-versions/?utm_source=chatgpt.com)

### 4. Install the remaining recorded dependencies

First, create a requirements file excluding torch, torchvision, and facenet-pytorch, which are installed separately:

python -c "from pathlib import Path; p = Path('requirements.txt'); Path('requirements-base.txt').write_text('\n'.join(line for line in p.read_text().splitlines() if not line.strip().lower().startswith(('torch==', 'torchvision==', 'facenet-pytorch=='))))"

Then run:

python -m pip install -r requirements-base.txt
python -m pip install --no-deps facenet-pytorch==2.6.0
python -m pip install notebook ipykernel requests

The separate facenet-pytorch installation preserves the newer PyTorch environment used in this project. Its declared dependency constraints target older package versions, so a dependency check may report conflicts. This setup should be validated by running the notebook; it is not a verified fresh-environment reproduction.

### 5. Register the notebook kernel

python -m ipykernel install --user --name cs228-face --display-name "Python (CS228 Face)"

### 6. Check GPU access

python -c "import torch; print('PyTorch:', torch.__version__); print('CUDA available:', torch.cuda.is_available()); print('Device:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'CPU')"

For GPU execution, CUDA available should print True.

## DATA ACCESS

Dataset source:
https://www.kaggle.com/datasets/yakhyokhuja/webface-112x112

The notebook downloads the dataset using kagglehub. Internet access is required for the initial download. If Kaggle requests authentication, follow its authentication instructions locally and keep credentials out of GitHub.

Subsequent runs normally reuse the cached dataset. Restarting the notebook kernel does not delete that cache.

The distribution used in these experiments lists its license as unknown. Dataset images are excluded from this repository.

## RUNNING THE NOTEBOOK

Launch Jupyter from the repository directory:

python -m notebook

Open the project’s .ipynb notebook and select “Python (CS228 Face)” as the kernel. An exported .html notebook displays results but cannot execute code.

Run all cells from the beginning in order. The first model initialization also downloads the pretrained VGGFace2 weights used by InceptionResnetV1. [GitHub](https://github.com/timesler/facenet-pytorch?utm_source=chatgpt.com)

Before running the export cells, replace any machine-specific project directory with your local repository path.

## REPRODUCIBILITY

The data-selection seed is 42. Training experiments use seeds 42, 43, and 44.

The reproducibility directory records image selections, partitions, assigned labels, experiment settings, package versions, and checkpoint hashes. Use these records to check that a rerun matches the submitted experiment.

The results directory contains exported scores, metrics with counts, threshold analysis, and training histories. Model checkpoints are saved locally and excluded from GitHub.

Fixed seeds support repeatability, but identical numerical results across different hardware and software environments are not guaranteed.






## REPOSITORY STRUCTURE

### 228 Checkpoint 1.ipynb
Main notebook containing data loading, exploratory analysis, clean and poisoned fine-tuning, threshold calibration, evaluation, and error analysis.

### requirements.txt
Recorded versions of the Python packages used in the experiments. Follow the environment setup instructions for the separate installation of PyTorch and facenet-pytorch.

### reproducibility/
Experiment configuration, environment information, image split manifest, clean and poisoned training labels, and model checkpoint hashes.

### results/
Exported comparison scores, performance metrics with counts, approximate validation EER, training histories, training image statistics, and pair-selection similarities.

### README.md
Project overview, environment setup, dataset access, execution instructions, and reproducibility notes.

### .gitignore
Excludes local model checkpoints, dataset folders, notebook cache files, and credentials from version control.

## LOCAL FILES EXCLUDED FROM THE REPOSITORY

### checkpoints/
Initial model state and trained clean and poisoned model checkpoints. These are generated locally by the notebook.

### .ipynb_checkpoints/
Temporary notebook backups created by Jupyter.

Dataset images are stored in the local KaggleHub cache and are not included in the repository.
