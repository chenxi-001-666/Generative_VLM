# 🏥 Generative Rationale-VLM: Transparent Medical Visual Question Answering
# 🏥 生成式推理-VLM：透明化医学视觉问答

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![CUDA 11.8+](https://img.shields.io/badge/CUDA-11.8+-green.svg)](https://developer.nvidia.com/cuda-toolkit)

**Generative Rationale-VLM** is an explainable medical visual question answering (VQA) framework that replaces "black-box" predictions with transparent 6-step clinical reasoning chains aligned with diagnostic protocols.

**生成式推理-VLM** 是一个可解释的医学视觉问答（VQA）框架，用符合诊断协议的透明化6步临床推理链取代"黑盒"预测。

---

## 🎯 Key Features / 主要特点

| English | 中文 |
|---------|------|
| **6-Step Clinical Reasoning**: Generates transparent diagnostic chains: morphology → location → size → density → infiltration → malignancy risk | **6步临床推理**：生成透明诊断链：形态 → 位置 → 大小 → 密度 → 浸润 → 恶性风险 |
| **Explainability Metrics**: Novel evaluation metrics (RIO, RQR, CLC) for medical interpretability | **可解释性指标**：用于医学可解释性的新型评估指标（RIO、RQR、CLC） |
| **Hallucination Detection**: Real-time verification against medical knowledge bases | **幻觉检测**：基于医学知识库的实时验证 |
| **Cross-Modal Distillation**: Balances inference efficiency with explanation quality | **跨模态蒸馏**：平衡推理效率与解释质量 |
| **Clinical Validation**: Reduces physician decision time by 27%, improves diagnostic accuracy by 13.8% | **临床验证**：减少医生决策时间27%，提高诊断准确率13.8% |

---

## 📊 Performance Highlights / 性能亮点

| Metric | Rationale-VLM | CNN-Attention Baseline | Improvement |
|--------|---------------|------------------------|-------------|
| 指标 | 推理-VLM | CNN-注意力基线 | 提升 |
| Accuracy (PathVQA) / 准确率 | **84.7%** | 76.3% | +8.4% |
| RIO (Image Alignment) / 图像对齐度 | **0.83** | 0.58 | +43% |
| RQR (Semantic Relevance) / 语义相关性 | **0.87** | 0.62 | +40% |
| Physician Decision Time / 医生决策时间 | **-27%** | Baseline / 基线 | |
| Diagnostic Accuracy / 诊断准确率 | **+13.8%** | Baseline / 基线 | |

---

## 🚀 Quick Start / 快速开始

### Installation / 安装

```bash
# Clone repository / 克隆仓库
git clone https://github.com/chenxi-001-666/Generative_VLM.git
cd Generative_VLM

# Install dependencies / 安装依赖
pip install -r requirements.txt
```

---

## ⚡ GPU Acceleration Setup / GPU加速配置

**Note**: Running on CPU is extremely slow (~10-20x slower). Follow these steps to configure CUDA environment for GPU acceleration.

**注意**：在CPU上运行极慢（慢约10-20倍）。请按照以下步骤配置CUDA环境以实现GPU加速。

---

### Step 1: Check Your GPU Compatibility / 检查GPU兼容性

```bash
# Check if you have NVIDIA GPU / 检查是否有NVIDIA GPU
nvidia-smi

# Expected output should show GPU model and CUDA version
# 预期输出应显示GPU型号和CUDA版本
# If not found, you may not have NVIDIA GPU or drivers installed
# 如果未找到，可能没有NVIDIA GPU或未安装驱动
```

---

### Step 2: Install Miniconda (if not installed) / 安装Miniconda

Download Miniconda from [Miniconda website](https://docs.conda.io/en/latest/miniconda.html)
从 [Miniconda官网](https://docs.conda.io/en/latest/miniconda.html) 下载Miniconda

**Windows:**
```powershell
# Download and run the Miniconda installer
# 下载并运行Miniconda安装程序
# After installation, restart your terminal
# 安装完成后，重启终端
```

**Linux/Mac:**
```bash
# Download Miniconda / 下载Miniconda
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
# Install / 安装
bash Miniconda3-latest-Linux-x86_64.sh
# Follow prompts, then restart terminal / 按照提示操作，然后重启终端
```

---

### Step 3: Create Conda Environment with CUDA Support / 创建支持CUDA的Conda环境

```bash
# Create new conda environment with Python 3.10 / 创建Python 3.10的conda环境
conda create -n generative_vlm python=3.10 -y

# Activate environment / 激活环境
conda activate generative_vlm

# Install PyTorch with CUDA 11.8 (adjust based on your CUDA version)
# 安装支持CUDA 11.8的PyTorch（根据你的CUDA版本调整）
# Check your CUDA version with: nvidia-smi
# 使用 nvidia-smi 检查你的CUDA版本
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.8 -c pytorch -c nvidia

# Verify CUDA is available / 验证CUDA是否可用
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'CUDA version: {torch.version.cuda}')"
```

---

### Step 4: Install Project Dependencies in Conda Environment / 在Conda环境中安装项目依赖

```bash
# Navigate to project directory / 进入项目目录
cd Generative_VLM

# Install remaining dependencies / 安装剩余依赖
pip install -r requirements.txt

# Install additional CUDA-optimized packages / 安装额外的CUDA优化包
pip install nvidia-cudnn-cu11==8.9.4.25  # For cuDNN acceleration / cuDNN加速
pip install nvidia-cublas-cu11==11.11.3.6  # For cuBLAS acceleration / cuBLAS加速
```

---

### Step 5: Verify GPU Setup / 验证GPU配置

```bash
# Run verification script / 运行验证脚本
python -c "
import torch
print(f'PyTorch version: {torch.__version__}')
print(f'CUDA available: {torch.cuda.is_available()}')
if torch.cuda.is_available():
    print(f'GPU device: {torch.cuda.get_device_name(0)}')
    print(f'CUDA version: {torch.version.cuda}')
    print(f'Memory allocated: {torch.cuda.memory_allocated(0)/1e9:.2f} GB')
    print(f'Memory cached: {torch.cuda.memory_reserved(0)/1e9:.2f} GB')
else:
    print('WARNING: CUDA not available. Training will be VERY slow on CPU!')
    print('警告：CUDA不可用。在CPU上训练将非常慢！')
"

# Expected output if successful / 成功时的预期输出:
# PyTorch version: 2.0.1
# CUDA available: True
# GPU device: NVIDIA GeForce RTX 4090 (or your GPU model / 或你的GPU型号)
# CUDA version: 11.8
```

---

### Step 6: Performance Comparison / 性能对比

**CPU vs GPU Training Time / CPU vs GPU训练时间:**
- **CPU only / 仅CPU**: ~8-12 hours per epoch / 每轮8-12小时 (estimated / 估计)
- **GPU (NVIDIA RTX 4090)**: ~20-30 minutes per epoch / 每轮20-30分钟
- **Speedup / 加速比**: 20-30x faster with GPU / GPU加速20-30倍

---

## 🗂️ Dataset Preparation / 数据集准备

The framework supports multiple medical VQA datasets. Choose your approach:

该框架支持多个医学VQA数据集。选择你的方法：

### Option 1: Using Preprocessed Features (Recommended for Training)
### 选项1：使用预处理特征（推荐用于训练）

If you want to use the pre-extracted image features:
如果你想使用预提取的图像特征：

1. Download the processed dataset from our repository / 从我们的仓库下载处理后的数据集
2. Place `.npy` feature files in `data/processed/images/` / 将 `.npy` 特征文件放入 `data/processed/images/`
3. Place metadata in `data/raw/metadata.csv` / 将元数据放入 `data/raw/metadata.csv`

### Option 2: Download and Preprocess Raw Datasets
### 选项2：下载并预处理原始数据集

For PathVQA dataset:
对于 PathVQA 数据集：

#### Step 1: Get Hugging Face Access Token / 获取 Hugging Face 访问令牌

1. Visit [Hugging Face](https://huggingface.co/) and create an account / 访问并创建账户
2. Go to Settings → Access Tokens / 前往 设置 → 访问令牌
3. Create a new token with "read" permissions / 创建具有"读取"权限的新令牌
4. Set the token as environment variable / 设置令牌为环境变量：
   ```bash
   # Windows
   set HF_TOKEN=your_token_here
   
   # Linux/Mac
   export HF_TOKEN=your_token_here
   ```

#### Step 2: Download Raw Data / 下载原始数据

```bash
# Run the download script (automatically handles authentication)
# 运行下载脚本（自动处理认证）
python download_data.py
```

#### Step 3: Extract Image Features / 提取图像特征

```bash
# Extract visual features using BLIP-2 encoder (GPU-accelerated)
# 使用BLIP-2编码器提取视觉特征（GPU加速）
python data/preprocess.py --dataset pathvqa --output_dir data/processed --device cuda
```

---

## 🏋️ Model Training / 模型训练

### Activate Conda Environment Before Training / 训练前激活Conda环境

```bash
# Always activate conda environment first / 始终先激活conda环境
conda activate generative_vlm

# Navigate to project directory / 进入项目目录
cd D:\MedVQAProjects\Generative_VLM
```

### Train Rationale-VLM (GPU-accelerated) / 训练推理-VLM（GPU加速）

```bash
# Train Rationale-VLM with CUDA / 使用CUDA训练推理-VLM
python experiments/train.py --config experiments/config.yaml --device cuda --gpu_id 0

# Monitor GPU usage during training / 训练期间监控GPU使用情况
nvidia-smi -l 1  # Updates every second / 每秒更新
```

### Train Baseline Model / 训练基线模型

```bash
# Train baseline model / 训练基线模型
python experiments/train.py --config experiments/config_baseline.yaml --device cuda
```

### Training with Multiple GPUs (if available) / 多GPU训练（如果可用）

```bash
# Use DataParallel for multiple GPUs / 使用DataParallel进行多GPU训练
python experiments/train.py --config experiments/config.yaml --device cuda --gpu_ids 0,1

# Use DistributedDataParallel for larger scale / 使用DistributedDataParallel进行大规模训练
python experiments/train.py --config experiments/config.yaml --device cuda --distributed
```

---

## 🔍 Inference / 推理

```bash
# Interactive inference (GPU-accelerated) / 交互式推理（GPU加速）
python experiments/infer.py --image_path <path_to_image> --question "<clinical_question>" --device cuda

# Batch inference / 批量推理
python experiments/batch_infer.py --input_csv test_cases.csv --output_csv results.csv --device cuda

# Performance benchmark / 性能基准测试
python experiments/benchmark.py --model rationale_vlm --device cuda --batch_sizes 1,4,8,16
```

---

## 📁 Project Structure / 项目结构

```
Generative_VLM/
├── .venv/                         # Python virtual environment / Python虚拟环境
├── data/                          # Data processing / 数据处理
│   ├── __init__.py               # Data package init / 数据包初始化
│   ├── preprocess.py             # Data preprocessing script / 数据预处理脚本
│   ├── processed/                # Processed features / 处理后的特征
│   └── raw/                      # Raw datasets / 原始数据集
├── experiments/                   # Training and evaluation / 训练与评估
│   ├── baseline_infer.py         # Baseline inference / 基线推理
│   ├── compare_results.py        # Result comparison / 结果比较
│   ├── config.yaml               # Main configuration / 主配置文件
│   ├── debug_dataloader.py       # DataLoader debugging / 数据加载器调试
│   ├── infer.py                  # Inference script / 推理脚本
│   ├── optimization_report.txt   # Optimization report / 优化报告
│   ├── physician_evaluation.py   # Physician evaluation / 医生评估
│   ├── start_training.py         # Training entry point / 训练入口
│   ├── train.py                  # Main training script / 主训练脚本
│   └── verify_config.py          # Config verification / 配置验证
├── metrics/                       # Evaluation metrics / 评估指标
│   ├── __init__.py              # Metrics package init / 指标包初始化
│   ├── clc.py                   # Clinical Logic Consistency / 临床逻辑一致性
│   ├── rio.py                   # Rationale-Image Overlap / 推理-图像重叠度
│   ├── robustness.py            # Robustness evaluation / 鲁棒性评估
│   └── rqr.py                   # Rationale-Question Relevance / 推理-问题相关性
├── models/                        # Model architectures / 模型架构
│   ├── __init__.py              # Models package init / 模型包初始化
│   ├── rationale_vlm.py         # Main Rationale-VLM / 主推理-VLM模型
│   ├── student_modules.py       # Distilled modules / 蒸馏模块
│   └── modules/                 # Sub-modules / 子模块
│       ├── __init__.py          # Modules package init / 模块包初始化
│       ├── dynamic_weight.py    # Dynamic weighting / 动态权重
│       └── hallucination.py     # Hallucination detection / 幻觉检测
├── tests/                        # Unit tests / 单元测试
│   ├── __init__.py              # Tests package init / 测试包初始化
│   └── test_imports.py          # Import testing / 导入测试
├── utils/                        # Utility functions / 工具函数
├── .gitattributes               # Git attributes / Git属性
├── .gitignore                   # Git ignore file / Git忽略文件
├── download_data.py             # Data download script / 数据下载脚本
├── install_final.py             # Installation script / 安装脚本
├── LICENSE                      # MIT License / MIT许可证
├── README.md                    # Project documentation / 项目文档
├── requirements.txt             # Python dependencies / Python依赖
└── setup.py                     # Package setup / 包设置
```

---

## 🚨 Troubleshooting GPU Issues / GPU问题排查

### Common Problems and Solutions / 常见问题与解决方案

| Problem / 问题 | Solution / 解决方案 |
|----------------|---------------------|
| **"CUDA not available" error** / **"CUDA不可用"错误** | Check CUDA toolkit installation: `nvcc --version`<br>Reinstall PyTorch with correct CUDA version:<br>`conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia` |
| **Out of memory error** / **内存不足错误** | Reduce batch size in config.yaml / 减少config.yaml中的batch_size<br>Use gradient accumulation / 使用梯度累积:<br>`gradient_accumulation_steps: 2` |
| **Slow GPU performance** / **GPU性能慢** | Enable cuDNN benchmarking / 启用cuDNN基准测试:<br>`export CUDNN_BENCHMARK=1`<br>Use mixed precision training / 使用混合精度训练:<br>`python experiments/train.py --fp16` |
| **Driver compatibility issues** / **驱动兼容性问题** | Update NVIDIA drivers / 更新NVIDIA驱动<br>Visit: https://www.nvidia.com/Download/index.aspx<br>Driver Version should be >= 525.60.11 for CUDA 11.8<br>对于CUDA 11.8，驱动版本应 >= 525.60.11 |

---

## 📊 Supported Datasets / 支持的数据集

| Dataset / 数据集 | Modality / 模态 | Samples / 样本量 | Questions / 问题量 | GPU Preprocessing Time / GPU预处理时间 |
|-----------------|-----------------|------------------|--------------------|----------------------------------------|
| PathVQA | Pathology / 病理学 | 4,998 | 32,799 | ~15 mins (GPU) / ~2 hours (CPU) |
| VQA-RAD | Radiology / 放射学 | 315 | 3,515 | ~2 mins (GPU) / ~20 mins (CPU) |
| SLAKE | Multimodal / 多模态 | 642 | 14,028 | ~5 mins (GPU) / ~45 mins (CPU) |

---

## 🔧 Configuration / 配置

Edit `experiments/config.yaml` to customize:
编辑 `experiments/config.yaml` 进行自定义：

```yaml
# Hardware configuration / 硬件配置
hardware:
  device: "cuda"  # or "cpu" / 或 "cpu"
  gpu_id: 0
  num_workers: 4  # DataLoader workers / 数据加载器工作进程数
  pin_memory: true  # Faster data transfer to GPU / 加速数据传输到GPU

# Mixed precision training (faster, less memory) / 混合精度训练（更快，更少内存）
training:
  use_amp: true  # Automatic Mixed Precision / 自动混合精度
  gradient_accumulation_steps: 1
  
# Model configuration / 模型配置
model:
  name: "rationale_vlm"
  hidden_size: 768
  num_attention_heads: 12

# Training parameters / 训练参数
training:
  batch_size: 16  # Adjust based on GPU memory / 根据GPU内存调整
  learning_rate: 1e-5
  num_epochs: 20

# Data configuration / 数据配置
data:
  dataset: "pathvqa"
  image_size: 224
  max_question_length: 64
```

---

## 📈 Evaluation / 评估

```bash
# Run comprehensive evaluation (GPU-accelerated) / 运行综合评估（GPU加速）
python experiments/evaluate.py --model rationale_vlm --dataset pathvqa --device cuda

# Generate detailed report / 生成详细报告
python experiments/generate_report.py --output report.pdf

# Benchmark performance / 性能基准测试
python experiments/benchmark.py --compare cpu cuda
```

---

## 🧪 Experiments Reproducibility / 实验可复现性

To reproduce the paper results with GPU acceleration:
使用GPU加速复现论文结果：

```bash
# 1. Setup conda environment with CUDA / 设置带CUDA的conda环境
conda create -n generative_vlm python=3.10 pytorch=2.0.1 torchvision=0.15.2 cudatoolkit=11.8 -c pytorch -c nvidia
conda activate generative_vlm

# 2. Install project dependencies / 安装项目依赖
pip install -r requirements.txt

# 3. Download and preprocess data (GPU accelerated) / 下载并预处理数据（GPU加速）
python download_data.py
python data/preprocess.py --device cuda

# 4. Train models (GPU accelerated) / 训练模型（GPU加速）
python experiments/train.py --config experiments/config.yaml --device cuda  # Rationale-VLM / 推理-VLM
python experiments/train.py --config experiments/config_baseline.yaml --device cuda  # Baseline / 基线

# 5. Run evaluations / 运行评估
python experiments/run_all_experiments.py --device cuda
```

---

## 📝 Citation / 引用

If you use this work, please cite:
如果您使用本工作，请引用：

```bibtex
@article{chen2025generative,
  title={Comparative Study of Explainability in Generative VLM vs CNN Baseline Models for Medical Visual Question Answering},
  author={Chen, Xi and Zhuo, Ziyue},
  journal={Advance Machine Learning (WOA7015)},
  year={2025}
}
```

---

## 👥 Authors / 作者

- **CHEN XI** (25053692)
- **ZHUO ZIYUE** (24083635)

---

## 📄 License / 许可证

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

本项目采用MIT许可证 - 详见 [LICENSE](LICENSE) 文件。

---

## 🙏 Acknowledgments / 致谢

- PathVQA, VQA-RAD, and SLAKE dataset creators / PathVQA、VQA-RAD和SLAKE数据集的创建者
- Hugging Face for dataset hosting / Hugging Face提供的数据集托管
- The 5 participating physicians for clinical evaluations / 参与临床评估的5位医生
- NVIDIA for CUDA acceleration technology / NVIDIA提供的CUDA加速技术
- All contributors to open-source medical AI research / 所有开源医学AI研究的贡献者

---

**Note**: This is a research project. The model outputs should be used as decision support only, not as definitive medical diagnosis. Always consult with qualified healthcare professionals for medical decisions.

**注意**：这是一个研究项目。模型输出应仅用作决策支持，而非确定性医学诊断。对于医疗决策，请始终咨询合格的医疗专业人员。
