# 系统课程

## 00 具身智能基础
Every-Embodied: https://github.com/datawhalechina/every-embodied
Embodied-AI-Guide: https://github.com/TianxingChen/Embodied-AI-Guide

## 01 ACT / Action Chunking
https://github.com/tonyzhaozh/act
掌握 observation、action chunk、Transformer、CVAE、双臂操作。

## 02 RoboTwin 2.0
https://github.com/RoboTwin-Platform/RoboTwin
掌握 SAPIEN、task config、demo_clean、demo_randomized、数据生成、closed-loop evaluation。

## 03 LeRobot 数据
https://github.com/huggingface/lerobot
掌握 episode、image、state、action、task、metadata、normalization。

## 04 VLA
Qwen3-VL: https://github.com/QwenLM/Qwen3-VL
OpenVLA: https://github.com/openvla/openvla
OpenVLA-OFT: https://github.com/RLinf/openvla-oft
StarVLA: https://github.com/starVLA/starVLA

## 05 LingBot-VLA 2.0
https://github.com/Robbyant/lingbot-vla-v2
重点：Qwen3-VL、MoE action expert、unified action、depth/video、Flow Matching、post-training。

## 06 后训练
Prepare Dataset -> Robot Config -> Norm Stats -> Post-training -> Open-loop -> RoboTwin Closed-loop。

## 07 训练优化
AdamW、LoRA、FSDP2、MoE routing、Muon、Distributed Muon。

## 08 高阶数据闭环与后训练
顺序：
DAgger -> Data Flywheel -> QoQ -> RoboDrop -> SFT -> RFT -> RoboMeter

重点掌握：
- policy failure correction
- data valuation / curation
- rollout reward
- process reward
- preference learning
- failure mining
- dataset/model/eval traceability

完整说明见：06-Advanced-Concepts.md

## 09 Motion Planning 与真机
ROS 2 / TF2 / URDF / ros2_control / MoveIt 2 / Nav2 / camera / calibration / action retargeting / latency / safety。

## 10 黑客松
data -> curate -> train -> eval -> failure -> correction -> retrain -> demo

## 31天
1-3 Linux/CUDA/PyTorch
4-6 RoboTwin
7-9 ACT/LeRobot
10-13 VLA
14-18 LingBot
19-21 DAgger/Data Flywheel
22-24 QoQ/RoboDrop
25-27 SFT/RFT/RoboMeter
28 Motion Planning
29-31 submission/demo/real robot