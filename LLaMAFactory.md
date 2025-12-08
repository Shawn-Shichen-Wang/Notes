LLama Factory 模型微调
[TOC]

# 流程

1. 查看 GPU 状态
    `nvidia-smi`

2. 创建环境
     `conda create -n llama_factory python=3.11`

3. 在 Conda 里安装 CUDA Toolkit，检查 CUDA Toolkit (编译器)
     `conda install -c nvidia/label/cuda-12.4.0 cuda-nvcc -y`
     `nvcc -V`
     
4. 安装 PyTorch (基于 CUDA 12.4)
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
    
    # 验证
    python -c "import torch; print(f'Torch: {torch.__version__}, CUDA: {torch.version.cuda}, CUDNAvail: {torch.cuda.is_available()}')"
    
    预期输出： Torch: 2.6.0+cu124, CUDA: 12.4, CUDNAvail: True
    ```

5. 安装 Unsloth
     `pip install --upgrade pip`
     `pip install "unsloth[cu124-torch260] @ git+https://github.com/unslothai/unsloth.git"`

6. 安装 LLaMA-Factory

      ```
      git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
      cd LLaMA-Factory
      pip install -e ".[torch,metrics]" --no-build-isolation
      ```

7. 下载模型
      ```
      # 1. 确保安装了 modelscope 库
      pip install modelscope
      
      # 2. 执行下载脚本
      python -c "from modelscope import snapshot_download; \
      model_dir = snapshot_download('Qwen/Qwen3-8B', cache_dir='models'); \
      print(f'\n>>> 模型下载完成！绝对路径是: {model_dir} <<<')"
      ```

8. 配置 wandB

    ```
      pip install wandb
      wandb login
      bb73c8bf86aa2392570db183534522d3df087ead
    ```

9. 数据准备

       - sharegpt 格式支持**更多的角色种类**，例如 human、gpt、observation、function 等等
       - 注意其中 human 和 observation 必须出现在奇数位置，gpt 和 function 必须出现在偶数位置。默认所有的 gpt 和 function 会被用于学习。
        
       - [样例数据集](https://github.com/hiyouga/LLaMA-Factory/blob/main/data/glaive_toolcall_zh_demo.json)
        
       ```
       [
         {
           "conversations": [
             {
               "from": "human",
               "value": "用户指令"
             },
             {
               "from": "function_call",
               "value": "工具参数"
             },
             {
               "from": "observation",
               "value": "工具结果"
             },
             {
               "from": "gpt",
               "value": "模型回答"
             }
           ],
           "system": "系统提示词（选填）",
           "tools": "工具描述（选填）"
         }
       ]
       ```

10. 开始训练

     ```
     #!/bin/bash
     
     # === 显卡与环境 ===
     export NCCL_P2P_DISABLE=1
     export NCCL_IB_DISABLE=1
     export CUDA_VISIBLE_DEVICES=1
     
     # === 编译器配置 ===
     export CC=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-gcc
     export CXX=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-g++
     
     # === WandB 配置 ===
     export WANDB_PROJECT="LLaMaFactory"
     
     # === 跳过版本检查 ===
     export DISABLE_VERSION_CHECK=1
     
     # === 网络屏蔽 ===
     export HF_HUB_OFFLINE=1
     export UNSLOTH_NO_UPDATE=1
     export WANDB_MODE=offline
     
     # === 训练命令 ===
     llamafactory-cli train \
         --stage sft \
         --do_train True \
         --model_name_or_path /home/sunsharp/LLaMA-Factory/models/Qwen/Qwen3-8B \
         --preprocessing_num_workers 16 \
         --finetuning_type lora \
         --template qwen \
         --dataset_dir data \
         --dataset intention \
         --cutoff_len 8192 \
         --learning_rate 2e-4 \
         --num_train_epochs 10.0 \
         --max_samples 100000 \
         --per_device_train_batch_size 1 \
         --gradient_accumulation_steps 32 \
         --lr_scheduler_type cosine \
         --logging_steps 2 \
         --save_steps 10 \
         --warmup_steps 5 \
         --packing False \
         --report_to wandb \
         --run_name SFT_V2 \
         --output_dir saves/Qwen3-8B-Intention/lora/SFT_V2 \
         --bf16 True \
         --plot_loss True \
         --trust_remote_code True \
         --ddp_timeout 180000000 \
         --optim adamw_torch \
         --lora_rank 16 \
         --lora_alpha 32 \
         --lora_dropout 0.05 \
         --lora_target all \
         --quantization_bit 4 \
         --gradient_checkpointing True \
         --val_size 0.1 \
         --eval_strategy steps \
         --eval_steps 10 \
         --per_device_eval_batch_size 1 \
         --compute_accuracy True \
         --metric_for_best_model eval_loss \
         --load_best_model_at_end True 
     ```
11. 部署原版模型
     ```
     tmux new -s base_server
     
     
     export CUDA_VISIBLE_DEVICES=1
     export API_PORT=9000
     export HF_HUB_OFFLINE=1
     export USE_MODELSCOPE_HUB=0
     
     llamafactory-cli api \
         --model_name_or_path /home/sunsharp/LLaMA-Factory/models/Qwen/Qwen3-8B \
         --template qwen \
         --infer_backend huggingface \
         --vllm_enforce_eager True

12. 部署微调后的模型

     ```
     tmux new -s sft_server
     
     export CUDA_VISIBLE_DEVICES=1
     export API_PORT=9000
     export HF_HUB_OFFLINE=1
     export USE_MODELSCOPE_HUB=0
     
     llamafactory-cli api \
         --model_name_or_path /home/sunsharp/LLaMA-Factory/models/Qwen/Qwen3-8B \
         --adapter_name_or_path /home/sunsharp/LLaMA-Factory/saves/Qwen3-8B-Intention/lora/SFT_V1/checkpoint-50 \
         --template qwen \
         --infer_backend huggingface \
         --vllm_enforce_eager True
     ```

