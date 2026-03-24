# 基于 LLM 的 PID 自动调参系统 (LLM-PID-Tuner)

这是一个结合了大型语言模型 (LLM) 的 PID 自动调参系统。它通过分析控制系统的实时数据，利用 AI 的逻辑推理能力自动优化 PID 参数（Kp, Ki, Kd），支持**本地仿真测试**和**真实硬件调参**。

## ✨ 功能特点

- 🤖 AI 智能调参 - 利用 LLM 进行参数优化
- 📊 实时数据分析 - 监控系统响应并调整参数
- 🎯 支持仿真和真实硬件 - 灵活切换模式
- 📈 可视化界面 - 实时显示调参过程
- 🔧 易于集成 - 支持多种硬件平台

## 📦 安装

1. 克隆仓库：
```bash
git clone https://github.com/2023winner/llm-pid-tuner.git
cd llm-pid-tuner
```

2. 安装依赖：
```bash
pip install -r requirements.txt
```

3. 配置 LLM API（如需要）：
```bash
cp config.example.py config.py
# 编辑 config.py 填入你的 API 密钥
```
## 🚀 使用方法

### 基本使用

```python
from llm_pid_tuner import PIDTuner

tuner = PIDTuner()
optimal_params = tuner.tune(system_data)
print(f"最优参数：{optimal_params}")
```

### 仿真模式

```bash
python examples/simulation.py
```

### 真实硬件模式

```bash
python examples/hardware.py
```
## 🛠️ 技术栈

- Python
- LLM API
- PID Control
- Data Analysis

## 📁 目录结构

```
llm-pid-tuner/
├── src/              # 源代码目录
├── docs/             # 文档目录
├── tests/            # 测试文件
├── examples/         # 示例代码
├── README.md         # 项目说明
└── .gitignore        # Git 忽略文件
```

## ⚠️ 注意事项

使用真实硬件时请确保连接稳定，建议先在仿真模式下测试。
## 📄 许可证

本项目采用 MIT 许可证 - 详见 LICENSE 文件
