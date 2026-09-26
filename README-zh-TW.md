[English](README.md) · [日本語](README-ja.md) · 繁體中文 · [简体中文](README-zh.md)

# Stable Diffusion API Automate

一個功能強大的命令列工具，透過 WebUI API 自動化 Stable Diffusion 圖像生成。可以用不同的設定生成多張圖像，即時監看進度，並將結果連同中繼資料一起儲存。

## 螢幕截圖

### 執行中的 CLI
![CLI 介面](screenshot-cli.png)

彩色終端機介面會顯示：
- 帶有視覺回饋的即時進度條
- 逐步的生成追蹤
- 預估剩餘時間與完成狀態
- 依訊息類型上色的輸出

### 開發環境
![VS Code 整合](screenshot-vscode.png)

這個工具與開發環境配合良好，輸出簡潔，除錯方便。

## 功能

- 批次處理：從 JSONL 檔案處理多組設定
- 即時進度：帶有逐步生成追蹤的即時進度條
- 彩色輸出：由 chalk 提供色彩的美觀終端機介面
- 安全結束：按 'z' 會在目前的生成完成後安全結束
- 有條理的輸出：依日期自動整理目錄
- 儲存中繼資料：可選的 JSON 中繼資料檔，包含生成參數
- 彈性設定：豐富的 CLI 選項方便自訂
- 詳細記錄：帶有時間戳記與檔案路徑的詳細模式

## 安裝

```bash
# Clone or download the project
cd stable-diffusion-api-automate

# Install dependencies
npm install
```

### 先決條件

1. 設定環境變數：

複製範例環境檔並加以修改：
```bash
cp .env.example .env
```

編輯 `.env` 檔案：
```bash
# Required: Your WebUI API URL
SD_WEBUI_URL=http://192.168.100.105:7860

# Optional: Default paths (can be overridden by CLI options)
# CONFIGS_FILE=prompts/configs.jsonl
# OUTPUT_DIR=out
```

2. 啟用 API 啟動 Stable Diffusion WebUI：

```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

`--listen` 旗標允許外部連線，`--api` 則啟用本工具所需的 API 端點。

更詳細的 API 文件請參閱：https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## 快速開始

1. 準備設定檔（`prompts/configs.jsonl`）：
```jsonl
{"prompt": "1girl, solo, green tracksuit, black hair, brown eyes, mischievous smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 2, "hires_fix": false}
{"prompt": "1girl, solo, red dress, blonde hair, blue eyes, gentle smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 1, "hires_fix": false}
```

2. 執行工具：
```bash
npm start
```

3. 監看進度，如需停止請按 'z' 安全結束。

## 用法

### 基本指令

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

### CLI 選項

| 選項 | 說明 | 預設值 |
|--------|-------------|---------|
| `--configs-file <path>` | 設定 JSONL 檔案的路徑 | 來自 `.env` 或 `prompts/configs.jsonl` |
| `--output-dir <path>` | 圖像輸出目錄 | 來自 `.env` 或 `out` |
| `--base-url <url>` | WebUI API 基礎 URL | 來自 `.env` 或 `http://localhost:7860` |
| `--save-meta` | 儲存中繼資料 JSON 檔 | `false` |
| `--disable-log-config` | 停用設定記錄 | `false` |
| `-v, --verbose` | 啟用詳細記錄 | `false` |
| `-h, --help` | 顯示說明 | - |

### 鍵盤操作

- 'z' 或 'Z'：安全結束（完成目前的生成後停止）
- Ctrl+C：強制結束（立即終止）

## 環境設定

本工具使用環境變數進行設定。請在專案根目錄建立 `.env` 檔案：

```bash
# Required: WebUI API URL
SD_WEBUI_URL=http://localhost:7860

# Optional: Default file paths
CONFIGS_FILE=prompts/configs.jsonl
OUTPUT_DIR=out
```

### 設定優先順序

設定依以下順序套用（優先順序由高到低）：
1. 命令列參數（例如 `--base-url http://localhost:8080`）
2. 環境變數（來自 `.env` 檔案）
3. 預設值

### 常用 WebUI URL 範例

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

## 設定檔格式

設定檔應為 JSONL（JSON Lines）格式，每一行包含一個完整的 Stable Diffusion API 請求內容：

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

### 支援的參數

支援所有標準的 Stable Diffusion WebUI API 參數：
- `prompt` - 正向提示詞文字
- `negative_prompt` - 負向提示詞文字
- `width`, `height` - 圖像尺寸
- `sampler_name` - 取樣方法
- `steps` - 取樣步數
- `cfg_scale` - CFG 比例值
- `seed` - 隨機種子（-1 表示隨機）
- `batch_size` - 每批圖像數量
- `n_iter` - 反覆次數
- `hires_fix` - 啟用高解析度修正
- 以及更多……

## 進度監看

本工具提供詳細的即時進度資訊：

### 單次生成顯示
```
🔄 [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 ETA: 12.3s
```

### 多次生成顯示
```
🔄 Iter 1/2: [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 | Overall: [████████░░░░░░░░░░░░░░░░░░░░] 28.6% ETA: 25.1s
```

### 整體進度
```
📸 [████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 30.0% Config 3/10
```

## 輸出結構

生成的檔案依日期整理：

```
out/
└── 2025-08-14/
    ├── 1723648392847-2605429855.png
    ├── 1723648392847-2605429855.meta.json (if --save-meta)
    ├── 1723648394123-2605429856.png
    └── 1723648394123-2605429856.meta.json (if --save-meta)
```

### 檔案命名規則

圖像以 `[timestamp]-[seed].png` 格式命名
- `timestamp`：生成圖像時的 Unix 時間戳記
- `seed`：生成實際使用的種子（來自 API 回應）

### 中繼資料檔

啟用 `--save-meta` 後，每張圖像都會附帶一個 `.meta.json` 檔案，內容包含：

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

## 伺服器需求

- 以 `--listen --api` 旗標執行的 Stable Diffusion WebUI
- 在 `.env` 檔案中設定的 WebUI 端點（見「環境設定」一節）
- 執行本工具的電腦必須能連線到 WebUI

啟用 API 啟動 WebUI：
```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

重要：`--listen` 與 `--api` 兩個旗標都必須提供：
- `--listen`：允許其他電腦連線
- `--api`：啟用 REST API 端點

完整的 API 文件與進階設定選項請前往：  
https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## 疑難排解

### 連線問題
- 確認 WebUI 以 `--api` 旗標執行
- 檢查伺服器的 IP 與連接埠是否正確（預設：`192.168.100.105:7860`）
- 確認防火牆設定允許連線

### 進度沒有更新
- 本工具會自動監看 `/sdapi/v1/progress` 端點
- 如果進度看起來卡住了，伺服器可能負載過重
- 試著減少 `n_iter` 或 `batch_size` 的值

### 生成失敗
- 查看 WebUI 主控台中的錯誤訊息
- 確認提示詞中沒有無效字元
- 確認 WebUI 已載入模型

### 安全結束沒有作用
- 確認終端機支援原始模式（raw mode）輸入
- 試著多按幾次 'z'
- 最後手段是使用 Ctrl+C（目前的生成可能不會被儲存）

## 範例

### 基本用法
```bash
# Generate images with default settings
npm start
```

### 進階用法
```bash
# Custom configuration with metadata saving
npm start -- \
  --configs-file ./my-prompts.jsonl \
  --output-dir ./renders \
  --save-meta \
  --verbose
```

### 安靜模式
```bash
# Minimal output, no config logging
npm start -- --disable-log-config
```

## 貢獻

1. Fork 儲存庫
2. 建立功能分支
3. 進行修改
4. 充分測試
5. 提交 pull request

## 授權

MIT 授權，可自由使用並依需要修改。

