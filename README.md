# 本地 AI 聊天室

基于 WebSocket 的多人聊天室，接入本地部署的 Dify 智能体，实现 AI 自动回复。

## 功能

- 多房间 WebSocket 实时聊天
- 消息持久化（SQLite）
- 接入 Dify 智能体，用户发言后 AI 自动回复
- 支持断线重连、未读消息标记

## 技术栈

- 前端：原生 HTML/CSS/JavaScript + WebSocket
- 后端：Node.js + ws + better-sqlite3
- AI：Dify（本地 Docker 部署）+ Ollama（qwen2.5:7b）

## 架构

```
浏览器 ←WebSocket→ Node.js 服务器 ←HTTP→ Dify API ←HTTP→ Ollama
                        ↓
                    SQLite (chat.db)
```

## 快速开始

### 前置条件

- Node.js ≥ 18
- Dify 已本地部署并发布应用
- Ollama 已运行，且有可用模型（如 qwen2.5:7b）

### 安装依赖

```bash
npm install
```

### 配置

在项目根目录创建 .env：

```
DIFY_API_KEY=app-你的KEY
DIFY_API_URL=http://localhost/v1/chat-messages
```

### 启动

```bash
node server.js
```

浏览器打开 index.html，输入昵称和房间名，加入后即可聊天。AI 会在用户发言后自动回复。

## 文件说明

| 文件 | 作用 |
|---|---|
| server.js | WebSocket 服务器 + Dify API 调用 |
| index.html | 前端页面 |
| style.css | 样式 |
| start_ai.bat | 一键启动脚本（Windows） |
| .env | 环境变量（不提交到 Git） |
| chat.db | SQLite 数据库（自动生成） |

## 已知问题

- WSL2 网关 IP 在重启后可能变化，需同步更新 Dify 中 Ollama 的基础 URL
- qwen3.5:9b 等推理模型会在回复中带 think 思考过程，已换用 qwen2.5:7b
