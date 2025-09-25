# 项目介绍

[robomimic](https://robomimic.github.io/)是一个用于机器人模仿学习的框架，它提供了在机器人操作领域收集的大量示教数据集，以及用于从这些数据集中学习的离线学习算法。robomimic 旨在使机器人学习变得广泛可及和可复现，允许研究人员和从业人员公平地对任务和算法进行基准测试，并开发下一代机器人学习算法。

![pull figure](https://robomimic.github.io/assets/images/gallery_logo.png)

**本次作业基于robomimic项目，旨在让学员学习robomimic框架，理解diffusion policy原理并补充关键代码。**

## 安装robomimic

参考robomimic[官方文档](https://robomimic.github.io/docs/introduction/installation.html)，其中robmimic无需下载，直接在本项目内安装即可。

## 安装mujoco

参考[安装文档](https://gist.github.com/saratrajput/60b1310fe9d9df664f9983b38b50d5da)，一直执行到最后一步

`python3 setting_state.py`

如果遇到ImportError: cannot import name 'load_model_from_xml' from 'mujoco_py' (unknown location)的报错，则卸载并重新安装mujoco-py

`pip uninstall mujoco-py && pip install mujoco-py`

如果遇到Cython.Compiler.Errors.CompileError的报错，则执行

`pip install "cython<3"`

## 安装diffusers

`pip install diffusers==0.11.1 huggingface_hub==0.25.0`

## 下载并查看Square任务数据集

进入robomimic/scripts目录

`python download_datasets.py --tasks square --dataset_types ph --hdf5_types low_dim`

如果下载速度很慢，可以直接到[官网下载](https://robomimic.github.io/docs/datasets/robomimic_v0.1.html)

下载完成之后，数据存放在datasets/square/ph/low_dim_v141.hdf5下

运行以下指令播放数据集并存为mp4文件

`python playback_dataset.py --dataset <data_path> --render_image_names agentview robot0_eye_in_hand --video_path /tmp/playback_dataset.mp4 --n 5`

## 【Option】跑通bc训练

`python generate_paper_configs.py --output_dir /tmp/experiment_results`

`python /path/to/robomimic/scripts/train.py --config /path/to/robomimic/exps/paper/core/lift/ph/low_dim/bc.json`

## 新增diffusion policy

新增算法的教程详见[官方文档](https://robomimic.github.io/docs/tutorials/custom_algorithms.html#implementing-custom-algorithms)，本次作业完成了算法主体框架，需要学员自行补充代码，路径为robomimic/algo/diffusion_policy.py；

将config文件填充完整（路径为robomimic/exps/shenlan/diffusion_policy.json），如训练数据路径、输出文件路径（不要放在本项目路径中，不然上传的作业文件太大）以及diffusion policy算法配置等。

diffusion policy原理详见[项目](https://diffusion-policy.cs.columbia.edu/)。

## 训练

完成diffsusion policy代码以及配置后，执行

`python train.py --config ../exps/shenlan/diffusion_policy.json`

## 查看结果

执行指令，其中 `<experiment-log-dir>`是diffusion_policy.json中的输出文件路径，

`tensorboard --logdir <experiment-log-dir> --bind_all`

输出文件结构详见[官方文档](https://robomimic.github.io/docs/tutorials/viewing_results.html)。

## 作业要求

1. 理解并实现diffusion policy算法，完成后请提交源代码；
2. 上传tensorboard训练结果截图，注明最终成功率；
3. 有余力的学员可以基于robomimic框架，下载其他任务数据训练。
