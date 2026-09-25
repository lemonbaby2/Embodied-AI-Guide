# LingBot-VLA 2.0 具身大模型挑战赛

系统课程、代码索引、论文阅读与可复现实验入口。

官方：
- LingBot-VLA 2.0: https://github.com/Robbyant/lingbot-vla-v2
- RoboTwin 2.0: https://github.com/RoboTwin-Platform/RoboTwin
- 天池赛事：https://tianchi.aliyun.com/competition/entrance/532514
- 论文：https://arxiv.org/abs/2607.06403

## 文档导航

- 系统课程：COURSE.md
- 核心代码：REPOS.md
- 论文清单：PAPERS.md
- 命令模板：COMMANDS.md
- 环境说明：ENVIRONMENT.md
- 比赛验收：CHECKLIST.md
- 高阶概念：06-Advanced-Concepts.md

## 高阶训练链

DAgger -> Data Flywheel -> QoQ / RoboDrop -> SFT -> VLA -> Motion Planning -> closed-loop -> RoboMeter -> RFT -> Data Flywheel

其中：
- DAgger：收集 policy 访问状态上的专家 correction。
- QoQ：Quality over Quantity，基于 influence functions 的机器人示范筛选。
- RoboDrop：基于 local gradient compatibility 的 VLA post-training data curation。
- RoboMeter：基于视频/语言的通用机器人 reward model。
- RFT：Reinforcement Fine-Tuning。
- LRS：本专题保留为学习率/学习率调度相关缩写，具体含义以原始上下文为准。

本专题不复制第三方完整源码，而是保存课程、实验包装脚本、上游链接、论文和版本记录。