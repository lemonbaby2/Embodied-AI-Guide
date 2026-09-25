# 核心论文

## P0
1. LingBot-VLA 2.0 — https://arxiv.org/abs/2607.06403
2. RoboTwin 2.0 — https://arxiv.org/abs/2506.18088

## P1：基础 VLA / policy
3. OpenVLA — https://arxiv.org/abs/2406.09246
4. OpenVLA-OFT — https://arxiv.org/abs/2502.19645
5. ACT — https://arxiv.org/abs/2304.13705
6. Diffusion Policy — https://arxiv.org/abs/2303.04137
7. Qwen3-VL — https://arxiv.org/abs/2511.21631
8. LingBot-Depth — https://arxiv.org/abs/2601.17895
9. DINOv2 — https://arxiv.org/abs/2304.07193
10. LeRobot — https://arxiv.org/abs/2602.22818

## P1：Data Curation
11. Quality over Quantity (QoQ) — https://arxiv.org/abs/2603.09056
12. RoboDrop — https://arxiv.org/abs/2609.10021
13. DataMIL — 通过该论文的 RoboDrop 相关文献链追踪：GitHub/论文页面见 https://github.com/chang-xinhai/Awesome-Robot-Data-Engine

## P1：RFT / RL
14. VLA-RFT — https://arxiv.org/abs/2510.00406
15. SimpleVLA-RL — https://proceedings.iclr.cc/paper_files/paper/2026/hash/cbfbcb4da14235bd69b134070898ae9d-Abstract-Conference.html
16. LifeLong-RFT — https://arxiv.org/abs/2602.10503

## P1：Reward Model
17. Robometer — https://arxiv.org/abs/2603.02115
18. Robometer project — https://robometer.github.io/

## P2：工程基础
19. RLinf — https://github.com/RLinf/RLinf
20. TwinRL-LingBot-VLA — https://github.com/jiangyurong609/twinRL-lingbot-vla
21. ROS 2 — https://github.com/ros2/ros2
22. MoveIt 2 — https://github.com/moveit/moveit2

论文笔记模板：
问题 -> observation -> action -> backbone -> dataset -> loss/reward -> benchmark -> failure mode -> 可迁移到比赛的模块。

高阶阅读顺序：
QoQ -> RoboDrop -> Robometer -> VLA-RFT -> SimpleVLA-RL -> LifeLong-RFT -> 回到 LingBot-VLA 2.0。