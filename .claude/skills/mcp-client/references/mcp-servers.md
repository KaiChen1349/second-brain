# 常見 MCP 伺服器設定

常用 MCP 伺服器的參考設定。將相關區段複製到你的 `.mcp.json`。

## 遠端伺服器（FastMCP/SSE/HTTP）

### Zapier MCP

連接超過 8,000 個應用程式和 30,000 個以上的動作。從 [mcp.zapier.com](https://mcp.zapier.com/) 取得你的 API 金鑰。

**目前格式（帶 Bearer 驗證的 FastMCP）：**
```json
{
  "zapier": {
    "url": "https://mcp.zapier.com/api/v1/connect",
    "api_key": "YOUR_MCP_API_KEY"
  }
}
```

**取得 API 金鑰：**
1. 前往 [mcp.zapier.com](https://mcp.zapier.com/)
2. 用你的 Zapier 帳號登入
3. 設定要暴露的動作/應用程式
4. 複製產生的 MCP API 金鑰

**運作原理：** 腳本使用 FastMCP 客戶端搭配 StreamableHttpTransport 和 Bearer 權杖驗證。當設定中有 `api_key` 時，會自動使用此傳輸方式。

**安全性：** 將你的 API 金鑰視為密碼——它授予了對你已設定的 Zapier 動作的存取權限。

**Token 成本：** 一次 MCP 工具呼叫 = 你方案配額中的 2 個 Zapier 任務。

**舊版 SSE 格式（如有需要）：**
```json
{
  "zapier": {
    "url": "https://actions.zapier.com/mcp/YOUR_MCP_SERVER_KEY/sse"
  }
}
```

---

## 本地伺服器（stdio）

### Sequential Thinking

透過動態反思式思考流程進行結構化問題解決。適用於將複雜問題分解為步驟。

```json
{
  "sequential-thinking": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
  }
}
```

**Docker 替代方案：**
```json
{
  "sequential-thinking": {
    "command": "docker",
    "args": ["run", "--rm", "-i", "mcp/sequentialthinking"]
  }
}
```

**環境變數：** 設定 `DISABLE_THOUGHT_LOGGING=true` 以停用詳細輸出。

**工具：** `sequentialthinking`
- `thought`（字串）：當前思考步驟
- `thoughtNumber`（整數）：當前步驟編號
- `totalThoughts`（整數）：預期總步驟數
- `nextThoughtNeeded`（布林值）：是否還需要更多步驟

---

### GitHub MCP

存取 GitHub 倉庫、議題、PR 等。

```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxxxxxxxxxx"
    }
  }
}
```

**取得權杖：**
1. 前往 GitHub 設定 > 開發者設定 > 個人存取權杖
2. 產生具有適當範圍的權杖（repo、read:org 等）

---

### Filesystem MCP

對本地檔案系統路徑的讀寫存取。

```json
{
  "filesystem": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem", "/allowed/path"]
  }
}
```

**安全性：** 只有指定路徑（及其子目錄）可被存取。

---

### Memory MCP

對話的持久化鍵值記憶體。

```json
{
  "memory": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-memory"]
  }
}
```

---

### PostgreSQL MCP

查詢 PostgreSQL 資料庫。

```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres"],
    "env": {
      "POSTGRES_CONNECTION_STRING": "postgresql://user:pass@host:5432/db"
    }
  }
}
```

---

### Brave Search MCP

透過 Brave Search API 進行網路搜尋。

```json
{
  "brave-search": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-brave-search"],
    "env": {
      "BRAVE_API_KEY": "your-api-key"
    }
  }
}
```

---

### Puppeteer MCP

用於網頁爬取和測試的瀏覽器自動化。

```json
{
  "puppeteer": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
  }
}
```

---

## 自訂 Python 伺服器

用 Python 撰寫的自訂 MCP 伺服器：

```json
{
  "my-server": {
    "command": "python",
    "args": ["path/to/my_mcp_server.py"],
    "env": {
      "MY_API_KEY": "xxx",
      "DEBUG": "true"
    },
    "cwd": "/path/to/working/directory"
  }
}
```

**使用 uv（建議）：**
```json
{
  "my-server": {
    "command": "uv",
    "args": ["run", "my_mcp_server.py"],
    "cwd": "/path/to/project"
  }
}
```

---

## 完整範例設定

```json
{
  "mcpServers": {
    "zapier": {
      "url": "https://actions.zapier.com/mcp/YOUR_KEY/sse"
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxx"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./data"]
    }
  }
}
```

---

## 傳輸參考

| 伺服器類型 | 傳輸方式 | 設定鍵 | 範例 |
|-------------|-----------|------------|---------|
| 遠端（SSE） | SSE | `url` | `https://...../sse` |
| 遠端（HTTP） | 可串流 HTTP | `url` | `https://...../mcp` |
| 本地（Node.js） | stdio | `command` | `npx` |
| 本地（Python） | stdio | `command` | `python` |
| 本地（Docker） | stdio | `command` | `docker` |

---

## 資源

- [MCP 規範](https://modelcontextprotocol.io/specification/)
- [官方 MCP 伺服器](https://github.com/modelcontextprotocol/servers)
- [Zapier MCP 指南](https://zapier.com/blog/zapier-mcp-guide/)
- [Python MCP SDK](https://github.com/modelcontextprotocol/python-sdk)
