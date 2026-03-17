# Claude Code 第二大腦技能集

一套自訂技能集合，將 Claude Code 打造成知識工作的第二大腦。這些技能展示了上下文的漸進式載入——沒有魔法，只是結構化的知識，讓 Claude 在特定任務中發揮超強能力。

## 這是什麼

大多數人把 Claude Code 當作寫程式和除錯的工具。這些技能將它擴展為一個擷取和落實知識的系統：

- **外部整合** - 連接 MCP 伺服器（Zapier、GitHub 等），不會造成上下文膨脹
- **影片製作** - 使用 Remotion 和 React 程式化地製作影片
- **簡報製作** - 具備品牌風格的專業投影片和 LinkedIn 輪播圖
- **文件撰寫** - 人們真的會照著做的 Runbook、SOP 和技術文件
- **技能開發** - 建立新技能以進一步擴展 Claude 的能力
- **品牌一致性** - 一次定義你的語調和視覺識別，到處使用

此系統透過漸進式載入運作：Claude 只在需要時才載入詳細指令，保持上下文的高效率，同時在每個領域維持深度專業知識。

---

## 品牌與語調產生器

產生品牌語調和品牌系統檔案，驅動 PPTX 產生器，也可以引導所有其他技能的客製化。這個技能會引導你建立品牌識別、定義寫作語調，以及建立視覺系統。

**核心理念**：你的品牌和語調應該只記錄一次，然後到處重複使用。這個技能建立的檔案將成為所有內容產生的唯一真實來源。

<details>
<summary><big>📦 產生的檔案</big></summary>

| 檔案 | 用途 | 使用者 |
|------|---------|---------|
| `brand.json` | 顏色、字型、素材 | PPTX 產生器 |
| `config.json` | 輸出設定 | PPTX 產生器 |
| `brand-system.md` | 設計哲學與準則 | 所有技能 |
| `tone-of-voice.md` | 寫作語調與個性 | PPTX 內容、SOP |

</details>

<details>
<summary><big>🔄 流程概覽</big></summary>

1. **收集品牌基本資訊** - 名稱、描述、主要用途
2. **定義顏色** - 完整系統的 10 個色值
3. **定義字型** - 標題、內文和程式碼字型
4. **定義素材** - Logo 和圖示路徑
5. **探索語調** - 個性、詞彙、句型模式
6. **建立設計哲學** - 核心原則和標誌性元素
7. **產生檔案** - 用收集到的資訊建立所有四個檔案

</details>

<details>
<summary><big>🎭 內建的語調範本</big></summary>

技能內含 5 個語調範例設定，幫助你找到自己的風格：

- **技術教育者** - 充滿熱情的專家，透過展示來教學
- **沉穩權威** - 自信且從容，讓專業知識透過具體細節說話
- **建造者視角** - 開發者對開發者，以程式碼支撐的直率觀點
- **平易近人的專家** - 讓複雜的事物變得易懂，但不會過度簡化
- **逆向思考者** - 用證據挑戰傳統智慧

</details>

<br/>

**觸發方式**：「幫我建立品牌系統」、「產生我的語調」、「為簡報設定品牌」、「建立品牌檔案」

---

## 技能概覽

### MCP 客戶端

將 Claude Code 連接到外部 MCP 伺服器（Zapier、GitHub、Sequential Thinking 等），採用漸進式載入——工具結構定義按需載入，而非塞滿你的上下文視窗。

**核心理念**：MCP 伺服器會暴露數千個 token 的工具定義。這個技能將它們包裝成輕量級客戶端，只在需要時才載入所需內容。

<details>
<summary><big>🔧 設定：建立你的設定檔</big></summary>

**步驟 1：** 複製範例設定檔來建立自己的設定：

```bash
cp .claude/skills/mcp-client/references/example-mcp-config.json \
   .claude/skills/mcp-client/references/mcp-config.json
```

**步驟 2：** 編輯 `mcp-config.json`，填入你的 API 金鑰和伺服器資訊。

設定格式與 Claude Desktop 的 MCP 設定完全相同：

```json
{
  "mcpServers": {
    "zapier": {
      "url": "https://mcp.zapier.com/api/v1/connect",
      "api_key": "YOUR_API_KEY_HERE"
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

**傳輸類型：**
- `url` + `api_key` → 帶 Bearer 驗證的遠端伺服器（Zapier）
- `command` + `args` → 本地 stdio 伺服器（npx、python、docker）
- `url` 以 `/sse` 結尾 → SSE 傳輸
- `url` 以 `/mcp` 結尾 → 可串流 HTTP

</details>

<details>
<summary><big>📋 可用指令（代理程式會自動處理）</big></summary>

```bash
# 列出已設定的伺服器
python .claude/skills/mcp-client/scripts/mcp_client.py servers

# 列出伺服器的工具（包含完整結構定義）
python .claude/skills/mcp-client/scripts/mcp_client.py tools zapier

# 呼叫工具
python .claude/skills/mcp-client/scripts/mcp_client.py call zapier <tool_name> '{"param": "value"}'
```

</details>

<details>
<summary><big>📝 在 CLAUDE.md 中記錄工具注意事項</big></summary>

**重要：** 設定好 MCP 伺服器後，請讓 Claude 測試每個工具並記錄任何特殊行為。這會節省未來呼叫的時間。

在你專案的 `CLAUDE.md` 中加入一個區段（或建立一個）——範例：

```markdown
## MCP 工具筆記

### Zapier
- `send_gmail_email`：`to` 欄位必須是單一電子郵件，不是陣列
- `create_notion_page`：需要 `database_id`，不是 `page_id`
- 速率限制：每次 MCP 呼叫最多 2 個 Zapier 任務

### Sequential Thinking
- 務必設定 `nextThoughtNeeded: true` 直到最後一個思考
- `totalThoughts` 是建議值，可在過程中調整
```

**為什麼這很重要：** MCP 工具經常有未記錄的參數要求或行為。測試一次並記錄下來，可以節省上下文空間並避免重複錯誤。

**工作流程：**
1. 連接新的 MCP 伺服器
2. 請 Claude：「列出 [伺服器] 的所有工具並用範例輸入測試每一個」
3. 在 CLAUDE.md 中記錄任何失敗、所需格式或注意事項
4. Claude 在未來的呼叫中會參考這些筆記

</details>

<br/>

**觸發範例**：「連接 Zapier」、「使用 MCP 伺服器」、「列出 MCP 工具」、「呼叫 Zapier 動作」，或任何 MCP 伺服器互動

---

### PPTX 產生器

使用 python-pptx 產生專業的品牌風格簡報投影片和 LinkedIn 輪播圖。

> **致謝**：此技能最初由 Rasmus 建立，[維護於此](https://github.com/Wirasm/presentation-slides-skill)。這裡的版本已針對品牌特定設定進行調整。

<details>
<summary><big>🎛️ 三種操作模式</big></summary>

1. **投影片產生** - 建立 16:9 品牌風格簡報
2. **輪播圖產生** - 建立方形 1:1 LinkedIn 輪播圖（匯出為 PDF）
3. **版面管理** - 建立、編輯和改進食譜版面

</details>

<details>
<summary><big>✨ 主要功能</big></summary>

- 食譜中有 16 個投影片版面範本（標題、內容、數據、雙欄、多卡片、浮動卡片、圓形主視覺、引言、圖表、程式碼等）
- 5 個輪播圖專用版面（鉤子、單一重點、編號重點、引言、行動呼籲）
- 含顏色、字型和素材的品牌系統
- 批次產生（每次最多 5 張投影片）確保可靠性
- 多樣性規則防止重複的版面

</details>

<details>
<summary><big>💡 關鍵概念</big></summary>

**視覺優先的版面選擇** - content-slide 是最後手段，不是預設選項。

技能包含一個決策樹，將條列式內容轉換為視覺版面：
- 3-5 個同等項目 → multi-card-slide
- 2-4 個大數字 → stats-slide
- 比較兩個事物 → two-column-slide
- 中心概念搭配周圍項目 → circular-hero-slide
- 有力的引言 → quote-slide

只有在沒有任何視覺版面適合時才使用 content-slide。

</details>

<br/>

**觸發方式**：包含品牌名稱的投影片、簡報、輪播圖、PPTX 或版面請求

---

### SOP 建立器

建立人們真的會照著做的 Runbook、Playbook 和技術文件。

**核心理念**：沒人會讀 50 頁的文件。讓它可掃描、可執行，而且不可能被誤解。

<details>
<summary><big>📂 文件類型</big></summary>

- **技術/工程**：Runbook、部署 Playbook、故障排除指南、操作指南、ADR
- **營運/業務**：流程 SOP、檢查清單、決策樹、交接文件
- **內容/創意**：製作工作流程、審查流程、發佈檢查清單
- **通用**：標準 SOP、快速參考、入職指南

</details>

<details>
<summary><big>🏗️ 通用結構</big></summary>

1. 完成定義（檢查清單——最重要，放在最前面）
2. 何時使用
3. 前提條件
4. 流程（編號步驟）
5. 驗證完成
6. 出問題時怎麼辦
7. 有問題？

</details>

<details>
<summary><big>📏 撰寫規則</big></summary>

- 具體明確（數字、名稱、閾值——不要用「視需要」或「定期」）
- 步驟以動作開頭（動詞，不是描述）
- 警告放在前面（在危險步驟之前，不是之後）
- 清楚的決策點（如果 X，則 Y——不要「依優先順序處理」）

</details>

<br/>

**觸發方式**：記錄流程、建立 Runbook、撰寫營運文件、規範化技術程序的請求

---

### 技能建立器

建立有效技能的指南，擴展 Claude 的能力。

**核心理念**：技能是模組化、自成一體的套件，將 Claude 從通用型代理程式轉變為專門型代理程式。只加入 Claude 還不知道的上下文。

<details>
<summary><big>🎁 技能提供什麼</big></summary>

1. 專門化工作流程 - 特定領域的多步驟程序
2. 工具整合 - 與特定檔案格式或 API 協作的指令
3. 領域專業知識 - 公司特有的知識、結構定義、業務邏輯
4. 打包資源 - 複雜任務所需的腳本、參考資料和素材

</details>

<details>
<summary><big>🧭 核心原則</big></summary>

- **簡潔是關鍵** - 只加入 Claude 還不知道的上下文
- **設定適當的自由度** - 根據任務的脆弱性匹配具體程度
- **漸進式載入** - 中繼資料始終在上下文中，主體在觸發時載入，資源按需載入

</details>

<details>
<summary><big>🗂️ 技能結構</big></summary>

```
skill-name/
├── SKILL.md（必要）
│   ├── YAML 前置資料（名稱、描述）
│   └── Markdown 指令
└── 打包資源（選填）
    ├── scripts/     - 可執行程式碼
    ├── references/  - 按需載入的文件
    └── assets/      - 輸出使用的檔案
```

</details>

<details>
<summary><big>🔄 建立流程</big></summary>

1. 用具體範例來理解
2. 規劃可重複使用的內容
3. 初始化（執行 init_skill.py）
4. 編輯和實作
5. 打包（執行 package_skill.py）
6. 根據實際使用來迭代

</details>

**觸發方式**：建立或更新擴展 Claude 能力的技能的請求

---

### Remotion 影片建立器

使用 React 和 [Remotion](https://remotion.dev) 程式化地製作影片。這個技能讓 Claude 擁有 Remotion 框架的專業知識——動畫、合成、素材、字幕等。

> **致謝**：此技能來自 Jonny Burger 和 Remotion 團隊的官方 [remotion-dev/skills](https://github.com/remotion-dev/skills) 倉庫。

**核心理念**：影片就是 React 元件。Claude 撰寫程式碼，你即時看到渲染結果，然後匯出為 MP4/WebM。

<details>
<summary><big>🚀 設定：建立 Remotion 專案</big></summary>

**步驟 1：** 建立新的 Remotion 專案：

```bash
npx create-video@latest
```

出現提示時，選擇：
- **Empty**（空白）範本
- **啟用 TailwindCSS**（建議）
- **安裝技能**（這會自動加入 Remotion 技能）

**步驟 2：** 啟動開發伺服器：

```bash
cd my-video
npm install
npm run dev
```

這會開啟瀏覽器預覽，你可以即時看到影片渲染。

**步驟 3：** 將 Claude 指向你的專案：

```
「編輯我位於 /path/to/my-video 的 Remotion 專案」
```

Claude 會讀取專案結構並編輯 `src/` 中的 React 元件。

</details>

<details>
<summary><big>🎬 你可以建立什麼</big></summary>

技能包含 28 個模組化規則，涵蓋：

- **動畫** - 內插、彈簧效果、緩動曲線
- **合成** - 多場景影片、動態時長
- **素材** - 圖片、影片、音訊、字型、GIF、Lottie
- **文字** - 打字機效果、文字高亮、文字測量
- **字幕** - TikTok 風格字幕、SRT 匯入、轉錄
- **圖表** - 動畫長條圖、資料視覺化
- **3D** - Three.js 和 React Three Fiber 整合
- **轉場** - 場景轉場、排序模式
- **地圖** - 動畫 Mapbox 地圖

</details>

<details>
<summary><big>💡 提示技巧</big></summary>

**好的提示**描述可以在網頁介面中繪製的內容：
- 「建立一個 10 秒的片頭，我的 Logo 淡入，然後文字逐字打出」
- 「加入一個長條圖，數值從 0 動畫到最終值」
- 「讓標題用彈簧動畫彈入」

**不要**要求 Claude 一次做出整支影片——一次做一個合成才是正確的方式。

</details>

<br/>

**觸發方式**：提到「Remotion」、包含 `remotion-videos/` 的路徑、`remotion.config.ts` 等檔案，或請求製作動畫 React 影片內容

---

## 客製化指南

這些技能開箱即用，但有些包含品牌特定的設定，你會想要根據自己的需求進行客製化。

### 需要客製化的項目

| 技能 | 需要的客製化 | 工作量 |
|-------|------------------------|--------|
| **SOP 建立器** | 直接使用 | 無 |
| **技能建立器** | 直接使用 | 無 |
| **Remotion** | 在外部建立專案 | 低 |
| **MCP 客戶端** | 建立設定檔 + 加入 API 金鑰 | 低 |
| **PPTX 產生器** | 需要設定品牌系統 | 中等 |

<details>
<summary><big>🟢 零工作量：SOP 建立器和技能建立器</big></summary>

這些技能專注於對任何人都適用的通用原則和工作流程。無需任何設定即可立即使用。

</details>

<details>
<summary><big>🟡 低工作量：Remotion</big></summary>

1. 執行 `npx create-video@latest` 建立 Remotion 專案
2. 選擇空白範本 + TailwindCSS + 安裝技能
3. 啟動 `npm run dev` 並告訴 Claude 專案路徑

技能提供領域知識——專案本身存放在此倉庫之外。

</details>

<details>
<summary><big>🟡 低工作量：MCP 客戶端</big></summary>

1. 將 `example-mcp-config.json` 複製為 `mcp-config.json`
2. 加入你的 API 金鑰（Zapier、GitHub 權杖等）
3. 測試每個伺服器的工具並在 `CLAUDE.md` 中記錄注意事項

設定格式與 Claude Desktop 相同，所以你可以重複使用現有的設定。

</details>

<details>
<summary><big>🟡 中等工作量：PPTX 產生器</big></summary>

這個技能需要在產生投影片之前完成完整的品牌設定。使用**品牌與語調產生器**技能互動式地建立這些檔案，或手動從範本資料夾複製。

**必要檔案**（在 `brands/your-brand-name/` 中）：

1. **`brand.json`** - 顏色（10 個值）、字型（3 個）和素材路徑
2. **`config.json`** - 輸出目錄、批次大小、檔案命名
3. **`brand-system.md`** - 設計哲學、顏色理念、排版規則、標誌性元素
4. **`tone-of-voice.md`** - 語調特色、詞彙模式、該做與不該做、範例用語

**設定步驟**：
1. 執行品牌與語調產生器技能，或
2. 將 `brands/template/` 複製到 `brands/your-brand-name/`
3. 替換每個檔案中的所有 `REPLACE` 佔位符
4. 將你的 Logo 放入 `brands/your-brand-name/assets/`
5. 產生一個簡單的簡報來測試

參見 `brands/dynamous/` 了解完整的品牌設定範例。

</details>

---

## 快速開始

1. **製作 SOP 和文件**：直接使用 SOP 建立器——開箱即用
2. **建立新技能**：使用技能建立器來打造你自己的擴充
3. **MCP 整合**：
   - 將 `example-mcp-config.json` 複製為 `mcp-config.json` 並加入你的 API 金鑰
   - 請 Claude 測試工具並在 `CLAUDE.md` 中記錄注意事項
4. **製作影片**：
   - 執行 `npx create-video@latest` 建立 Remotion 專案
   - 用 `npm run dev` 啟動開發伺服器
   - 告訴 Claude 專案路徑並開始下達指令
5. **製作簡報**：
   - 首先，執行品牌與語調產生器來設定你的品牌
   - 或手動複製並設定 `brands/` 中的範本資料夾
   - 然後用 PPTX 產生器技能來產生投影片

---

*這些技能展示了擴展 Claude Code 是很直觀的——只是組織良好的上下文，讓代理程式成為你特定工作流程的專家。*
