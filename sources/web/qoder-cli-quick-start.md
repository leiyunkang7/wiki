# Qoder CLI SDK Quick Start

**URL:** https://docs.qoder.com/zh/cli/sdk/quick-start
**收藏时间:** 2026-05-30
**状态:** ✅ 已获取内容

## 概述

Qoder Agent SDK 让你用 TypeScript 调用 Qoder AI 的能力——读写文件、搜索代码、执行命令等，只需几行代码就能把 AI Agent 嵌入到应用或脚本里。

## 前置条件

- Node.js 18+

## 安装

```bash
npm install @qoder-ai/qoder-agent-sdk
```

## 认证

SDK 通过 Personal Access Token (PAT) 认证身份。

1. 到 [qoder.com/account/integrations](https://qoder.com/account/integrations) 生成 PAT
2. 设置环境变量：`export QODER_PERSONAL_ACCESS_TOKEN="<your-pat>"`

```js
import { accessTokenFromEnv, query } from '@qoder-ai/qoder-agent-sdk';

const stream = query({
  prompt: 'Hello',
  options: {
    auth: accessTokenFromEnv(),
  },
});
```

## 示例

```js
import { accessTokenFromEnv, query } from '@qoder-ai/qoder-agent-sdk';

for await (const message of query({
  prompt: 'Analyze the codebase, find functions without test coverage, and write unit tests for them.',
  options: {
    auth: accessTokenFromEnv(),
    allowedTools: ['Read', 'Write', 'Edit', 'Glob', 'Grep', 'Bash'],
    permissionMode: 'acceptEdits',
  },
})) {
  if (message.type === 'assistant') {
    for (const block of message.message.content) {
      if (block.type === 'text') {
        console.log(block.text);
      } else if (block.type === 'tool_use') {
        console.log(`Tool: ${block.name}`);
      }
    }
  } else if (message.type === 'result') {
    console.log(`Done: ${message.subtype}`);
  }
}
```

## 下一步

- [SDK 认证](/zh/cli/sdk/authentication) — PAT、环境变量和认证错误处理
- [多轮对话](/zh/cli/sdk/multi-turn-conversation) — 多消息会话
- [流式输出](/zh/cli/sdk/streaming-output) — 实时接收增量内容
- [SDK References](/zh/cli/sdk/references) — 完整 API 参考
