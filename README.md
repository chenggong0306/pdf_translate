# PDF 翻译工具

基于 [PDFMathTranslate](https://github.com/Byaidu/PDFMathTranslate) 二次开发，支持学术 PDF 文档（含数学公式）的高质量翻译，提供 Web GUI 界面，去除水印，使用 uvicorn 驱动后端服务。

## 主要改动

- ✅ Flask 后端改为 **uvicorn** 启动（ASGI 异步服务器，性能更好）
- ✅ 去除翻译后 PDF 首页的 **BabelDOC 水印**

## 环境要求

- Python `>=3.11, <3.13`
- [uv](https://github.com/astral-sh/uv) 包管理器

## 部署步骤

### 1. 安装 uv

```bash
curl -Ls https://astral.sh/uv/install.sh | sh
```

Windows:
```powershell
powershell -ExecutionPolicy Bypass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### 2. 克隆代码

```bash
git clone https://github.com/chenggong0306/pdf_translate.git
cd pdf_translate
```

### 3. 安装依赖

```bash
uv sync
```

### 4. 预热模型资源

首次运行需要下载模型（约几百 MB）：

```bash
uv run babeldoc --warmup
```

### 5. 启动 GUI

```bash
uv run pdf2zh -i
```

浏览器访问：`http://localhost:7860`

---

## 启动方式说明

| 命令 | 说明 |
|------|------|
| `uv run pdf2zh -i` | 启动 Web GUI（Gradio，端口 7860） |
| `uv run pdf2zh --flask` | 启动 REST API 后端（uvicorn，端口 11008） |
| `uv run pdf2zh --mcp --sse` | 启动 MCP SSE 服务 |

---

## 服务器后台运行（Linux）

使用 `nohup` 后台保持运行：

```bash
nohup uv run pdf2zh -i > pdf2zh.log 2>&1 &
```

或使用 `systemd` 管理服务，创建 `/etc/systemd/system/pdf2zh.service`：

```ini
[Unit]
Description=PDF Translate GUI
After=network.target

[Service]
WorkingDirectory=/path/to/pdf_translate
ExecStart=/root/.local/bin/uv run pdf2zh -i --server-name 0.0.0.0
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

然后启用服务：

```bash
systemctl daemon-reload
systemctl enable pdf2zh
systemctl start pdf2zh
systemctl status pdf2zh
```

---

## 支持的翻译服务

- Google 翻译
- DeepL
- OpenAI（GPT 系列）
- Azure OpenAI
- 腾讯翻译
- Ollama（本地模型）
- SiliconFlow
- 以及更多...

---

## 许可证

AGPL-3.0
