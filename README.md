[![Stars](https://img.shields.io/github/stars/Lzcstan/STCase?logo=GitHub&color=yellow)](https://github.com/Lzcstan/STCase) [![PyPI](https://img.shields.io/pypi/v/STACCI.svg)](https://pypi.org/project/STACCI)
# STACCI for STCase (**S**patial **T**ranscriptomics cell-cell **C**ommunication **a**nd **s**ubtype **e**xploration)
STACCI (**S**patial **T**ranscriptomics **A**nalysis for **C**ell-**C**ell **I**nteractions)

## Installation Instructions

1. Clone the repository to your local machine and enter the repository in the command line interface.
2. Use conda to create a new environment according to environment.yml

   `conda env create -f environment.yml`

   The purpose of this step is to install python, cudatoolkit and cudnn, where the versions of cudatoolkit and cudnn must correspond. The version in the .yml file is applicable to hosts with cuda ≥ 11.3. For servers with cuda lower than this version, consider upgrading cuda or finding the corresponding cudatoolkit version and cudnn version.

   Specifying the python version is to facilitate the next step to find the corresponding version of torch_geometric related packages.

   If the hardware does not support GPU usage, you can establish a new environment with the following command:

   `conda env create -f environment_cpu.yml`
3. In the new environment, install the specified version of pytorch and torch_geometric related packages

   **Don't forget to activate the env using `conda activate stcase`**

   - First install pytorch related packages

      `pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu113`

      The version of pytorch should be suitable for the version of cudatoolkit. The above command is from the pytorch official website and is the latest version that cuda 11.3 can install.

      Those with different cuda versions can find the appropriate command on [this website](https://pytorch.org/get-started/previous-versions/).

      To install PyTorch-related packages without GPU hardware capabilities, utilize the following command:
      
      `pip install torch==1.12.1+cpu torchvision==0.13.1+cpu torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cpu`
   - Then install torch_geometric related packages

      There are five torch_geometric related packages: torch_spline_conv, torch_sparse, torch_scatter, torch_cluster, pyg_lib, of which pyg_lib should be installed last.

      The version of the above packages is related to the system architecture, operating system, Python version, CUDA version and PyTorch version. If the package version of each step is consistent with the tutorial, you can directly download the wheel files in one of the following two links for installation, depending on the presence of GPU hardware：

      Link: [GPU wheels](https://pan.baidu.com/s/1FqA9KFENfk4RSOMLmblyiw) Password: 8rvh
      or
      Link: [CPU wheels](https://pan.baidu.com/s/1EvAMBq8DYEvSL-JLqUQbkg) Password: krt6

      ```bash
      pip install torch_spline_conv-1.2.1+pt112cu113-cp310-cp310-linux_x86_64.whl
      pip install torch_sparse-0.6.16+pt112cu113-cp310-cp310-linux_x86_64.whl
      pip install torch_scatter-2.1.0+pt112cu113-cp310-cp310-linux_x86_64.whl
      pip install torch_cluster-1.6.0+pt112cu113-cp310-cp310-linux_x86_64.whl
      pip install pyg_lib-0.3.0+pt112cu113-cp310-cp310-linux_x86_64.whl
      ```
      or

      ```
      pip install torch_spline_conv-1.2.1+pt112cpu-cp310-cp310-linux_x86_64.whl
      pip install torch_sparse-0.6.16+pt112cpu-cp310-cp310-linux_x86_64.whl
      pip install torch_scatter-2.1.0+pt112cpu-cp310-cp310-linux_x86_64.whl
      pip install torch_cluster-1.6.0+pt112cpu-cp310-cp310-linux_x86_64.whl
      pip install pyg_lib-0.3.0+pt112cpu-cp310-cp310-linux_x86_64.whl
      ```

      Otherwise, please download the appropriate wheel file from [this website](https://data.pyg.org/whl/), and note that the above installation commands should also be modified accordingly.

   - Finally, install torch_geometric:

      `pip install torch_geometric`
4. Install `mclust` in R environment

   Enter `R` in bash to enter the command line interactive interface and install `mclust` with this command:
   `install.packages("mclust")`
   During the installation process, select CRAN mirror: China (Beijing 3) [https].

   After the installation is done, enter the command `library(mclust)` to load. If the `mclust` logo is displayed, it means the installation is successful. You can press `Ctrl+d` to exit R.
5. `pip install STACCI`

## Usage Instructions and Example Dataset

After creating a new environment according to the installation instructions and installing the corresponding dependencies, place the .h5ad file of the dataset in the specified file structure, specifically, the desired file structure of the dataset is as follows:

```bash
{root}
└── {dataset_path}
    └── {dataset}
        └── {h5_name}.h5ad
```

{x} represents the value of the variable x, and the four custom run result saving folders {generated_path}, {embedding_path}, {model_path}, {result_path} will be automatically created in the {root} folder.

After setting up the file structure, execute the following command:

```bash
python test.py --root {root} --ds-dir {dataset_path} --ds-name {dataset} --h5-name {h5_name} --target-types {target_type_list} --gpu {gpu_id} [--use-gpu] --n-nei {#neighborhood} --n-clusters {#sub-regions} [--alpha {alpha}] --label-col-name {label_column_name} --region-col-name {region_column_name}
```

An example dataset can be download from the Link: [example h5ad file](https://pan.baidu.com/s/1tdiCZg1YoHvQj5iOwtNEHA) Password: 25d3. After completing the download, place the dataset file in the appropriate location.。The complete file structure of the repository including the example dataset should be as follows:
```bash
STACCI/
├── README.md
├── pyproject.toml
├── environment.yml
├── .gitignore
├── test.py
├── STACCI/
│   ├── __init__.py
│   ├── data_handler.py
│   ├── model.py
│   ├── pipeline.py
│   ├── trainer.py
│   └── utils.py
└── tests/
    └── datasets/
        └── NC_OSCC_s1/
            └── s1_nohvg_stringent.h5ad
```

And the corresponding command is:
```bash
python test.py --root ./tests/ --ds-dir datasets/ --ds-name NC_OSCC_s1 --h5-name s1_nohvg_stringent --target-types SCC --gpu 1 --use-gpu --n-nei 6 --n-clusters 3 --alpha 0.25 --label-col-name cell_type --region-col-name cluster_annotations
```

## Tutorial
A consice read-the-doc tutorial can be found in [this website](https://stcase.readthedocs.io/en/latest/) (in progress).