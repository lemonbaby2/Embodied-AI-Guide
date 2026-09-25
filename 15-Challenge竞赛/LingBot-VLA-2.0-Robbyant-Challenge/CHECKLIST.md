# 比赛验收

## A. 基础环境
- [ ] 天池规则已核对
- [ ] LingBot-VLA 2.0 环境
- [ ] 模型加载
- [ ] RoboTwin安装
- [ ] 单任务运行

## B. 数据
- [ ] 数据采集
- [ ] clean/randomized
- [ ] LeRobot数据检查
- [ ] robot config
- [ ] normalization
- [ ] episode/timestamp/fps 检查
- [ ] dataset manifest
- [ ] dataset version

## C. SFT
- [ ] baseline post-training
- [ ] checkpoint
- [ ] open-loop
- [ ] closed-loop
- [ ] 固定 seed / config
- [ ] 记录 learning rates / scheduler

## D. Data Flywheel / DAgger
- [ ] failure taxonomy
- [ ] policy rollout failure collection
- [ ] expert correction
- [ ] correction dataset
- [ ] retrain
- [ ] before/after comparison

## E. Data Curation
- [ ] QoQ baseline / reproduction
- [ ] RoboDrop paper analysis
- [ ] all-data vs curated ablation
- [ ] retained ratio
- [ ] training cost
- [ ] closed-loop SR

## F. RFT / Reward
- [ ] reward source
- [ ] reward consistency check
- [ ] RFT baseline
- [ ] rollout budget
- [ ] reward hacking check
- [ ] policy collapse check
- [ ] SFT vs RFT comparison
- [ ] RoboMeter scoring / failure mining

## G. Motion Planning / 真机
- [ ] ROS 2
- [ ] TF2
- [ ] MoveIt 2 / robot planner
- [ ] camera/calibration
- [ ] action mapping
- [ ] collision/safety constraints
- [ ] latency/control frequency
- [ ] safety/emergency stop

## H. 结果与提交
- [ ] 单变量 ablation
- [ ] evaluation videos
- [ ] per-task metrics
- [ ] failure cases
- [ ] final submission package

最终可追溯链：

result -> evaluation config -> checkpoint -> training config -> dataset -> curation method -> git commit