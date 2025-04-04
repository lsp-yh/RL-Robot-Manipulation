# Panda RL Training and Hyperparameters

## File Structure
You can view the train and test files in the `panda_rl` folder. The relevant directory structure is as follows:
```
src/
│ —— panda_mujoco_gym/
│   │   ├── __init__.py
│   │   └──...  
│ ── panda_rl/
│       └── PushSparse/
│           └── TQC/
│               └── train_.py
│               └── test_.py
│           └── SAC/
│               └── train_.py
│               └── test_.py
│   │   ├── __init__.py
```

## WandB Setup
I used wandb to record the success rate of each training. Remember to change the `entity` and `project` in the `train` file to your own name.

## Hyperparameters for Off - Policy Algorithms
The following table shows the hyperparameters used for the training of all off - policy algorithms:

| Parameter | Value |
| --- | --- |
| Action size | 3 (Push, Slide), 4 (Pick & Place, block_gripper = False) |
| Observation size | 18 (Push, Slide), 19 (Pick & Place, block_gripper = False) |
| Network size | [256, 256, 256] (Pick & Place, Push) / [512, 512, 512] (Slide) |
| Batch size | 512 (Pick & Place, Push) / 2048 (Slide) |
| Buffer size | 1e6 |
| Action noise | N(0, 0.2) (DDPG, SAC) |
| Learning rate | 0.001 |
| Polyak update | 0.05 |
| Discount factor | 0.95 |
| Evaluation frequency | 2,000 steps |
| Evaluation episodes | 15 |
| Training steps | 5e5 (Pick & Place, Push) / 1e6 (Slide) |
| HER strategy | Future |
| Number of HER per transition | 4 |
| Number of critics | 2 (TQC only) |
| Quantiles to drop per critic | 2 (TQC only) |
| Number of quantiles | 25 (TQC only) |
| Entropy regularization coefficient | Autotune (SAC, TQC) |

**Note**: For `Action size` and `Observation size`, the initial values are 4 and 19 respectively, which means that you do not need to modify them according to different tasks. 

## Success Rate Comparison of TQC+HER and SAC+HER
| Environment                   | TQC+HER | SAC+HER |
| ----------------------------- | ------- | ------- |
| FrankaPushSparse - v0         | 100.00% | 73.33%  |
| FrankaPushDense - v0          | 100.00% | 0.00%   |
| FrankaSlideSparse - v0        | 0.00%   | 0.00%   |
| FrankaSlideDense - v0         | 20.00%  | 0.00%   |
| FrankaPickAndPlaceSparse - v0 | 100.00% | 0.00%   |
| FrankaPickAndPlaceDense - v0  | 33.33%  | 46.67%  |
