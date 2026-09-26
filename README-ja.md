[English](README.md) · 日本語 · [繁體中文](README-zh-TW.md) · [简体中文](README-zh.md)

# Stable Diffusion API Automate

WebUI API を通じて Stable Diffusion の画像生成を自動化する、強力なコマンドラインツールです。異なる設定で複数の画像を生成し、進捗をリアルタイムで確認し、結果をメタデータとともに保存できます。

## スクリーンショット

### 動作中の CLI
![CLI インターフェース](screenshot-cli.png)

色分けされたターミナル画面では、次の内容が表示されます。
- 視覚的なフィードバック付きのリアルタイム進捗バー
- ステップごとの生成の追跡
- 残り時間の見積もりと完了状況
- メッセージの種類ごとに色分けされた出力

### 開発環境
![VS Code との連携](screenshot-vscode.png)

開発環境との相性がよく、すっきりした出力で簡単にデバッグできます。

## 機能

- バッチ処理：JSONL ファイルから複数の設定を処理
- リアルタイム進捗：ステップごとの生成を追跡するライブ進捗バー
- 色分けされた出力：chalk による見やすいターミナル画面
- 安全な終了：'z' を押すと、現在の生成が終わってから安全に終了
- 整理された出力：日付ごとのディレクトリに自動で整理
- メタデータの保存：生成パラメータを含む JSON メタデータファイル（任意）
- 柔軟な設定：豊富な CLI オプションでカスタマイズ可能
- 詳細なログ：タイムスタンプとファイルパス付きの詳細モード

## インストール

```bash
# Clone or download the project
cd stable-diffusion-api-automate

# Install dependencies
npm install
```

### 前提条件

1. 環境変数を設定します:

サンプルの環境ファイルをコピーして編集します:
```bash
cp .env.example .env
```

`.env` ファイルを編集します:
```bash
# Required: Your WebUI API URL
SD_WEBUI_URL=http://192.168.100.105:7860

# Optional: Default paths (can be overridden by CLI options)
# CONFIGS_FILE=prompts/configs.jsonl
# OUTPUT_DIR=out
```

2. API を有効にして Stable Diffusion WebUI を起動します:

```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

`--listen` フラグは外部からの接続を許可し、`--api` はこのツールに必要な API エンドポイントを有効にします。

API の詳しいドキュメントは次を参照してください: https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## クイックスタート

1. 設定ファイル（`prompts/configs.jsonl`）を用意します:
```jsonl
{"prompt": "1girl, solo, green tracksuit, black hair, brown eyes, mischievous smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 2, "hires_fix": false}
{"prompt": "1girl, solo, red dress, blonde hair, blue eyes, gentle smile", "negative_prompt": "worst quality, bad quality", "width": 720, "height": 1280, "sampler_name": "Euler a", "steps": 28, "cfg_scale": 7, "seed": -1, "batch_size": 1, "n_iter": 1, "hires_fix": false}
```

2. ツールを実行します:
```bash
npm start
```

3. 進捗を確認し、途中で止めたいときは 'z' を押して安全に終了します。

## 使い方

### 基本コマンド

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

### CLI オプション

| オプション | 説明 | デフォルト |
|--------|-------------|---------|
| `--configs-file <path>` | 設定 JSONL ファイルのパス | `.env` または `prompts/configs.jsonl` |
| `--output-dir <path>` | 画像の出力先ディレクトリ | `.env` または `out` |
| `--base-url <url>` | WebUI API のベース URL | `.env` または `http://localhost:7860` |
| `--save-meta` | メタデータの JSON ファイルを保存 | `false` |
| `--disable-log-config` | 設定のログ出力を無効化 | `false` |
| `-v, --verbose` | 詳細なログを有効化 | `false` |
| `-h, --help` | ヘルプを表示 | - |

### キーボード操作

- 'z' または 'Z'：安全に終了（現在の生成を終えてから停止）
- Ctrl+C：強制終了（即座に停止）

## 環境設定

このツールは環境変数で設定します。プロジェクトのルートに `.env` ファイルを作成してください:

```bash
# Required: WebUI API URL
SD_WEBUI_URL=http://localhost:7860

# Optional: Default file paths
CONFIGS_FILE=prompts/configs.jsonl
OUTPUT_DIR=out
```

### 設定の優先順位

設定は次の順に適用されます（上ほど優先）:
1. コマンドライン引数（例：`--base-url http://localhost:8080`）
2. 環境変数（`.env` ファイルから）
3. デフォルト値

### よく使う WebUI の URL の例

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

## 設定ファイルの形式

設定ファイルは JSONL（JSON Lines）形式で、各行に Stable Diffusion API の完全なペイロードを1つずつ記述します:

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

### 対応パラメータ

Stable Diffusion WebUI API の標準パラメータはすべて使えます:
- `prompt` - ポジティブプロンプトのテキスト
- `negative_prompt` - ネガティブプロンプトのテキスト
- `width`, `height` - 画像サイズ
- `sampler_name` - サンプリング方法
- `steps` - サンプリングのステップ数
- `cfg_scale` - CFG スケールの値
- `seed` - 乱数シード（-1 でランダム）
- `batch_size` - 1バッチあたりの画像数
- `n_iter` - 繰り返し回数
- `hires_fix` - 高解像度補正を有効化
- ほか多数

## 進捗の確認

ツールは詳細な進捗情報をリアルタイムで表示します:

### 1回の生成の表示
```
🔄 [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 ETA: 12.3s
```

### 複数回の生成の表示
```
🔄 Iter 1/2: [████████████████░░░░░░░░░░░░░░] 57.1% Step 16/28 | Overall: [████████░░░░░░░░░░░░░░░░░░░░] 28.6% ETA: 25.1s
```

### 全体の進捗
```
📸 [████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░] 30.0% Config 3/10
```

## 出力の構成

生成されたファイルは日付ごとに整理されます:

```
out/
└── 2025-08-14/
    ├── 1723648392847-2605429855.png
    ├── 1723648392847-2605429855.meta.json (if --save-meta)
    ├── 1723648394123-2605429856.png
    └── 1723648394123-2605429856.meta.json (if --save-meta)
```

### ファイル名の規則

画像は `[timestamp]-[seed].png` という形式で名前が付きます
- `timestamp`：画像を生成した時刻の Unix タイムスタンプ
- `seed`：生成に実際に使われたシード（API のレスポンスから取得）

### メタデータファイル

`--save-meta` を有効にすると、各画像に次の内容を含む `.meta.json` ファイルが付きます:

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

## サーバーの要件

- `--listen --api` フラグ付きで動作している Stable Diffusion WebUI
- `.env` ファイルで設定された WebUI のエンドポイント（「環境設定」を参照）
- このツールを実行するマシンから WebUI にアクセスできること

API を有効にして WebUI を起動するには:
```bash
# Windows
.\webui.bat --listen --api

# Linux/Mac
./webui.sh --listen --api
```

重要：`--listen` と `--api` の両方のフラグが必要です:
- `--listen`：ほかのマシンからの接続を許可
- `--api`：REST API エンドポイントを有効化

API の総合的なドキュメントや高度な設定については、次を参照してください:  
https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/API

## トラブルシューティング

### 接続できない
- WebUI が `--api` フラグ付きで起動しているか確認します
- サーバーの IP とポートが正しいか確認します（デフォルト：`192.168.100.105:7860`）
- ファイアウォールの設定で接続が許可されているか確認します

### 進捗が更新されない
- ツールは `/sdapi/v1/progress` エンドポイントを自動で監視しています
- 進捗が止まっているように見える場合、サーバーが過負荷になっている可能性があります
- `n_iter` や `batch_size` の値を減らしてみてください

### 生成に失敗する
- WebUI のコンソールでエラーメッセージを確認します
- プロンプトに無効な文字が含まれていないか確認します
- WebUI でモデルが読み込まれているか確認します

### 安全な終了が効かない
- ターミナルが raw モードの入力に対応しているか確認します
- 'z' を何度か押してみてください
- 最終手段として Ctrl+C を使います（現在の生成は保存されない場合があります）

## 使用例

### 基本的な使い方
```bash
# Generate images with default settings
npm start
```

### 高度な使い方
```bash
# Custom configuration with metadata saving
npm start -- \
  --configs-file ./my-prompts.jsonl \
  --output-dir ./renders \
  --save-meta \
  --verbose
```

### 静かなモード
```bash
# Minimal output, no config logging
npm start -- --disable-log-config
```

## コントリビュート

1. リポジトリをフォークします
2. フィーチャーブランチを作成します
3. 変更を加えます
4. しっかりテストします
5. プルリクエストを送ります

## ライセンス

MIT ライセンス。自由に使い、必要に応じて変更してください。

