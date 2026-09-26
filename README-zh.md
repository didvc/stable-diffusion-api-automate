[English](README.md) · [日本語](README-ja.md) · [繁體中文](README-zh-TW.md) · 简体中文

# Stable Diffusion API Automate

一个功能强大的命令行工具，通过 WebUI API 自动化 Stable Diffusion 图像生成。可以用不同的配置生成多张图像，实时监控进度，并将结果连同元数据一起保存。

## 截图

### 运行中的 CLI
![CLI 界面](screenshot-cli.png)

彩色终端界面会显示：
- 带有可视反馈的实时进度条
- 逐步的生成跟踪
- 预计剩余时间和完成状态
- 按消息类型着色的输出

### 开发环境
![VS Code 集成](screenshot-vscode.png)

该工具与开发环境配合良好，输出简洁，调试方便。

## 功能

- 批量处理：从 JSONL 文件处理多个配置
- 实时进度：带逐步生成跟踪的实时进度条
- 彩色输出：由 chalk 提供颜色的美观终端界面
- 安全退出：按 'z' 在当前生成完成后安全退出
- 有序输出：按日期自动整理目录
- 保存元数据：可选的 JSON 元数据文件，包含生成参数
- 灵活配置：丰富的 CLI 选项便于定制
- 详细日志：带时间戳和文件路径的详细模式

## 安装

```bash
# Clone or download the project
cd stable-diffusion-api-automate

# Install dependencies
npm install
```

### 前提条件

1. 配置环境变量：

复制示例环境文件并进行修改：
```bash
cp .env.example .env
```

编辑 `.env` 文件：
```bash
# Required: Your WebUI API URL
SD_WEBUI_URL=http://192.168.100.105:7860

# Optional: Default paths (can be overridden by CLI options)
# CONFIGS_FILE=prompts/configs.jsonl
# OUTPUT_DIR=out
```

2. 启用 API 启动 Stable Diffusion WebUI：

```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

`--listen` 参数允许外部连接，`--api` 启用本工具所需的 API 端点。

更详细的 API 文档请参阅：https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## 快速开始

1. 准备配置文件（`prompts/configs.jsonl`）：
```jsonl
{"prompt": "1girl, solo, green tracksuit, black hair, brown eyes, mischievous smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 2, "hires_fix": false}
{"prompt": "1girl, solo, red dress, blonde hair, blue eyes, gentle smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 1, "hires_fix": false}
```

2. 运行工具：
```bash
npm start
```

3. 监控进度，如需停止请按 'z' 安全退出。

## 用法

### 基本命令

```bash
# Run with default settings
npm start

# Show help
npm run help

# Run with verbose logging
npm start -- -v

# Use custom config file and output directory
npm start -- --configs-file custom.jsonl --output-dir ./my-images

# Save metadata files
npm start -- --save-meta

# Quiet mode (no config logging)
npm start -- --disable-log-config
```

### CLI 选项

| 选项 | 说明 | 默认值 |
|--------|-------------|---------|
| `--configs-file <path>` | 配置 JSONL 文件的路径 | 来自 `.env` 或 `prompts/configs.jsonl` |
| `--output-dir <path>` | 图像输出目录 | 来自 `.env` 或 `out` |
| `--base-url <url>` | WebUI API 基础 URL | 来自 `.env` 或 `http://localhost:7860` |
| `--save-meta` | 保存元数据 JSON 文件 | `false` |
| `--disable-log-config` | 禁用配置日志 | `false` |
| `-v, --verbose` | 启用详细日志 | `false` |
| `-h, --help` | 显示帮助信息 | - |

### 键盘控制

- 'z' 或 'Z'：安全退出（完成当前生成后停止）
- Ctrl+C：强制退出（立即终止）

## 环境配置

本工具使用环境变量进行配置。请在项目根目录创建 `.env` 文件：

```bash
# Required: WebUI API URL
SD_WEBUI_URL=http://localhost:7860

# Optional: Default file paths
CONFIGS_FILE=prompts/configs.jsonl
OUTPUT_DIR=out
```

### 配置优先级

设置按以下顺序生效（优先级从高到低）：
1. 命令行参数（例如 `--base-url http://localhost:8080`）
2. 环境变量（来自 `.env` 文件）
3. 默认值

### 常用 WebUI URL 示例

```bash
# Local WebUI (default port)
SD_WEBUI_URL=http://localhost:7860

# Local WebUI with custom port
SD_WEBUI_URL=http://localhost:8080

# Remote WebUI on local network
SD_WEBUI_URL=http://192.168.1.100:7860

# Remote WebUI with custom port
SD_WEBUI_URL=http://192.168.1.100:8080
```

## 配置文件格式

配置文件应为 JSONL（JSON Lines）格式，每行包含一个完整的 Stable Diffusion API 请求体：

```jsonl
{
  "prompt": "your positive prompt here",
  "negative_prompt": "your negative prompt here", 
  "width": 720,
  "height": 1280,
  "sampler_name": "Euler a",
  "steps": 28,
  "cfg_scale": 7,
  "seed": -1,
  "batch_size": 1,
  "n_iter": 2,
  "hires_fix": false
}
```

### 支持的参数

支持所有标准的 Stable Diffusion WebUI API 参数：
- `prompt` - 正向提示词文本
- `negative_prompt` - 反向提示词文本
- `width`, `height` - 图像尺寸
- `sampler_name` - 采样方法
- `steps` - 采样步数
- `cfg_scale` - CFG 比例值
- `seed` - 随机种子（-1 表示随机）
- `batch_size` - 每批图像数量
- `n_iter` - 迭代次数
- `hires_fix` - 启用高分辨率修复
- 以及更多……

## 进度监控

本工具提供详细的实时进度信息：

### 单次迭代显示
```
🔄 [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 ETA: 12.3s
```

### 多次迭代显示
```
🔄 Iter 1/2: [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 | Overall: [████████░░░░░░░░░░░░░░░░░░░░] 28.6% ETA: 25.1s
```

### 总体进度
```
📸 [████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 30.0% Config 3/10
```

## 输出结构

生成的文件按日期整理：

```
out/
└── 2025-08-14/
    ├── 1723648392847-2605429855.png
    ├── 1723648392847-2605429855.meta.json (if --save-meta)
    ├── 1723648394123-2605429856.png
    └── 1723648394123-2605429856.meta.json (if --save-meta)
```

### 文件命名规则

图像按 `[timestamp]-[seed].png` 格式命名
- `timestamp`：生成图像时的 Unix 时间戳
- `seed`：生成实际使用的种子（来自 API 响应）

### 元数据文件

启用 `--save-meta` 后，每张图像都会附带一个 `.meta.json` 文件，其中包含：

```json
{
  "config": {
    "prompt": "...",
    "negative_prompt": "...",
    "width": 720,
    // ... full generation parameters
  },
  "response": {
    "info": {
      "seed": 2605429855,
      // ... API response metadata
    },
    "timestamp": "2025-08-14T12:34:56.789Z",
    "filename": "1723648392847-2605429855.png",
    "seed": 2605429855
  }
}
```

## 服务器要求

- 以 `--listen --api` 参数运行的 Stable Diffusion WebUI
- 在 `.env` 文件中配置的 WebUI 端点（见“环境配置”一节）
- 运行本工具的机器必须能够访问 WebUI

启用 API 启动 WebUI：
```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

重要：`--listen` 和 `--api` 两个参数都必须提供：
- `--listen`：允许其他机器连接
- `--api`：启用 REST API 端点

完整的 API 文档和高级配置选项请访问：  
https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## 故障排除

### 连接问题
- 确认 WebUI 以 `--api` 参数运行
- 检查服务器 IP 和端口是否正确（默认：`192.168.100.105:7860`）
- 确认防火墙设置允许连接

### 进度不更新
- 本工具会自动监控 `/sdapi/v1/progress` 端点
- 如果进度看起来卡住了，服务器可能负载过高
- 尝试减小 `n_iter` 或 `batch_size` 的值

### 生成失败
- 查看 WebUI 控制台中的错误信息
- 确认提示词中没有无效字符
- 确认 WebUI 已加载模型

### 安全退出无效
- 确认终端支持原始模式（raw mode）输入
- 尝试多按几次 'z'
- 最后手段是使用 Ctrl+C（当前生成可能不会被保存）

## 示例

### 基本用法
```bash
# Generate images with default settings
npm start
```

### 高级用法
```bash
# Custom configuration with metadata saving
npm start -- \
  --configs-file ./my-prompts.jsonl \
  --output-dir ./renders \
  --save-meta \
  --verbose
```

### 安静模式
```bash
# Minimal output, no config logging
npm start -- --disable-log-config
```

## 贡献

1. Fork 仓库
2. 创建功能分支
3. 进行修改
4. 充分测试
5. 提交 pull request

## 许可证

MIT 许可证，可以自由使用并按需修改。

