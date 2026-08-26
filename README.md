# Cloudflare Worker Edge TTS · Reeden 多音色配置包

基于 Cloudflare Worker 自建 **微软 Edge TTS**（Azure Speech REST）代理，为 Reeden 阅读器提供**免费、不限次**的多角色 TTS 朗读能力。

## ✨ 特性

- **免费不限次**：走 Azure Speech 免费 REST 接口，无调用次数限制
- **HD 高清音色**：支持 DragonHD Flash 系列，自然度远超标准 Neural
- **短句自动情绪**：短语气词（嗯啊/唉/哼）自动注入副语言标记（喘息/叹气/轻笑/低语），情绪饱满
- **空音频/500 自动重试**：HD 音色偶发失败自动重试 + 指数退避抖动
- **长文本分块**：智能按句子边界分块，超长文本稳定合成
- **93 个网文角色**：旁白/男主/女主/反派/低语/情欲/情绪/特殊身份，全部带人设介绍
- **73 个标准音色**：44 标准（含语调优化）+ 29 HD/MAI，可选丰富

## 📁 文件说明

| 文件 | 说明 |
|---|---|
| `edge-tts-worker.js` | Cloudflare Worker 代码（中文注释） |
| `reeden-tts-cf-config.json` | 93 个网文角色配置（Reeden 导入用） |
| `reeden-tts-standard-cf.json` | 73 个音色配置（Reeden 导入用） |

## 🚀 部署 Worker

1. 将 `edge-tts-worker.js` 部署到 Cloudflare Workers
2. 设置环境变量 `API_KEY`（你的访问密钥，用于 Bearer 鉴权）
3. 绑定自定义域名或使用 Workers 默认域名，得到 `https://your-worker.example.com/v1/audio/speech`

> ⚠️ **签名密钥需替换**：代码中 `YOUR_AZURE_SIGNING_KEY` 需替换为你自己的 Azure 翻译签名密钥（获取方式见下方说明）。

## 📖 Reeden 配置

1. Reeden → 语音设置 → 自定义 HTTP 合成 → **从文件导入**
2. 多角色朗读导入 `reeden-tts-cf-config.json`
3. 单独选音色导入 `reeden-tts-standard-cf.json`
4. 将配置文件中的 `YOUR_API_KEY` 替换为你 Worker 的 `API_KEY`

## 🔒 安全说明

本仓库已脱敏：所有 API Key、签名密钥均替换为占位符（`YOUR_API_KEY` / `YOUR_AZURE_SIGNING_KEY`）。使用时需替换为自己的密钥。

## 🛠 技术细节

- 走 `dev.microsofttranslator.com/apps/endpoint` 获取 token → `eastasia.tts.speech.microsoft.com` REST 合成
- 支持副语言标记：`[whispering]` `[panting]` `[sighing]` `[laughing]` `[excited]` 等
- 短文本（≤20 字符）自动插入 break 停顿 + 情绪标记
- 智能分块 1000 字符/块，避免超 Azure 限制

## ⚠️ 免责声明

仅供个人学习使用。请遵守微软 Azure 服务条款，勿用于商业用途。
