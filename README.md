# 🐇 BunnyEra-Console

BunnyEra Console is the core desktop application of the BunnyEra brand matrix, built with Electron.
BunnyEra Console 是 BunnyEra 品牌矩阵的核心桌面应用，基于 Electron 构建。

It integrates logging, resources, monitoring and signal modules to provide a unified platform for enterprise automation.
它整合日志、资源、监控、信号等模块，为企业提供统一的操作平台与自动化工作流。

---

## 🚀 Features / 功能模块
- Log Module / 日志模块：本地写入与读取，支持系统化记录。
- Agent Module / 资源模块：接入虚拟卡系统（AgentCardOS）骨架。
- Monitor Module / 监控模块：系统状态与任务进度监控骨架。
- Signal Module / 信号模块：接入 BunnyEraEchoBot（验证码/翻译）骨架。
- Matrix Account / 矩阵账号模块与网络推送（webhook / API）。

---

## 📂 Repository Structure / 目录结构
```
BunnyEra-Console/
 ├── src/                # Electron main & renderer sources
 ├── modules/            # Module implementations and skeletons
 │    ├── AgentModule/
 │    ├── LogModule/
 │    └── MonitorModule/
 ├── docs/
 └── README.md
```

---

## 📦 Requirements / 环境要求
- Node.js >= 18
- npm (or yarn)
- Windows, macOS, or Linux

---

## ⚙️ Install / 安装
```bash
# clone repo
git clone https://github.com/BunnyEraOfficial/BunnyEra-Console.git
cd BunnyEra-Console
# install deps
npm install
```

---

## 🚀 Development / 开发与运行
This project uses Vite (renderer) + Electron (main). The dev script runs both renderer (with HMR) and the Electron main process with live reload.
项目使用 Vite (renderer) + Electron (主进程)。dev 脚本会并行启动 renderer（支持 HMR）和 Electron 主进程（支持重载）。

```bash
# start development (renderer + electron)
npm run dev
```

After the dev server starts, Electron will open the application window.
在开发服务器启动后，Electron 会打开应用窗口。

---

## 📦 Packaging / 打包
We use electron-builder to create distributable binaries. The repository provides scripts for each platform:
我们使用 electron-builder 打包应用，仓库提供了针对每个平台的脚本：

```bash
# build for Windows (NSIS)
npm run build:win
# build for macOS (unsigned dmg)
npm run build:mac
# build for Linux (AppImage)
npm run build:linux
```

Notes: macOS builds will be unsigned by default (you can provide signing certificates later if needed).
注意：macOS 默认生成 unsigned dmg（需签名以通过 Gatekeeper）。

---

## 🔧 Quick usage example — LogModule (MVP demo)
A minimal demo is included in the MVP to exercise the LogModule: write a log entry from the renderer and read it back. The LogModule runs in the Electron main process and exposes APIs to the renderer via the secure preload/contextBridge.

Example renderer code (TypeScript) — write and read logs:

```ts
// src/renderer/example/log-demo.tsx
import React, { useState } from 'react';

export default function LogDemo() {
  const [content, setContent] = useState('');
  const [logText, setLogText] = useState('');

  const writeLog = async () => {
    // window.api.logger.appendLog(filePath, text) is exposed by preload.ts
    await window.api.logger.appendLog('app.log', `[${new Date().toISOString()}] ${content}
`);
    setContent('');
  };

  const readLog = async () => {
    const text = await window.api.logger.readLog('app.log');
    setLogText(text || '');
  };

  return (
    <div>
      <h3>Log Demo</h3>
      <input value={content} onChange={e => setContent(e.target.value)} placeholder="Enter log message" />
      <button onClick={writeLog}>Write Log</button>
      <button onClick={readLog}>Read Log</button>
      <pre style={{whiteSpace: 'pre-wrap'}}>{logText}</pre>
    </div>
  );
}
```

Notes: the actual API surface in the MVP is: window.api.logger.appendLog(path: string, text: string) and window.api.logger.readLog(path: string) => Promise<string>. Modify or extend as needed.

---

## 🛠️ What this MVP includes / 本次 MVP 包含
- Electron + Vite + React + TypeScript + Ant Design scaffold
- LogModule basic implementation (append/read file via main process, exposed via IPC)
- Scripts: dev, build:win, build:mac, build:linux
- Module skeletons for AgentModule, MonitorModule, SignalModule

---

## 🤝 Contributing / 贡献指南
- Fork the repository and create a branch
- Commit code and open a Pull Request
- Use Issues to report bugs or request features (use templates where provided)

---

If you want, I can also: set up ESLint/Prettier, add CI (GitHub Actions) to build releases, or implement additional modules (AgentCardOS integration, BunnyEraEchoBot).
如果你愿意，我还可以：添加 ESLint/Prettier、配置 CI（GitHub Actions）以自动构建发布，或实现更多模块（AgentCardOS 集成、BunnyEraEchoBot）。

---

License: Add a LICENSE file if you want to declare a license.
