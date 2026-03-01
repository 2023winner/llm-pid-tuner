# 基于 LLM 的 PID 自动调参系统 (LLM-PID-Tuner)

[![Star History Chart](https://api.star-history.com/svg?repos=KINGSTON-115/llm-pid-tuner&type=Date)](https://star-history.com/#KINGSTON-115/llm-pid-tuner)

> 📺 [B站教程视频](https://b23.tv/WVUuIFb) - 详细视频教程手把手教你使用

这是一个结合了大型语言模型 (LLM) 的 PID 自动调参系统。它通过分析控制系统的实时数据，利用 AI 的逻辑推理能力自动优化 PID 参数（Kp, Ki, Kd），支持**本地仿真测试**和**真实硬件调参**。

---

## 📂 文件作用说明

| 文件名 | 作用 | 核心功能 |
| :--- | :--- | :--- |
| **`simulator.py`** | **本地仿真调参** | 无需硬件，在电脑上运行热力学模型进行 PID 算法测试和 AI 调参演示。 |
| **`tuner.py`** | **真实硬件调参** | 电脑端上位机，通过串口与 MCU 通信，将真实传感器数据发给 AI 并更新硬件参数。 |
| **`firmware.cpp`** | **MCU 固件** | 运行在 Arduino/ESP32 等单片机上的代码，执行 PID 控制并与 `tuner.py` 通信。 |
| **`system_id.py`** | **系统辨识工具** | 辅助脚本，使用 Ziegler-Nichols (Z-N) 法对系统进行初步辨识，为 AI 提供参考。 |

---

## 🛠️ 使用场景指南

### 场景一：我想先学习或测试 PID 算法 (无需硬件)
*   **使用文件**：`simulator.py`
*   **操作流程**：
    1. 配置环境变量（API Key 等）。
    2. 直接运行 `python simulator.py`。
    3. 观察 AI 如何在虚拟热力学模型中一步步将温度稳定在 100°C。

### 场景二：我想给我的真实设备（如 3D 打印机加热头）调参
*   **使用文件**：`firmware.cpp` + `tuner.py`
*   **操作流程**：
    1. 将 `firmware.cpp` 修改并烧录到你的开发板（Arduino/ESP32 等）。
    2. 连接开发板到电脑，确认串口号（如 COM3 或 /dev/ttyUSB0）。
    3. 修改 `tuner.py` 中的 `SERIAL_PORT` 和 API 配置。
    4. 运行 `python tuner.py`，AI 将接管你的硬件进行实时调参。

---

## 🚀 快速开始

### 1. 安装依赖
```bash
pip install requests serial  # 如果使用真实硬件需安装 pyserial
```

### 2. 配置 LLM API (支持多种模型)
项目支持所有 OpenAI 兼容的 API（如 GPT-4, DeepSeek, MiniMax）以及本地模型和 Claude。

**方式 A：通过环境变量配置（推荐，安全且灵活）**
```powershell
# Windows (PowerShell)
$env:LLM_API_BASE_URL="https://api.openai.com/v1"
$env:LLM_MODEL_NAME="gpt-4"
$env:LLM_API_KEY="your-api-key"

# 如果使用本地 Ollama
$env:LLM_API_BASE_URL="http://localhost:11434/v1"
$env:LLM_MODEL_NAME="llama3"
$env:LLM_API_KEY="ollama"

# 如果使用 Claude 原生 API
$env:LLM_PROVIDER="anthropic"
$env:LLM_API_BASE_URL="https://api.anthropic.com/v1"
$env:LLM_MODEL_NAME="claude-3-5-sonnet-20240620"
```

**方式 B：直接修改文件代码**
在 `simulator.py` 或 `tuner.py` 顶部的 `全局配置` 区域直接修改变量值。

### 3. 运行本地仿真
```bash
python simulator.py
```

---

## 🤖 调参适配说明

| 适配对象 | API 地址示例 | 说明 |
| :--- | :--- | :--- |
| **Ollama** | `http://localhost:11434/v1` | 兼容 OpenAI 格式，推荐 `llama3` 或 `qwen2` |
| **LM Studio** | `http://localhost:1234/v1` | 开启 Local Server 后即可使用 |
| **Claude 原生** | `https://api.anthropic.com/v1` | 需设置 `LLM_PROVIDER="anthropic"` |
| **昇腾 (Ascend)** | 需配合推理框架 | 使用 MindSpore Serving 或 vLLM 部署后提供兼容端点 |

---

## 📈 AI 调参逻辑 (System Prompt)

AI 调参器被赋予了专业的控制工程知识，遵循以下逻辑：
- **震荡剧烈** (Oscillation) → 减小 Kp 或增大 Kd。
- **响应太慢** (Slow Response) → 增大 Kp。
- **稳态误差** (Steady-State Error) → 增大 Ki。
- **超调过大** (Overshoot) → 大幅减小 Kp 或增大 Kd。
- **重要原则**：**禁止超调**（在某些系统中超调可能导致损坏）且**稳态优先**。

---

## ⚠️ 注意事项
1. **安全第一**：在真实硬件上使用时，请务必设置硬件级别的过温保护或紧急停止按钮。
2. **串口占用**：运行 `tuner.py` 前请关闭 Arduino IDE 的串口监视器，否则会发生冲突。
3. **收敛标准**：当误差小于 0.3°C 且稳定时，AI 会自动返回 `status: DONE` 结束调参。

---

## 📜 许可证
[MIT License](LICENSE)
