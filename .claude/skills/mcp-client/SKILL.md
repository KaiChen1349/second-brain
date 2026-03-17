---
name: mcp-client
description: Universal MCP client for connecting to any MCP server with progressive disclosure. Wraps MCP servers as skills to avoid context window bloat from tool definitions. Use when interacting with external MCP servers (Zapier, Sequential Thinking, GitHub, filesystem, etc.), listing available tools, or executing MCP tool calls. Triggers on requests like "connect to Zapier", "use MCP server", "list MCP tools", "call Zapier action", "use sequential thinking", or any MCP server interaction.
---

# 通用 MCP 客戶端

連接任何 MCP 伺服器，採用漸進式載入——按需載入工具結構定義，而非一次性將數千個 token 塞入上下文。

## 技能位置

此技能位於：`.claude/skills/mcp-client/`

**腳本路徑：** `.claude/skills/mcp-client/scripts/mcp_client.py`

## 設定

腳本依以下順序尋找設定檔：
1. `MCP_CONFIG_PATH` 環境變數（自訂路徑）
2. **`references/mcp-config.json`**（此技能的設定檔——建議使用）
3. 專案根目錄的 `.mcp.json`
4. `~/.claude.json`

**你的設定檔：** `.claude/skills/mcp-client/references/mcp-config.json`

編輯此檔案以加入你的 API 金鑰。範例檔案（`example-mcp-config.json`）作為參考範本保留。

**如果使用者尚未提供 Zapier API 金鑰，請向他們索取。**

## 執行指令

所有指令使用位於 `.claude/skills/mcp-client/scripts/mcp_client.py` 的腳本：

```bash
# 列出已設定的伺服器
python .claude/skills/mcp-client/scripts/mcp_client.py servers

# 列出伺服器的工具
python .claude/skills/mcp-client/scripts/mcp_client.py tools <server_name>

# 呼叫工具
python .claude/skills/mcp-client/scripts/mcp_client.py call <server> <tool> '{"arg": "value"}'
```

## 工作流程

1. **檢查設定檔是否存在** - 執行 `servers` 指令。如果出錯，建立 `.mcp.json`
2. **列出伺服器** - 查看已設定的 MCP 伺服器
3. **列出工具** - 從特定伺服器取得工具結構定義
4. **呼叫工具** - 帶參數執行工具

## 指令參考

| 指令 | 說明 |
|---------|-------------|
| `servers` | 列出所有已設定的 MCP 伺服器 |
| `tools <server>` | 列出工具及完整參數結構定義 |
| `call <server> <tool> '<json>'` | 帶參數執行工具 |

## 範例：Zapier

```bash
# 1. 列出伺服器以確認 Zapier 已設定
python .claude/skills/mcp-client/scripts/mcp_client.py servers

# 2. 列出 Zapier 工具
python .claude/skills/mcp-client/scripts/mcp_client.py tools zapier

# 3. 呼叫 Zapier 工具
python .claude/skills/mcp-client/scripts/mcp_client.py call zapier <tool_name> '{"param": "value"}'
```

## 範例：Sequential Thinking

```bash
# 1. 列出工具
python .claude/skills/mcp-client/scripts/mcp_client.py tools sequential-thinking

# 2. 使用 sequential thinking
python .claude/skills/mcp-client/scripts/mcp_client.py call sequential-thinking sequentialthinking '{"thought": "Breaking down the problem...", "thoughtNumber": 1, "totalThoughts": 5, "nextThoughtNeeded": true}'
```

## 設定檔格式

設定檔格式（`references/mcp-config.json`）：

```json
{
  "mcpServers": {
    "zapier": {
      "url": "https://mcp.zapier.com/api/v1/connect",
      "api_key": "your-api-key"
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

**傳輸偵測：**
- `url` + `api_key` → 帶 Bearer 驗證的 FastMCP（Zapier）
- `command` + `args` → stdio（本地伺服器如 sequential-thinking）
- `url` 以 `/sse` 結尾 → SSE 傳輸
- `url` 以 `/mcp` 結尾 → 可串流 HTTP

## 錯誤處理

錯誤以 JSON 格式回傳：
```json
{"error": "message", "type": "configuration|validation|connection"}
```

- `configuration` - 找不到設定檔。請建立 `.mcp.json`
- `validation` - 無效的伺服器或工具名稱
- `connection` - 無法連接到伺服器

## 依賴套件

```bash
pip install mcp fastmcp
```

## 參考資料

- `references/example-mcp-config.json` - 設定檔範本
- `references/mcp-servers.md` - 常見伺服器設定
- `references/python-mcp-sdk.md` - Python SDK 文件
