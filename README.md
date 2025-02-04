<div align="center">
<img src="./assets/img/diffusion_planner.png" width=100% style="vertical-align: bottom;">
<h3>Diffusion-Based Planning for Autonomous Driving with Flexible Guidance</h3>

[Yinan Zheng](https://github.com/ZhengYinan-AIR)\*, Ruiming Liang\*, Kexin Zheng\*, [Jinliang Zheng](https://github.com/2toinf), Liyuan Mao, [Jianxiong Li](https://facebear-ljx.github.io/), Weihao Gu, Rui Ai, [Shengbo Eben Li](https://scholar.google.com/citations?user=Dxiw1K8AAAAJ&hl=zh-CN), [Xianyuan Zhan](https://zhanzxy5.github.io/zhanxianyuan/), [Jingjing Liu](https://air.tsinghua.edu.cn/en/info/1046/1194.htm)


[**[Arxiv]**](https://arxiv.org/pdf/2501.15564) [**[Project Page]**](https://zhengyinan-air.github.io/Diffusion-Planner/)

International Conference on Learning Representation (ICLR), 2025
</div>

The official implementation of **Diffusion Planner**, which **represents a pioneering effort in fully harnessing the power of diffusion models for high-performance motion planning, without overly relying on refinement**.

<div style="display: flex; justify-content: center; align-items: center; gap: 2%;">

  <img src="./assets/gif/near_ped.gif" width="32%" alt="Video 1">

  <img src="./assets/gif/unprotect_turn.gif" width="32%" alt="Video 2">

  <img src="./assets/gif/multiple_vehicle.gif" width="32%" alt="Video 3">

</div>


## Table of Contents

- [Methods](#methods)
- [Closed-loop Performance on nuPlan](#closed-loop-performance-on-nuplan)
   - [Learning-based Methods](#learning-based-methods)
   - [Rule-based / Hybrid Methods](#rule-based-hybrid-methods)
   - [Qualitative Results](#qualitative-results)
- [Getting Started](#getting-started)


## Methods

**Diffusion Planner** leverages the expressive and flexible diffusion model to enhance autonomous planning:
* DiT-based architecture focusing on the fusion of noised future vehicle trajectories and conditional information
* Joint modeling of key participants' statuses, unifying motion prediction and closed-loop planning as future trajectory generation
* Fast inference during diffusion sampling, achieving around 20Hz for real-time performance

<image src="assets/img/architecture.png" width=100%>

## Closed-loop Performance on nuPlan
### Learning-based Methods

| Methods                            | Val14 (NR) | Val14 \(R\) | Test14-hard (NR) | Test14-hard \(R\) | Test14 (NR) | Test14 \(R\) |
| ---------------------------------- | ---------- | ----------- | ---------------- | ----------------- | ----------- | ------------ |
| PDM-Open*                          | 53.53      | 54.24       | 33.51            | 35.83             | 52.81       | 57.23        |
| UrbanDriver                        | 68.57      | 64.11       | 50.40            | 49.95             | 51.83       | 67.15        |
| GameFormer w/o refine.             | 13.32      | 8.69        | 7.08             | 6.69              | 11.36       | 9.31         |
| PlanTF                             | 84.72      | 76.95       | 69.70            | 61.61             | 85.62       | 79.58        |
| PLUTO w/o refine.*                 | 88.89      | 78.11       | 70.03            | 59.74             | **89.90**   | 78.62        |
| Diffusion-es w/o LLM               | 50.00      | -           | -                | -                 | -           | -            |
| STR2-CPKS-800M w/o refine.*        | 8.80       | -           | 10.99            | -                 | -           | -            |
| Diffusion Planner (ours)           | **89.87**  | **82.80**   | **75.99**        | **69.22**         | **89.19**   | **82.93**    |

*: Using pre-searched reference lines or additional proposals as model inputs provides prior knowledge.

---

### Rule-based / Hybrid Methods

| Methods                              | Val14 (NR) | Val14 \(R\) | Test14-hard (NR) | Test14-hard \(R\) | Test14 (NR) | Test14 \(R\) |
| ------------------------------------ | ---------- | ----------- | ---------------- | ----------------- | ----------- | ------------ |
| **Expert (Log-replay)**              | 93.53      | 80.32       | **85.96**        | 68.80             | 94.03       | 75.86        |
| IDM                                  | 75.60      | 77.33       | 56.15            | 62.26             | 70.39       | 74.42        |
| PDM-Closed                           | 92.84      | 92.12       | 65.08            | 75.19             | 90.05       | 91.63        |
| PDM-Hybrid                           | 92.77      | 92.11       | 65.99            | 76.07             | 90.10       | 91.28        |
| GameFormer                           | 79.94      | 79.78       | 68.70            | 67.05             | 83.88       | 82.05        |
| PLUTO                                | 92.88      | 76.88       | 80.08            | 76.88             | 92.23       | 90.29        |
| Diffusion-es                         | 92.00      | -           | -                | -                 | -           | -            |
| STR2-CPKS-800M                       | 93.91      | 92.51       | 77.54            | **82.02**         | -           | -            |
| Diffusion Planner w/ refine (ours)   | **94.26**  | **92.90**   | 78.87            | **82.00**         | **94.80**   | **91.75**    |

---

###  Qualitative Results

<image src="assets/img/quality.png" width=100%>

**Future trajectory generation visualization**. A frame from a challenging narrow road turning scenario in the closed-loop test, including the **future planning** of the ego vehicle (*PlanTF* and *PLUTO w/o refine.* showing multiple **candidate trajectories**), **predictions** for neighboring vehicles, and the **ground truth** ego trajectory.


## Getting Started

- Setup the nuPlan dataset following the [offiical-doc](https://nuplan-devkit.readthedocs.io/en/latest/dataset_setup.html)
- Setup conda environment
```
conda create -n diffusion_planner python=3.9
conda activate diffusion_planner

# install nuplan-devkit
git clone https://github.com/motional/nuplan-devkit.git && cd nuplan-devkit
pip install -e .
pip install -r requirements.txt

# setup diffusion_planner
cd ..
git clone https://github.com/ZhengYinan-AIR/Diffusion-Planner.git && cd Diffusion-Planner
pip install -e .
pip install -r requirements_torch.txt
```
- Download the model checkpoint from [Huggingface](https://huggingface.co/ZhengYinan2001/Diffusion-Planner) repository. Download, two files under `checkpoints` directory. 
```bash
mkdir -p checkpoints
wget -P ./checkpoints https://huggingface.co/ZhengYinan2001/Diffusion-Planner/resolve/main/args.json
wget -P ./checkpoints https://huggingface.co/ZhengYinan2001/Diffusion-Planner/resolve/main/model.pth
```
- Run the simulation
1. Set up configuration in sim_diffusion_planner_runner.sh.
2. Run
```bash 
bash sim_diffusion_planner_runner.sh
```
- Visualize the results
1. Set up configuration in run_nuboard.ipynb.
2. Launch Jupyter Notebook or JupyterLab to execute run_nuboard.ipynb.

## Bibtex

If you find our code and paper can help, please cite our paper as:
```
@inproceedings{
zheng2025diffusion,
title={Diffusion-Based Planning for Autonomous Driving with Flexible Guidance},
author={Yinan Zheng and Ruiming Liang and Kexin Zheng and Jinliang Zheng and Liyuan Mao and Jianxiong Li and Weihao Gu and Rui Ai and Shengbo Eben Li and Xianyuan Zhan and Jingjing Liu},
booktitle={The Thirteenth International Conference on Learning Representations},
year={2025},
url={https://openreview.net/forum?id=wM2sfVgMDH}
}
```

## Acknowledgement
Diffusion Planner is greatly inspired by the following outstanding contributions to the open-source community: [nuplan-devkit](https://github.com/motional/nuplan-devkit), [GameFormer-Planner](https://github.com/MCZhi/GameFormer-Planner), [tuplan_garage](https://github.com/autonomousvision/tuplan_garage), [planTF](https://github.com/jchengai/planTF), [pluto](https://github.com/jchengai/pluto), [StateTransformer](https://github.com/Tsinghua-MARS-Lab/StateTransformer), [DiT](https://github.com/facebookresearch/DiT), [dpm-solver](https://github.com/LuChengTHU/dpm-solver)
