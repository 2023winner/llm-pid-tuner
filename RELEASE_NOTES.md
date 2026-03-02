# Release v2.0 PRO - 硬件调参专用版

🎉 我们非常高兴地发布 LLM-PID-Tuner v2.0 PRO 的独立可执行版本！

## 📦 这个版本包含什么？

此 Release 包含打包好的 `llm-pid-tuner.exe`，它是专为**真实硬件调参**设计的上位机程序。
*   **无需 Python 环境**：直接双击运行，不再需要安装 Python 或任何依赖库。
*   **交互式串口选择**：程序会自动扫描并列出当前连接的串口设备，您可以轻松选择目标设备。
*   **增强版内核**：内置了最新的 History-Aware 和 Chain-of-Thought 调参逻辑。

## 🚀 快速开始

1.  **下载**：点击下方 Assets 中的 `llm-pid-tuner.exe` 进行下载。
2.  **准备硬件**：将您的单片机（如 Arduino, ESP32）连接到电脑。
3.  **运行**：双击 `llm-pid-tuner.exe`。
    *   如果是首次运行，程序会自动生成一个 `config.json` 文件。
4.  **配置**：用记事本打开 `config.json`，填入您的 API Key 和其他设置：
    ```json
    {
        "SERIAL_PORT": "AUTO",
        "BAUD_RATE": 115200,
        "LLM_API_KEY": "sk-您的Key",
        "LLM_API_BASE_URL": "https://api.openai.com/v1",
        "LLM_MODEL_NAME": "gpt-4"
    }
    ```
5.  **重启**：保存文件后，重新运行程序即可。

## 📝 配置说明

*   **`SERIAL_PORT`**: 默认为 `"AUTO"`（自动扫描），也可指定如 `"COM3"`。
*   **`LLM_API_BASE_URL`**: 支持 OpenAI, DeepSeek, MiniMax 等所有兼容 OpenAI 格式的接口。
*   **环境变量**: 依然支持环境变量（优先级高于配置文件），方便脚本调用。

---
*Happy Tuning!* 🎛️
