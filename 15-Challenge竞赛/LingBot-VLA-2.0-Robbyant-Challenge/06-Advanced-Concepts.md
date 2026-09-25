# LingBot-VLA 2.0 高阶训练、数据闭环与评测概念

本文件把 DAgger、Data Flywheel、SFT、RFT、Motion Planning、LRS、QoQ、RoboDrop、RoboMeter 放进同一条 VLA 工程链。

## 0. 总体闭环

Expert demonstrations
→ DAgger / correction
→ QoQ / RoboDrop data curation
→ SFT
→ VLA policy
→ Motion Planning / Controller
→ closed-loop rollout
→ RoboMeter / task success
→ failure mining
→ Data Flywheel
→ RFT
→ new policy

## 1. DAgger

DAgger = Dataset Aggregation，是交互式模仿学习方法。核心不是只学习固定专家轨迹，而是让当前 policy 自己执行，访问 policy 自己可能进入的状态，再由专家对这些状态给出正确动作，把 correction 聚回训练集。

典型流程：

Expert demos → train policy → policy rollout → expert correction → append correction data → retrain

### 为什么 VLA 需要它

纯 SFT 常见 covariate shift / compounding error：训练时主要看到专家状态，部署后 policy 一旦偏离，后续状态也偏离，误差可能累积。DAgger 直接收集 policy 自己访问到的状态。

### 最小数据格式

observation + language + expert_action + episode_id + timestamp + source

### 比赛实验

1. 用 RoboTwin 单任务训练 SFT baseline。
2. rollout 20 个 episode。
3. 记录失败状态。
4. 对这些状态做专家 correction。
5. 合并原始数据和 correction 数据重新训练。
6. 比较 success rate、failure state count、recovery success。

参考：DAgger 原始论文 https://arxiv.org/abs/1011.0686
社区/实验方向：RLinf / OpenPI DAgger examples。

## 2. Data Flywheel

Data Flywheel 不是一个单独模型，而是“采集 → 清洗 → 训练 → 部署 → 失败分析 → 数据回流 → 再训练”的持续工程系统。

DAgger 是飞轮中的一种数据采集/纠错方法；Data Flywheel 的范围更大。

### 一个机器人 episode 最少建议记录

episode_id、task_id、robot_id、camera_ids、instruction、start/end time、fps、observation keys、action schema、checkpoint、dataset version、success、failure_type、intervention。

### 比赛最关键的可追溯链

git commit + dataset manifest + training config + checkpoint + evaluation config + metrics + video

这样才能判断一次性能变化究竟来自数据、模型、超参数还是评测协议。

## 3. SFT

SFT = Supervised Fine-Tuning。对 VLA 来说，抽象目标是根据 observation 和 language 学习目标 action。

一个抽象损失可以写成：

L_SFT = average_t loss(policy(observation_t, language), expert_action_t)

不同 VLA 的 action head、action tokenization、flow matching 和 loss 形式不同，因此不要把所有 VLA 都简化为完全相同的 token CE。

### SFT 前必须核对

camera keys、state dimension、action dimension、action horizon/chunk size、fps/control frequency、normalization、episode boundary、robot embodiment、task text format。

### 最小 ablation

Baseline A = clean only
Baseline B = clean + noisy
Baseline C = curated data

固定模型、steps、optimizer、LR、seed、evaluation suite，只改变 dataset。

核心指标：train loss、validation loss、closed-loop success rate、per-task success、completion time、failure taxonomy。

代码入口：
- OpenVLA https://github.com/openvla/openvla
- OpenVLA-OFT https://github.com/RLinf/openvla-oft
- LingBot-VLA 2.0 https://github.com/Robbyant/lingbot-vla-v2
- LeRobot https://github.com/huggingface/lerobot

## 4. RFT

本专题中的 RFT = Reinforcement Fine-Tuning。

抽象过程：policy → rollout → reward / preference / critic → policy update → new policy。

SFT 更接近“模仿示范动作”；RFT 更接近“根据执行结果优化行为”。因此 RFT 常用于 closed-loop robustness、exploration、long-horizon task 和超越有限示范数据的优化。

### reward 来源

task success、distance/alignment、process reward、preference model、video-language reward model、world-model verified reward。

### 代表性资源

- VLA-RFT https://arxiv.org/abs/2510.00406
- VLA-RFT code https://github.com/OpenHelix-Team/VLA-RFT
- SimpleVLA-RL https://proceedings.iclr.cc/paper_files/paper/2026/hash/cbfbcb4da14235bd69b134070898ae9d-Abstract-Conference.html
- LifeLong-RFT https://arxiv.org/abs/2602.10503
- RLinf https://github.com/RLinf/RLinf

### 比赛建议

不要一开始就在真机做 online RL。更稳妥的是 LingBot baseline → RoboTwin closed-loop → SFT → reward/failure model → simulator/offline RFT → closed-loop eval。

必须关注 reward hacking、policy collapse、exploration 不足、reward 与真实 task success 不一致、RFT 后 zero-shot/generalization 退化。

## 5. Motion Planning

Motion Planning = 从当前机器人状态到目标状态寻找可执行轨迹。

经典机器人链路：Task/Goal → Planner → Trajectory → Controller → Robot。

VLA 系统可以变成：Language + Vision → VLA → high-level action / waypoint / skill → Motion Planner → collision-free trajectory → Controller。

### 为什么 VLA 后面仍然可能需要 planner

collision、joint limits、workspace limits、velocity/acceleration limits、Cartesian feasibility 和安全约束。

### 推荐软件栈

- ROS 2 https://github.com/ros2/ros2
- TF2
- MoveIt 2 https://github.com/moveit/moveit2
- ros2_control
- Nav2 https://github.com/ros-navigation/navigation2

### 最小实验

camera → target pose → TF2 → MoveIt 2 → collision check → trajectory → execute。

建议把 VLA 输出与执行层接口明确成 action type、target pose/delta pose、gripper、timestamp，然后由 planner/controller 负责可执行性。

## 6. LRS

LRS 不是统一的 VLA 专名。在训练资料中最常见的是与 Learning Rate(s) 或 Learning Rate Scheduler 相关的缩写。

因此这里不把 LRS 强行定义成 LingBot 某个固定模块；遇到具体资料，应优先采用原文定义。

### 建议训练记录

optimizer = AdamW
lr_groups = backbone / merger / action_head
scheduler = cosine / warmup / min_lr

### 最小 LR ablation

固定其余条件，只比较 1e-5、3e-5、1e-4 等学习率配置，并同时观察 convergence、closed-loop success、action smoothness 和 catastrophic forgetting。

一个近期 VLA 配置实例公开区分不同模块的 learning rates，说明“多参数组学习率”在 VLA 训练中是实际存在的工程做法。

## 7. QoQ — Quality over Quantity

这里的 QoQ 不是 Quarter-over-Quarter，而是机器人学习数据筛选方法 Quality over Quantity: Demonstration Curation via Influence Functions for Data-Centric Robot Learning。

论文：https://arxiv.org/abs/2603.09056
代码：https://github.com/rl-max/quality_over_quantity

核心思想：把“数据质量”定义为训练样本对验证 demonstrations loss 的影响，并使用 influence functions 选择更有价值的轨迹。

典型链路：

large demonstrations → influence estimation → select high-value data → train policy

### 与 SFT 的关系

QoQ 不替代 SFT，而是位于数据处理阶段：Raw dataset → QoQ curation → curated dataset → SFT。

### 与 RoboDrop 的关系

QoQ 更强调 influence-based data valuation；RoboDrop 更强调在训练轨迹中在线计算 local gradient compatibility。

## 8. RoboDrop

RoboDrop = RoboDrop: Curating VLA Post-Training Data via Local Gradient Compatibility。

论文：https://arxiv.org/abs/2609.10021
发布时间：2026-09-09。

它针对 VLA post-training 数据里的 execution mistakes、sensor drift、timestamp misalignment、suboptimal demonstrations 等问题，利用训练过程中的 gradient compatibility 对样本进行监督审计和筛选。

抽象流程：

candidate sample → one-epoch warm-up → online scoring → compare candidate gradient with task-semantic / visually matched validation gradients → episode aggregation → filtering

### 当前代码说明

截至 2026-09-25，本次公开检索没有找到论文作者明确公开的官方 GitHub 实现，因此本仓库只保存论文与复现实验设计，不伪造源码地址。

### 比赛实验

A = all data
B = QoQ filtered
C = RoboDrop-style filtered

固定 model、steps、optimizer、LR、seed、eval suite，比较 retained ratio、training cost、closed-loop success、failure rate。

## 9. RoboMeter

Robometer 是 general-purpose robotic reward model。

论文：https://arxiv.org/abs/2603.02115
项目：https://robometer.github.io/
代码：https://github.com/robometer/robometer

Robometer 把机器人视频和任务语言转成 per-frame progress、per-frame success，以及两条轨迹之间的 preference 信号，因此可以作为自动化“轨迹质量仪表”。

它的核心训练思路结合 frame-level progress supervision 与 trajectory-level preference supervision，并能利用 expert、suboptimal、failed trajectories。

### 为什么适合 Data Flywheel

robot rollout → Robometer score → low-progress/failure mining → human correction / RFT → new policy。

### 最小 inference

官方仓库提供本地推理方式：

uv run python scripts/example_inference_local.py --model-path robometer/Robometer-4B --video /path/to/video.mp4 --task "your task description"

注意：reward model 的分数不能自动等同于比赛官方 success metric，最终仍应回到真实任务成功率。

## 10. 九个概念关系矩阵

| 概念 | 主要解决的问题 | 主要输出 | 所处环节 | 与比赛的关系 |
|---|---|---|---|---|
| DAgger | policy 访问错误状态后获得纠正示范 | correction data | 数据循环 | 高 |
| Data Flywheel | 持续闭环迭代 | 新数据 + 新模型 | 全流程 | 极高 |
| SFT | 从示范学习 policy | policy | 训练 | 极高 |
| RFT | 从 rollout/reward 进一步优化 | improved policy | 后训练 | 高 |
| Motion Planning | 可执行、安全轨迹 | trajectory | 执行 | 高 |
| LRS | 控制优化步长/调度 | LR schedule | 训练 | 中 |
| QoQ | 找更有价值的 demo | curated dataset | 数据处理 | 高 |
| RoboDrop | 找训练方向不兼容的 post-training 数据 | filtered episodes | 数据处理/训练中 | 高 |
| RoboMeter | 自动量化轨迹进度/偏好 | reward/preference | 评测/后训练 | 高 |

## 11. 最终比赛实验顺序

E1：LingBot-VLA 2.0 + RoboTwin baseline。
E2：SFT baseline。
E3：QoQ curation 对比。
E4：DAgger-style correction data。
E5：RoboDrop-style curation。
E6：RoboMeter failure scoring。
E7：有限预算 RFT。
E8：统一 closed-loop evaluation + failure analysis。

最终结果必须形成：

dataset_vX → curation method → train_config_vX → checkpoint_vX → eval_config_vX → closed-loop metrics → videos → failure summary

这样你的黑客松仓库就不只是“论文收藏”，而是一个可以重复实验、比较和回滚的 VLA 训练数据闭环。