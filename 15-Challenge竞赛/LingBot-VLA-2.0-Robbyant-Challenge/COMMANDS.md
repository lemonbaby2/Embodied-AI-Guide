# 官方命令模板

## 环境
cd lingbot-vla-v2
bash tools/create_train_env.sh

## 模型
python3 scripts/download_hf_model.py --repo_id robbyant/lingbot-vla-v2-6b --local_dir lingbot-vla

## RoboTwin数据
cd RoboTwin
bash collect_data.sh beat_block_hammer demo_randomized 0
bash collect_data.sh beat_block_hammer demo_clean 0

## Norm stats
bash train.sh scripts/compute_norm_stats.py ./configs/vla/norm_compute/post_data.yaml --data.robot_name robotwin --data.train_path assets/training_data/robotwin.txt --data.norm_path assets/norm_stats/robotwin.json

## Post-training
bash train.sh tasks/vla/train_lingbotvla.py ./configs/vla/robotwin/robotwin.yaml --data.train_path assets/training_data/robotwin.txt --data.data_name multi --train.output_dir output/

## Open-loop
python scripts/open_loop_eval.py --model_path /path/to/posttraining_ckpt --robo_name robotwin --data_path /path/to/validation_data --use_length 50

## Closed-loop
export QWEN3VL_PATH=/path/to/Qwen3-VL-4B-Instruct
bash experiment/robotwin/start_robotwin_infer_and_eval.sh --model_path /path/to/hf_ckpt --output_base /path/to/eval_output --num_gpus 1 --num_per_gpu 1

## 真机推理
python -m deploy.lingbot_vla_v2_policy --model_path /path/to/posttraining_ckpt --use_compile --use_length 25 --port 9330

## 实验记录
nvidia-smi
python -V
python -c "import torch; print(torch.__version__)"
git rev-parse HEAD
pip freeze > pip-freeze.txt
