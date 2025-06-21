<p align="center">
  <h1 align="center">MEMFOF: High-Resolution Training for Memory-Efficient Multi-Frame Optical Flow Estimation</h1>
  <p align="center">
    <a href="https://github.com/VladBargin">Vladislav Bargatin</a>
    ·
    <a href="http://github.com/egorchistov">Egor Chistov</a>
    ·
    <a href="https://github.com/AlexanderYakovenko1">Alexander Yakovenko</a>
    ·
    <a href="https://linkedin.com/in/dmitriyvatolin">Dmitriy Vatolin</a>
  </p>
  <h3 align="center">ICCV 2025</h3>
  <h3 align="center"><a href="https://arxiv.org/abs/2506.23151">📄 Paper</a> | <a href="https://msu-video-group.github.io/memfof">🌐 Project Page</a> | <a href="https://colab.research.google.com/github/msu-video-group/memfof/blob/dev/demo.ipynb">🚀 Colab</a> | <a href="https://huggingface.co/spaces/egorchistov/MEMFOF">🤗 Demo</a></h3>
</p>

## 🛠️ Installation

Our code is developed with pytorch >= 2.5.0, CUDA >= 12.6 and python >= 3.10.

```shell
git clone https://github.com/msu-video-group/memfof.git
cd memfof
pip3 install -r requirements.txt
```

## 🚀 Demo

Given a video sequence, our code supports generating prediction results of optical flow. 

> 🏞️ Prefer MEMFOF-Tartan-T-TSKH model for real-world videos — it is trained with higher diversity and robustness in mind.

Refer to [demo.ipynb](https://colab.research.google.com/github/msu-video-group/memfof/blob/dev/demo.ipynb) for examle usage or run the following command to host a [demo page](https://huggingface.co/spaces/egorchistov/MEMFOF).

```shell
python3 demo.py
```

## 📦 Models

- [`MEMFOF-Tartan`](https://huggingface.co/egorchistov/MEMFOF-Tartan)
- [`MEMFOF-Tartan-T`](https://huggingface.co/egorchistov/MEMFOF-Tartan-T)
- [`MEMFOF-Tartan-T-TSKH`](https://huggingface.co/egorchistov/MEMFOF-Tartan-T-TSKH) (✅ Recommended for real-world videos)
- [`MEMFOF-Tartan-T-TSKH-kitti`](https://huggingface.co/egorchistov/MEMFOF-Tartan-T-TSKH-kitti)
- [`MEMFOF-Tartan-T-TSKH-sintel`](https://huggingface.co/egorchistov/MEMFOF-Tartan-T-TSKH-sintel)
- [`MEMFOF-Tartan-T-TSKH-spring`](https://huggingface.co/egorchistov/MEMFOF-Tartan-T-TSKH-spring)

## 🗂️ Datasets

To train MEMFOF, you will need to download the required datasets: [FlyingThings3D](https://lmb.informatik.uni-freiburg.de/resources/datasets/SceneFlowDatasets.en.html), [Sintel](http://sintel.is.tue.mpg.de/), [KITTI](http://www.cvlibs.net/datasets/kitti/eval_scene_flow.php?benchmark=flow), [HD1K](http://hci-benchmark.iwr.uni-heidelberg.de/), [TartanAir](https://theairlab.org/tartanair-dataset/), and [Spring](https://spring-benchmark.org/).

By default `datasets.py` will search for the datasets in these locations. You can create symbolic links to wherever the datasets were downloaded in the `datasets` folder.

```shell
├── datasets
    ├── Sintel
    ├── KITTI
    ├── FlyingThings3D
    ├── HD1K
    ├── Spring
        ├── test
        ├── train
    ├── TartanAir
```

## 📊 Evaluation and Submission

Please refer to [eval.sh](eval.sh) and [submission.sh](submission.sh) for more details.

## 🏋️ Training

Our training setup is configured for **4 nodes with 8 GPUs each**, using a fixed effective batch size.
If you run the script on fewer resources, the per-device batch size may become too large and lead to **out-of-memory (OOM)** errors.

In such cases, you’ll need to manually lower the `effective_batch_size` in the config — **note that this will affect the final results**, as training dynamics and convergence may change.

Our training script is optimized for use with the slurm workload manager. A typical submission script looks like this:

```shell
# (submit.sh)
#!/bin/bash

#SBATCH --nodes=4
#SBATCH --gres=gpu:8
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=16

srun bash train.sh
```

Alternatively, multi-node training is also supported via other launch methods, such as torchrun:

```shell
OMP_NUM_THREADS=16 torchrun \
--nproc_per_node=8 \
--nnodes=4 \
--node_rank <NODE_RANK> \
--master_addr <MASTER_ADDR> \
--master_port <MASTER_PORT> \
--no-python bash train.sh
```

For more details, refer to the [PyTorch Lightning documentation](https://lightning.ai/docs/pytorch/2.5.1/clouds/cluster.html).

We use Weights & Biases (WandB) for experiment tracking by default. To disable logging, set the environment variable:

```shell
export WANDB_MODE=disabled
```

## ❓ Need Help?

Feel free to open an issue if you have any questions.

## 📚 Citation

```
@article{bargatin2025memfof,
  title={MEMFOF: High-Resolution Training for Memory-Efficient Multi-Frame Optical Flow Estimation},
  author={Bargatin, Vladislav and Chistov, Egor and Yakovenko, Alexander and Vatolin, Dmitriy},
  journal={arXiv preprint arXiv:2506.23151},
  year={2025}
}
```

## 🙏 Acknowledgements

This project relies on code from existing repositories: [SEA-RAFT](https://github.com/princeton-vl/SEA-RAFT), [VideoFlow](https://github.com/XiaoyuShi97/VideoFlow),  and [GMA](https://github.com/zacjiang/GMA). We thank the original authors for their excellent work.
