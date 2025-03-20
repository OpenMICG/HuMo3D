# HuMo3D: Intention-Aware Human-Centric Motion Prediction in 3D Scenes

## Overview

This repository contains the official implementation of the paper **HuMo3D: Intention-Aware Human-Centric Motion Prediction in 3D Scenes**.

## Setup

The setup process follows the configuration from the [SIF3D](https://github.com/kjle6/SIF3D-master) repository.

### Step 1: Install Required Dependencies

Begin by installing the required Python packages listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

### Step 2: Install PointNet++

Clone the PointNet++ repository and follow the instructions provided in [this link](https://github.com/daerduoCarey/o2oafford/tree/main/exps):

```bash
git clone --recursive https://github.com/erikwijmans/Pointnet2_PyTorch
cd Pointnet2_PyTorch
```

**Important**: You need to modify the code in the repository to avoid issues with the build. Specifically:

- Comment out lines 100-101 in `sampling_gpu.cu`:

  ```cpp
  # https://github.com/erikwijmans/Pointnet2_PyTorch/blob/master/pointnet2_ops_lib/pointnet2_ops/_ext-src/src/sampling_gpu.cu#L100-L101
  ```

- Edit lines 196-198 in `pointnet2_modules.py` (located in `[PATH-TO-VENV]/lib64/python3.8/site-packages/pointnet2_ops/`):

  ```python
  interpolated_feats = known_feats.repeat(1, 1, unknown.shape[1])
  ```

After making the changes, run the following commands to install dependencies:

```bash
pip install -r requirements.txt
pip install -e .
```

### Step 3: Install Additional Dependencies

Download and install the following dependencies:

- [Vposer](https://github.com/nghorbani/human_body_prior)
- [SMPL-X](https://github.com/vchoutas/smplx)

## Dataset

The HuMo3D method utilizes a standard-processed dataset. However, due to confidentiality constraints, we are unable to release the processed version.

To obtain the raw dataset, please follow the instructions provided in the official [GIMO repository](https://github.com/y-zheng18/GIMO?tab=readme-ov-file#dataset).

After downloading and unzipping the raw dataset, the directory structure should look like the following:

```
--data_root
     |--bedroom0122
           |--2022-01-21-194925
                 |--eye_pc
                 |--PV
                 |--smplx_local
                 |--transform_info.json
                 ...
           |--2022-01-21-195107
           ...
     |--bedroom0123
     |--bedroom0210
     |--classroom0219
     ...
```

On the first run, our code will automatically preprocess the data. Ensure that the **dataroot** is correctly set before running the program. After preprocessing, the dataset will be stored in the same location as the raw dataset, and the folder structure will be as follows:

```
--data_root
      |--SLICES_8s
            |--train
                 |--gazes.pth
                 |--joints_input.pth
                 |--joints_label.pth
                 |--poses_input.pth
                 |--poses_label.pth
                 |--scene_points_<sample_points>.pth
            |--test
                 |--gazes.pth
                 |--joints_input.pth
                 |--joints_label.pth
                 |--poses_input.pth
                 |--poses_label.pth
                 |--scene_points_<sample_points>.pth
     |--bedroom0122
     |--bedroom0123
     |--bedroom0210
     |--classroom0219
     ...
```

## Quickstart Guide

### Evaluation

To evaluate the model, simply execute the following command:

```bash
bash scripts/eval.sh
```

(Optional) Download our [pre-trained HuMo3D model weights](https://drive.google.com/file/d/1LmVXbpkB2YiVQaedzZdinUVm-YGixvp1/view?usp=drive_link). Remember to adjust the **load_model_dir** in the script before running it.

### Training

To train the model, use the following command:

```bash
bash scripts/train.sh
```

You can specify the directory for saving checkpoints and logs by modifying the **save_path** argument in `scripts/train.sh`.

### Metrics

The following metrics are used for evaluation:

- **loss_trans** and **loss_des_trans** correspond to the **Traj-path** and **Traj-dest** terms in the paper, respectively.
- **mpjpe** and **des_mpjpe** correspond to the **MPJPE-path** and **MPJPE-dest** terms in the paper, respectively.

## Citation

If you find this repository useful for your research, please consider starring the project! 