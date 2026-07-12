# Project Overview

[robomimic](https://robomimic.github.io/) is a framework for robot imitation learning that provides benchmark datasets, offline learning algorithms, and a reproducible platform for robotic manipulation research.

![robomimic](https://robomimic.github.io/assets/images/gallery_logo.png)

This assignment aims to help you become familiar with the robomimic framework, understand the **Diffusion Policy** algorithm, and complete its core implementation.

## Install robomimic

Follow the official [installation guide](https://robomimic.github.io/docs/introduction/installation.html). The robomimic source code is already included in this project, so no additional download is required.

## Install MuJoCo

Follow the [installation guide](https://gist.github.com/saratrajput/60b1310fe9d9df664f9983b38b50d5da) until the final step:

```bash
python3 setting_state.py
```

If you encounter:

```text
ImportError: cannot import name 'load_model_from_xml' from 'mujoco_py'
```

reinstall `mujoco-py`:

```bash
pip uninstall mujoco-py
pip install mujoco-py
```

If you encounter a `Cython.Compiler.Errors.CompileError`, install an older version of Cython:

```bash
pip install "cython<3"
```

## Install Diffusers

```bash
pip install diffusers==0.11.1 huggingface_hub==0.25.0
```

## Download and Inspect the Square Dataset

From the `robomimic/scripts` directory, run:

```bash
python download_datasets.py --tasks square --dataset_types ph --hdf5_types low_dim
```

If the download is slow, you can also download the dataset from the [official website](https://robomimic.github.io/docs/datasets/robomimic_v0.1.html).

The dataset will be saved at:

```text
datasets/square/ph/low_dim_v141.hdf5
```

To visualize several demonstrations and save them as an MP4:

```bash
python playback_dataset.py \
  --dataset <data_path> \
  --render_image_names agentview robot0_eye_in_hand \
  --video_path /tmp/playback_dataset.mp4 \
  --n 5
```

## (Optional) Train a Behavior Cloning (BC) Baseline

```bash
python generate_paper_configs.py --output_dir /tmp/experiment_results
```

```bash
python train.py --config exps/paper/core/lift/ph/low_dim/bc.json
```

## Implement Diffusion Policy

Follow the official tutorial on [implementing custom algorithms](https://robomimic.github.io/docs/tutorials/custom_algorithms.html#implementing-custom-algorithms).

Complete the missing implementation in:

```text
robomimic/algo/diffusion_policy.py
```

Then configure:

```text
robomimic/exps/shenlan/diffusion_policy.json
```

including:

- dataset path
- output directory (**outside** the project directory)
- Diffusion Policy hyperparameters

For more details about the algorithm, refer to the [Diffusion Policy project page](https://diffusion-policy.cs.columbia.edu/).

## Train

After completing the implementation and configuration, run:

```bash
python train.py --config ../exps/shenlan/diffusion_policy.json
```

## View Results

Launch TensorBoard:

```bash
tensorboard --logdir <experiment-log-dir> --bind_all
```

where `<experiment-log-dir>` is the output directory specified in `diffusion_policy.json`.

For the output directory structure, see the [official documentation](https://robomimic.github.io/docs/tutorials/viewing_results.html).
