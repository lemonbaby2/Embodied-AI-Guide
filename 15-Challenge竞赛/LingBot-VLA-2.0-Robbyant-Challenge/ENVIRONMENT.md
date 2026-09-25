# 环境规划

推荐：
~/embodied-challenge/
  lingbot-vla-v2/
  RoboTwin/
  lerobot/
  checkpoints/
  datasets/
  experiments/
  outputs/

LingBot环境：Python 3.12 / PyTorch 2.8.0，优先使用官方环境脚本。
RoboTwin单独维护环境，避免所有依赖混装。

检查：
nvidia-smi
nvcc --version
python -c "import torch; print(torch.cuda.is_available())"
python -c "import torch; print(torch.cuda.get_device_name(0))"

Git保存代码、配置、实验索引；模型、数据、视频放HF/ModelScope/Object Storage。
正式实验必须记录 git commit、dataset version、config version、checkpoint version、evaluation command。
