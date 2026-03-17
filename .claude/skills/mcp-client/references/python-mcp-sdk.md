# Python MCP SDK 參考

用於建置和使用 MCP 伺服器的官方 Python SDK。

## 安裝

```bash
pip install mcp
```

**目前穩定版本：** 1.25.x（建議固定為 `mcp>=1.25,<2` 以確保穩定性）

**注意：** v2 版本預計於 2026 年第一季發布。v1.x 將持續收到錯誤修復。

## 核心概念

MCP（Model Context Protocol）提供標準化的方式，將 AI 模型連接到外部資料來源和工具。

**把它想成 AI 的 USB-C**——一個跨不同工具和服務通用的連接器。

## 客戶端用法（使用 MCP 伺服器）

### 建立工作階段

```python
from mcp import ClientSession

# 用於 stdio 傳輸
from mcp import StdioServerParameters
from mcp.client.stdio import stdio_client

server_params = StdioServerParameters(
    command="npx",
    args=["-y", "@modelcontextprotocol/server-github"],
    env={"GITHUB_TOKEN": "xxx"}
)

async with stdio_client(server_params) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
        # 使用 session...
```

### SSE 傳輸（遠端伺服器）

```python
from mcp import ClientSession
from mcp.client.sse import sse_client

async with sse_client(url, headers=headers, timeout=30) as (read, write):
    async with ClientSession(read, write) as session:
        await session.initialize()
        # 使用 session...
```

### 可串流 HTTP 傳輸

```python
from mcp import ClientSession
from mcp.client.streamable_http import streamablehttp_client

async with streamablehttp_client(url, headers=headers) as (read, write, _):
    async with ClientSession(read, write) as session:
        await session.initialize()
        # 使用 session...
```

### 列出工具

```python
result = await session.list_tools()

for tool in result.tools:
    print(f"工具：{tool.name}")
    print(f"說明：{tool.description}")
    print(f"結構定義：{tool.inputSchema}")
```

### 呼叫工具

```python
result = await session.call_tool("tool_name", {"arg1": "value1"})

# 擷取內容
for item in result.content:
    if hasattr(item, 'text'):
        print(item.text)
    elif hasattr(item, 'data'):
        print(item.data)
```

### 列出資源

```python
result = await session.list_resources()

for resource in result.resources:
    print(f"資源：{resource.uri}")
    print(f"名稱：{resource.name}")
```

### 讀取資源

```python
result = await session.read_resource("resource://uri")

for content in result.contents:
    print(content.text)
```

---

## 建立伺服器（建置 MCP 伺服器）

### FastMCP（建議使用）

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("My Server")

@mcp.tool()
def add(a: int, b: int) -> int:
    """將兩個數字相加。"""
    return a + b

@mcp.resource("greeting://{name}")
def get_greeting(name: str) -> str:
    """取得個人化的問候語。"""
    return f"Hello, {name}!"

if __name__ == "__main__":
    mcp.run()
```

### 工具裝飾器

```python
@mcp.tool()
def search_database(query: str, limit: int = 10) -> list[dict]:
    """
    搜尋資料庫中的匹配記錄。

    Args:
        query: 搜尋查詢字串
        limit: 回傳的最大結果數

    Returns:
        匹配記錄的列表
    """
    # 實作
    return results
```

**型別提示**會自動轉換為 JSON Schema。

### 資源裝飾器

```python
@mcp.resource("config://settings")
def get_settings() -> str:
    """以 JSON 格式回傳應用程式設定。"""
    return json.dumps(settings)

@mcp.resource("file://{path}")
def read_file(path: str) -> str:
    """從檔案系統讀取檔案。"""
    return Path(path).read_text()
```

### 提示詞裝飾器

```python
@mcp.prompt()
def code_review(code: str, language: str = "python") -> str:
    """產生程式碼審查提示詞。"""
    return f"Please review this {language} code:\n\n```{language}\n{code}\n```"
```

---

## 傳輸設定

### stdio（本地伺服器）

本地子程序伺服器的預設傳輸。

```python
from mcp import StdioServerParameters

params = StdioServerParameters(
    command="python",
    args=["server.py"],
    env={"DEBUG": "true"},
    cwd="/path/to/dir"
)
```

### SSE（遠端——舊版）

用於遠端 HTTP 連接的伺服器發送事件。

```python
from mcp.client.sse import sse_client

async with sse_client(
    url="https://example.com/mcp/sse",
    headers={"Authorization": "Bearer token"},
    timeout=60
) as (read, write):
    # ...
```

### 可串流 HTTP（遠端——現代）

具有更佳連接處理的現代 HTTP 傳輸。

```python
from mcp.client.streamable_http import streamablehttp_client

async with streamablehttp_client(
    url="https://example.com/mcp",
    headers={"Authorization": "Bearer token"}
) as (read, write, session_info):
    # ...
```

---

## 錯誤處理

```python
from mcp.types import McpError

try:
    result = await session.call_tool("tool", args)
except McpError as e:
    print(f"MCP 錯誤：{e.code} - {e.message}")
except ConnectionError as e:
    print(f"連接失敗：{e}")
except TimeoutError as e:
    print(f"請求逾時：{e}")
```

---

## 驗證

MCP SDK 支援 OAuth 2.1 用於資源伺服器功能。

```python
from mcp.server.auth import bearer_auth

@mcp.tool()
@bearer_auth(scopes=["read:data"])
def protected_tool(data: str) -> str:
    """需要驗證的工具。"""
    return process(data)
```

---

## 最佳實踐

1. **使用上下文管理器** - 始終使用 `async with` 來管理工作階段
2. **處理錯誤** - 用 try/except 包裝呼叫以實現優雅降級
3. **設定逾時** - 防止在無回應的伺服器上掛起
4. **為工具加上型別** - 讓 SDK 從型別提示產生 JSON Schema
5. **為工具撰寫文件** - Docstring 會成為工具說明

---

## 資源

- [Python SDK GitHub](https://github.com/modelcontextprotocol/python-sdk)
- [SDK 文件](https://modelcontextprotocol.github.io/python-sdk/)
- [PyPI 套件](https://pypi.org/project/mcp/)
- [MCP 規範](https://modelcontextprotocol.io/specification/)
