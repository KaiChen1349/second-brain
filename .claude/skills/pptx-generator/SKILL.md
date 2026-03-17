---
name: pptx-generator
description: |
  Generate and edit presentation slides as PPTX files. Also create LinkedIn carousels and manage reusable slide layouts.

  TRIGGERS - Use this skill when user says:
  - "create slides for [brand]" / "generate presentation for [brand]" / "make slides for [brand]"
  - "create a carousel for [brand]" / "linkedin carousel" / "make a carousel about [topic]"
  - "edit this pptx" / "update the slides" / "modify this presentation"
  - "create a new layout" / "add a layout to the cookbook" / "make a [type] layout template"
  - "edit the [name] layout" / "update the cookbook" / "improve the [name] template"
  - Any request mentioning slides, presentations, carousels, PPTX, or layouts with a brand name

  Creates .pptx files compatible with PowerPoint, Google Slides, and Keynote.
  Creates PDF carousels for LinkedIn (square 1:1 format).
---

# PPTX 投影片產生器

使用 python-pptx 產生專業、符合品牌風格的簡報投影片。此技能支援：
- **投影片產生** - 為 `brands/` 中的任何品牌建立簡報
- **輪播圖產生** - 建立 LinkedIn 輪播圖（正方形格式，匯出為 PDF）
- **投影片編輯** - 修改現有的 PPTX 檔案
- **版面管理** - 建立、編輯、更新版面範本庫版面

**重要：** 所有技能資源都在 `.claude/skills/pptx-generator/` 中。搜尋檔案時，請始終使用以 `.claude/skills/pptx-generator/` 開頭的 Glob 模式。

---

## ⚠️ 關鍵：批次產生規則

**一次切勿產生超過 5 張投影片。**

| 規則 | 詳情 |
|------|------|
| 每批次最多投影片數 | **5**（可以是 1、2、3、4 或 5） |
| 每批次結束後 | **停下來驗證輸出** |
| 必須驗證 | 檢查：沒有重複標題、正確的間距、正確的顏色 |
| 繼續的條件 | 驗證通過 |
| **所有批次完成後** | **合併成單一檔案並刪除分割檔案** |

這可以防止 token 上限錯誤，並及早發現品質問題。

**關鍵：合併後務必清理分割檔案。** 使用者應該只看到一個最終 PPTX 檔案，而不是多個分割檔案。

---

## ⚠️ 前置條件：品牌檢查

**在產生投影片之前，請檢查是否有任何品牌存在。**

```
Glob: .claude/skills/pptx-generator/brands/*/brand.json
```

**如果找不到品牌（只有 `template/` 存在）：**

1. **停止** - 不要繼續產生投影片
2. **詢問使用者：**
   > "尚未設定任何品牌。您希望我先幫您建立一個品牌嗎？
   > 我需要您的品牌色彩、字型和風格指南來完成設定。"

3. **如果使用者想建立品牌**，請按照下方的**建立新品牌**章節操作。

4. **如果使用者拒絕**，解釋投影片需要品牌設定，並提供使用通用樣式作為備選方案。

---

## 建立新品牌

當沒有品牌存在或使用者要求建立新品牌時：

### 步驟 1：閱讀範本

```
Read: .claude/skills/pptx-generator/brands/template/README.md
Read: .claude/skills/pptx-generator/brands/template/brand.json
Read: .claude/skills/pptx-generator/brands/template/config.json
```

### 步驟 2：收集品牌資訊

向使用者詢問（或從提供的素材中提取）：

| 必填 | 說明 |
|------|------|
| **品牌名稱** | 資料夾名稱（小寫、無空格） |
| **色彩** | 背景色、文字色、強調色（十六進位色碼） |
| **字型** | 標題字型、內文字型、程式碼字型 |

| 選填 | 說明 |
|------|------|
| 輸出目錄 | 產生檔案的儲存位置（預設：`output/{brand}`） |
| 標誌 | 標誌檔案路徑（PNG/SVG） |
| 品牌指南 | 現有的風格指南或參考網站 |
| 語氣風格 | 書寫風格、用語偏好 |

### 步驟 3：建立品牌檔案

1. **建立品牌資料夾：**
   ```bash
   mkdir -p .claude/skills/pptx-generator/brands/{brand-name}
   ```

2. **建立 brand.json**，填入收集到的值：
   ```json
   {
     "name": "Brand Name",
     "description": "One-line description",
     "colors": {
       "background": "hex-without-hash",
       "background_alt": "hex-without-hash",
       "text": "hex-without-hash",
       "text_secondary": "hex-without-hash",
       "accent": "hex-without-hash",
       "accent_secondary": "hex-without-hash",
       "accent_tertiary": "hex-without-hash",
       "code_bg": "hex-without-hash",
       "card_bg": "hex-without-hash",
       "card_bg_alt": "hex-without-hash"
     },
     "fonts": {
       "heading": "Font Name",
       "body": "Font Name",
       "code": "Monospace Font"
     },
     "assets": {
       "logo": "assets/logo.png",
       "logo_dark": null,
       "icon": null
     }
   }
   ```

3. **建立 config.json**，填入輸出設定：
   ```json
   {
     "output": {
       "directory": "output/{brand}",
       "naming": "{name}-{date}",
       "keep_parts": false
     },
     "generation": {
       "slides_per_batch": 5,
       "auto_combine": true,
       "open_after_generate": false
     },
     "defaults": {
       "slide_width_inches": 13.333,
       "slide_height_inches": 7.5
     }
   }
   ```

4. **建立 brand-system.md** - 從範本複製並填入品牌指南

5. **建立 tone-of-voice.md** - 從範本複製並填入語氣風格指南

6. **新增素材** - 將標誌/圖片複製到 `brands/{brand-name}/assets/`

### 步驟 4：驗證

建立品牌後，使用以下方式驗證：
```
Glob: .claude/skills/pptx-generator/brands/{brand-name}/*
```

然後繼續產生投影片。

---

## 技能模式

此技能有三種模式：

### 模式 1：產生簡報投影片
使用者希望使用品牌樣式建立簡報投影片（16:9）。
→ 依序執行：品牌探索 → 版面選擇 → 內容調整 → 執行
→ 版面位於：`cookbook/*.py`

### 模式 2：產生 LinkedIn 輪播圖
使用者希望為社群媒體建立 LinkedIn 輪播圖（正方形 1:1 格式）。
→ 依序執行：品牌探索 → 輪播圖規劃 → 產生 → 匯出 PDF
→ 版面位於：`cookbook/carousels/*.py`

### 模式 3：管理版面範本庫
使用者希望建立、編輯或改善版面範本。
→ 依序執行：版面 CRUD 操作章節

---

## 模式 1：產生簡報投影片

### 步驟 1：品牌探索

1. **列出可用品牌：**
   ```
   Glob: .claude/skills/pptx-generator/brands/*/brand.json
   ```
   從路徑提取唯一的品牌名稱（例如 `brands/rasmus/...` → "rasmus"）

2. **讀取品牌設定檔案：**
   ```
   Read: .claude/skills/pptx-generator/brands/{brand-name}/brand.json
   Read: .claude/skills/pptx-generator/brands/{brand-name}/config.json
   ```
   - `brand.json` - 色彩、字型、素材
   - `config.json` - 輸出目錄、產生設定

3. **讀取輔助 Markdown 檔案以獲取上下文：**
   ```
   Glob: .claude/skills/pptx-generator/brands/{brand-name}/*.md
   ```
   這些提供語氣、風格和設計理念。

4. **從品牌檔案中提取：**
   - **從 brand.json：** 色彩（不含 # 的十六進位值）、字型、素材路徑
   - **從 config.json：** 輸出目錄、每批次投影片數、命名規則
   - **從 Markdown：** 語氣、風格、用語、視覺原則

如果找不到品牌，列出可用品牌並請使用者選擇。

### 步驟 2：版面探索（閱讀所有前置資訊）

**⚠️ 必須：在選擇任何版面之前，先閱讀所有版面的前置資訊。**

此步驟對於做出明智的版面決策至關重要。你必須了解所有版面的功能後才能做出選擇。

**步驟 2a：探索所有版面：**
```
Glob: .claude/skills/pptx-generator/cookbook/*.py
```

**步驟 2b：閱讀每個版面檔案**（不是只讀一兩個）：

對於找到的每個 `.py` 檔案，讀取前 40 行以提取 `# /// layout` 前置資訊區塊。建立心智地圖：
- 每個版面的用途（`purpose`、`best_for`）
- 每個版面不應使用的情況（`avoid_when`）
- 限制和約束（`max_*`、`min_*`、`*_max_chars`）

**前置資訊區塊**的格式如下：

```python
# /// layout
# name = "floating-cards-slide"
# purpose = "Feature highlights, process steps, multiple equal items with depth"
# best_for = [
#     "Exactly 3 related features or concepts",
#     "Process with 3 steps",
# ]
# avoid_when = [
#     "More than 3 items - use multi-card-slide instead",
#     "Long card titles (over 15 characters)",
# ]
# max_cards = 3
# card_title_max_chars = 15
# instructions = [
#     "EXACTLY 3 cards required - no more, no less",
#     "Card titles must be SHORT: 1-2 words, max 15 characters",
# ]
# ///
```

**關鍵前置資訊欄位：**
| 欄位 | 說明 |
|------|------|
| `name` | 版面識別名稱 |
| `purpose` | 此版面的用途 |
| `best_for` | 理想使用情境（陣列） |
| `avoid_when` | 不應使用此版面的情況（陣列） |
| `max_*` / `min_*` | 項目限制（卡片、項目符號、統計數據） |
| `instructions` | 使用此版面的特定技巧 |

**步驟 2c：選擇版面**（僅在閱讀所有前置資訊後）：

既然你已經知道所有可用版面及其約束：

1. 使用者指定版面 → 使用該版面（但驗證內容是否適合）
2. 使用者描述內容 → 比對最符合的 `best_for` 標準
3. **檢查 `avoid_when`** → 不要在警告的情況下使用該版面
4. **遵守限制** → 如果內容超過 `max_*`，使用不同的版面
5. 需要多張投影片 → 為每張選擇適當的版面
6. 沒有合適的版面 → 建立自訂版面（見模式 2）

**選擇過程範例：**
- 使用者想要「AI 基礎設施的 5 大支柱」
- 你已閱讀所有前置資訊，知道：
  - `floating-cards-slide`：`max_cards = 3` → 不適合
  - `multi-card-slide`：`max_cards = 5` → 完美契合
- 選擇 `multi-card-slide`

**為什麼要閱讀所有前置資訊？**
- 版面在 `avoid_when` 中互相參照（例如「使用 multi-card-slide 替代」）
- 不了解所有選項就無法做出正確選擇
- 防止因版面不適合而需要回頭重做

### 步驟 2d：視覺優先版面選擇（多樣性關鍵）

**🎨 預設使用視覺版面。content-slide 是最後手段，不是預設選項。**

#### 多樣性問題

簡報產生中最大的錯誤是**將 content-slide**（標題 + 項目符號）作為傳達資訊的預設選擇。這會建立重複、無聊的簡報。

**常見失敗模式：**
- 30 張投影片中有 11 張是 content-slide（37% 重複率）
- 使用者說「這缺乏多樣性」
- 你認為「但我使用了 11 種不同的版面類型！」
- 現實：版面種類是有多樣性，但分佈很糟糕

#### 多樣性強制規則

**硬性限制：**
1. **同一版面連續使用不超過 2-3 次**
2. **content-slide 應佔總投影片數的 <25%**（不是 35-40%）
3. **視覺版面（卡片、統計、分欄、主視覺、對角線）應佔 50% 以上**
4. **段落分隔不算多樣性** - 它們是結構性的（不計入多樣性統計）

#### 決策樹：「我應該使用 content-slide 嗎？」

在預設使用 content-slide 之前，**依序**回答這些問題：

```
我有 3-5 個相等的項目嗎？
  是 → 使用 multi-card-slide（不是 content-slide）

我有 2-4 個大數字/指標嗎？
  是 → 使用 stats-slide（不是 content-slide）

我在比較兩樣東西嗎？
  是 → 使用 two-column-slide（不是 content-slide）

我有一個中心概念和周圍項目嗎？
  是 → 使用 circular-hero-slide（不是 content-slide）

我有剛好 3 個相關項目嗎？
  是 → 使用 floating-cards-slide（不是 content-slide）

我有 1-3 個字想要大幅強調嗎？
  是 → 使用 giant-focus-slide 或 bold-diagonal-slide（不是 content-slide）

我有一句有力的引言或原則嗎？
  是 → 使用 quote-slide（不是 content-slide）

這是呈現此資訊的唯一方式嗎？
  是 → 現在可以使用 content-slide
  否 → 回到決策樹重新評估
```

#### 將項目符號轉換為視覺版面

**範例 1：「驗證模式」**

❌ **不好（content-slide）：**
```
Title: Validation Patterns
Bullets:
- Run comprehensive test suites
- Type checking and linting
- Code review by humans and AI
- Deployment previews
```

✅ **好（multi-card-slide）：**
```
Title: Validation Patterns
Cards:
1. Testing | Run comprehensive test suites after every change
2. Linting | Type checking and formatting as guardrails
3. Review | Human and AI code review process
4. Preview | Deployment previews for visual regression
```

**範例 2：「為什麼 PIV 有效」**

❌ **不好（content-slide）：**
```
Title: Why PIV Works
Bullets:
- Forces planning before implementation
- Validation catches issues immediately
- Iterative improvements compound
- System gets smarter with every bug
```

✅ **好（floating-cards-slide，3 張卡片）：**
```
Title: Why PIV Works
Cards:
1. Plan First | Forces architectural thinking before coding
2. Fast Feedback | Validation catches issues immediately
3. Compounds | System improves with every bug
```
（注意：從 4 個減少到 3 個項目以符合 floating-cards-slide 的 `max_cards` 限制）

**範例 3：「人機協作策略」**

❌ **不好（content-slide）：**
```
Title: Human-in-the-Loop Strategy
Bullets:
- In-the-loop: Human approves before execution
- On-the-loop: Human reviews after completion
- Code review remains critical
- AI generates, humans validate
```

✅ **好（two-column-slide）：**
```
Title: Human-in-the-Loop Strategy
Left: In-the-Loop
- Human approves before execution
- Critical for production changes
- Quality gateway

Right: On-the-Loop
- Human reviews after completion
- Faster iteration cycles
- AI generates, human validates
```

#### 主動視覺思維

在規劃任何投影片之前，問自己：

1. **「這可以更視覺化嗎？」** - 答案幾乎總是「是」
2. **「我在最近 2 張投影片中使用過 content-slide 嗎？」** - 如果是，使用其他版面
3. **「這張投影片看起來和上一張一樣嗎？」** - 如果是，更換版面
4. **「我是否陷入了模式？」** - 立即打破它
5. **「以卡片/分欄/統計呈現會更吸引人嗎？」** - 通常是的

#### 適合使用 content-slide 的情況

**僅在以下情況使用 content-slide：**
- 你已經真正嘗試了所有其他版面，但都不適合
- 資訊本質上是線性和文字性的（少見）
- 你需要在兩個複雜視覺投影片之間加入「喘息」投影片
- 你已經達到版面分佈上限（最近已使用完所有視覺版面）

**永遠不要將 content-slide 作為你的預設思維。**

#### 快速參考：內容類型 → 最佳版面

| 內容類型 | 最佳版面 | 原因 |
|----------|----------|------|
| 3-5 個相等的功能/步驟 | multi-card-slide | 卡片建立視覺層次 |
| 剛好 3 個精選項目 | floating-cards-slide | 浮動卡片增加深度 |
| 2-4 個指標/KPI | stats-slide | 大數字吸引注意力 |
| 前後比較 | two-column-slide | 並排顯示對比 |
| 中心概念與類型 | circular-hero-slide | 放射狀模式展示關係 |
| 戲劇性強調（1-3 個字） | giant-focus-slide | 放大比例產生衝擊力 |
| 高能量警告 | bold-diagonal-slide | 動態形狀傳達緊迫感 |
| 有力的引言/原則 | quote-slide | 出處增加權威性 |
| 相關項目清單 | multi-card-slide | 比項目符號更好 |
| 有步驟的流程 | floating-cards-slide | 視覺流程優於文字 |
| 技術比較 | two-column-slide | 結構化比較 |

**僅在以下情況使用 content-slide：**
- 以上都不適合
- 資訊確實是線性的
- 需要在視覺投影片之間加入文字量較大的喘息投影片
- 已達到多樣性上限

### 步驟 3：投影片規劃（務必執行）

**在產生任何投影片之前，建立書面計畫。**

這適用於單張投影片、批次和完整簡報。規劃可以防止：
- 投影片間內容重複
- 版面選擇錯誤
- 遺漏關鍵資訊
- 不良的流程和結構

**建立投影片計畫表：**

```markdown
| # | 版面 | 標題 | 關鍵內容 | 備註 |
|---|------|------|----------|------|
| 1 | title-slide | [標題] | [副標題、作者] | 開場投影片 |
| 2 | content-slide | [標題] | [3-4 個要點] | 主要概念 |
| 3 | stats-slide | [標題] | [2-3 個指標] | 影響數據 |
| ... | ... | ... | ... | ... |
```

**對於每張投影片，指定：**
- **投影片編號** - 在簡報中的位置
- **版面** - 使用哪個版面範本庫版面
- **標題** - 確切的標題文字（檢查是否重複！）
- **關鍵內容** - 要點、統計數據、引言等
- **備註** - 任何特殊考量

**規劃檢查清單：**
- [ ] 投影片之間沒有重複標題
- [ ] 投影片之間有邏輯流程
- [ ] 每種內容類型使用適當的版面
- [ ] 內容適合所選版面
- [ ] 批次按邏輯分組（每批最多 5 張）
- [ ] **多樣性檢查：content-slide 使用率 <25%**
- [ ] **多樣性檢查：同一版面連續使用不超過 2-3 次**
- [ ] **多樣性檢查：視覺版面佔投影片 50% 以上**
- [ ] **多樣性檢查：每個 content-slide 都先經過決策樹評估**

**規劃完成後，在產生前簡要呈現計畫。**

**30 張投影片簡報的良好多樣性分佈範例：**
- Content-slide：6-7 張（20-23%）
- 段落分隔：5 張（17%）
- 視覺版面：15-16 張（50-53%）
  - Multi-card：3-4 張
  - Two-column：2-3 張
  - Stats：1-2 張
  - Floating-cards：2-3 張
  - Circular-hero：1-2 張
  - Giant-focus/Bold-diagonal：2-3 張
  - Quote：1 張
- 標題/結尾：2-3 張（7-10%）

### 步驟 4：內容調整

**對於計畫中的每張投影片：**

#### 簡報文字格式規則

**重要：所有投影片文字都要遵循這些規則。**

| 元素 | 規則 | 範例 |
|------|------|------|
| 標題 | 不加結尾句點或逗號 | "Why AI Matters" 而非 "Why AI Matters." |
| 副標題 | 不加結尾標點 | "The future of coding" 而非 "The future of coding." |
| 項目符號 | 不加結尾句點（除非是完整句子） | "Faster development" 而非 "Faster development." |
| 大標題 | 最少標點，不用省略號 | "What's Next" 而非 "What's Next..." |
| 統計/數字 | 乾淨格式，不加結尾標點 | "50%" 而非 "50%." |
| 行動呼籲 | 不加結尾標點 | "Get Started" 而非 "Get Started." |
| 標籤 | 簡短，不加標點 | "Step 1" 而非 "Step 1:" |

**避免：**
- 標題、項目符號、標籤的結尾句點
- 大標題中的省略號（...）
- 短語中過多的逗號
- 標籤/標頭結尾的冒號
- 項目符號中的分號

**例外：** 完整句子的描述或引言可以使用適當的標點。

#### 品牌值對應

1. **將 brand.json 的值對應到版面佔位符：**

   | 版面佔位符 | brand.json 路徑 |
   |-----------|-----------------|
   | `BRAND_BG` | `colors.background` |
   | `BRAND_BG_ALT` | `colors.background_alt` |
   | `BRAND_TEXT` | `colors.text` |
   | `BRAND_TEXT_SECONDARY` | `colors.text_secondary` |
   | `BRAND_ACCENT` | `colors.accent` |
   | `BRAND_ACCENT_SECONDARY` | `colors.accent_secondary` |
   | `BRAND_ACCENT_TERTIARY` | `colors.accent_tertiary` |
   | `BRAND_CODE_BG` | `colors.code_bg` |
   | `BRAND_CARD_BG` | `colors.card_bg` |
   | `BRAND_CARD_BG_ALT` | `colors.card_bg_alt` |
   | `BRAND_HEADING_FONT` | `fonts.heading` |
   | `BRAND_BODY_FONT` | `fonts.body` |
   | `BRAND_CODE_FONT` | `fonts.code` |

   **注意：** brand.json 中的所有顏色值都是不含 `#` 前綴的十六進位值。

2. **以品牌語氣撰寫內容**（來自 tone-of-voice.md）
3. **保留版面結構**（裝飾元素、間距、層次）

### 步驟 5：批次產生（關鍵）

**每批次最多 5 張投影片。這是硬性限制。**

產生多張投影片時：
1. 在單一 PPTX 檔案中產生 1-5 張投影片
2. **在產生更多之前停下來檢查輸出**
3. 只有在驗證通過後，才繼續下一批次
4. 重複直到所有投影片產生完成

**為什麼要批次處理：**
- 防止 token 上限錯誤
- 允許在批次之間進行品質檢查
- 及早發現問題，避免問題擴散

**⚠️ 關鍵背景色錯誤修復：**

**每張投影片都必須明確設定背景。** 如果你不設定 `slide.background.fill.solid()` 和 `slide.background.fill.fore_color.rgb`，投影片將使用 PowerPoint 預設的白色背景，導致深色主題品牌的文字無法閱讀。

**每張投影片的必要設定：**
```python
slide = prs.slides.add_slide(prs.slide_layouts[6])
slide.background.fill.solid()  # ← 必要
slide.background.fill.fore_color.rgb = hex_to_rgb(BRAND_BG)  # ← 必要
```

這在以下情況尤其關鍵：
- 產生多個批次（每個批次是新的 Presentation 物件）
- 使用輔助函式建立投影片
- 合併多個 PPTX 檔案

**執行：**

**建議：使用 heredoc（不建立檔案）：**
```bash
uv run --with python-pptx==1.0.2 python << 'EOF'
# [已調整品牌值和內容的程式碼]
EOF
```

**如果 heredoc 失敗（Windows 問題）：使用暫存目錄：**
```bash
# 如需要建立暫存目錄
mkdir -p .claude/skills/pptx-generator/.tmp

# 將腳本寫入暫存目錄
# （在 .claude/skills/pptx-generator/.tmp/gen.py 建立檔案）

# 執行
uv run --with python-pptx==1.0.2 python .claude/skills/pptx-generator/.tmp/gen.py

# 必須：執行後立即清理
rm .claude/skills/pptx-generator/.tmp/gen.py
```

**關鍵：永遠不要在專案根目錄建立 Python 檔案。** 始終使用 heredoc 或技能資料夾內的暫存目錄。

### 步驟 6：品質驗證（必須執行）

**每個批次後，在繼續之前必須驗證：**

1. **開啟產生的 PPTX** 並進行目視檢查
2. **檢查這些常見問題：**

| 問題 | 檢查重點 | 修復方式 |
|------|----------|----------|
| 白色背景 | 投影片背景為白色而非品牌色 | 加入 slide.background.fill.solid() 並設定 fore_color.rgb |
| 重複標題 | 同一投影片上出現相同的標題文字 | 移除重複的文字方塊 |
| 間距問題 | 標題與副標題/內容太近 | 增加下方元素的 Y 座標 |
| 文字溢出 | 內容超出投影片邊界 | 縮小字型大小或分割內容 |
| 缺少元素 | 裝飾元素未渲染 | 檢查形狀位置和顏色 |
| 顏色錯誤 | 顏色與品牌不符 | 驗證十六進位值（程式碼中不含 # 前綴） |
| 錯誤標點 | 標題/項目符號上有結尾句點/逗號 | 移除不必要的標點 |

3. **如果發現問題：**
   - 在繼續之前修復當前批次
   - 記錄問題以避免在未來批次中重複

4. **如果驗證通過：**
   - 繼續下一批次投影片

### 步驟 7：輸出

**使用 config.json 中的輸出設定：**

| 設定項目 | 預設值 | 說明 |
|----------|--------|------|
| `output.directory` | `output/{brand}` | 檔案儲存位置 |
| `output.naming` | `{name}-{date}` | 檔案命名模式 |
| `output.keep_parts` | `false` | 合併後是否保留分割檔案 |

**解析佔位符：**
- `{brand}` → 品牌資料夾名稱
- `{name}` → 使用者要求的簡報名稱
- `{date}` → 目前日期（YYYY-MM-DD）

```bash
# 從設定建立輸出目錄
mkdir -p {resolved-output-directory}
```

**批次產生工作流程：**
1. 將每個批次產生為 `{name}-part1.pptx`、`{name}-part2.pptx` 等
2. 在繼續之前驗證每個批次
3. 所有批次完成後，**合併成最終檔案**（如果 `auto_combine` 為 true）
4. 刪除分割檔案（如果 `keep_parts` 為 false）

### 步驟 8：合併批次（適用於多批次簡報）

**🚨 關鍵錯誤警告：合併時必須設定背景 🚨**

合併簡報時，`add_slide()` 建立的投影片具有**預設白色背景**。形狀複製不會複製投影片背景屬性。你必須在建立每張新投影片後立即明確設定背景。

**這是合併簡報中白色投影片最常見的原因。**

所有批次驗證完成後，合併成單一 PPTX：

```bash
uv run --with python-pptx==1.0.2 python << 'SCRIPT'
from pptx import Presentation
from pptx.dml.color import RGBColor
from pathlib import Path
import shutil

def hex_to_rgb(hex_color: str) -> RGBColor:
    h = hex_color.lstrip("#")
    return RGBColor(int(h[0:2], 16), int(h[2:4], 16), int(h[4:6], 16))

# 品牌背景色（從 brand.json 取得）
BRAND_BG = "REPLACE_WITH_BRAND_BACKGROUND"  # 例如 "07090F"

# 依序列出所有分割檔案
output_dir = Path("output/{brand-name}")
part_files = sorted(output_dir.glob("{name}-part*.pptx"))

if len(part_files) > 1:
    # 以第一個分割檔案作為基礎
    combined = Presentation(part_files[0])

    # 從其餘分割檔案加入投影片
    for part_file in part_files[1:]:
        part_prs = Presentation(part_file)
        for slide in part_prs.slides:
            # 複製投影片版面並加入合併檔案
            blank_layout = combined.slide_layouts[6]
            new_slide = combined.slides.add_slide(blank_layout)

            # 🚨 關鍵：建立投影片後立即設定背景
            # PowerPoint 預設為白色背景 - 在複製形狀之前必須設定
            new_slide.background.fill.solid()
            new_slide.background.fill.fore_color.rgb = hex_to_rgb(BRAND_BG)

            # 從來源投影片複製所有形狀
            for shape in slide.shapes:
                # 將形狀複製到新投影片
                el = shape.element
                new_slide.shapes._spTree.insert_element_before(
                    el, 'p:extLst'
                )

    # 儲存合併檔案
    combined.save(output_dir / "{name}-final.pptx")
    print(f"已將 {len(part_files)} 個分割檔案合併為 {name}-final.pptx")

    # 必須：清理分割檔案 - 使用者應只看到最終檔案
    for part_file in part_files:
        part_file.unlink()
        print(f"已刪除 {part_file.name}")
else:
    # 只有一個分割檔案，直接重新命名
    shutil.move(part_files[0], output_dir / "{name}-final.pptx")
SCRIPT
```

**最終輸出：** `output/{brand-name}/{name}-final.pptx`

**為什麼會發生此錯誤：**
- `combined.slides.add_slide()` 建立的新投影片物件具有 PowerPoint 預設白色背景
- `shapes._spTree.insert_element_before()` 複製形狀（文字、矩形、圖片）但不複製背景
- 背景是投影片屬性，不是形狀，所以必須單獨設定
- 沒有明確設定背景，第 6-30 張（或基礎之後加入的所有投影片）將是白色的

**合併後測試檢查清單：**
- [ ] 開啟合併後的 PPTX
- [ ] 瀏覽所有投影片（不只是前幾張）
- [ ] 驗證每張投影片都有正確的背景色
- [ ] 如果發現任何白色投影片，表示合併程式碼缺少背景設定

---

## 模式 2：產生 LinkedIn 輪播圖

LinkedIn 輪播圖是正方形（1:1）格式的多頁 PDF。每頁是一張可滑動的投影片。

### 輪播圖與簡報的比較

| 面向 | 簡報 | 輪播圖 |
|------|------|--------|
| 尺寸 | 16:9（13.333" × 7.5"） | 1:1（7.5" × 7.5"） |
| 版面 | `cookbook/*.py` | `cookbook/carousels/*.py` |
| 輸出 | PPTX | PDF（透過 PPTX 轉換） |
| 投影片數 | 通常 10-50+ 張 | 最佳 5-10 張 |
| 文字大小 | 標準 | 較大（手機可讀） |
| 內容 | 詳細 | 每張一個重點 |

### 步驟 1：品牌探索

與模式 1 相同 - 讀取 brand.json、config.json 和 tone-of-voice.md。

### 步驟 2：輪播圖版面探索

**探索輪播圖專用版面：**
```
Glob: .claude/skills/pptx-generator/cookbook/carousels/*.py
```

**可用的輪播圖版面：**

| 版面 | 用途 | 最適合 |
|------|------|--------|
| `hook-slide` | 開場吸引注意力 | 僅限第一張 |
| `single-point-slide` | 一個關鍵要點加說明 | 內文內容 |
| `numbered-point-slide` | 帶大數字的編號清單項目 | 列表、步驟 |
| `quote-slide` | 帶出處的引言 | 社交證明、見解 |
| `cta-slide` | 行動呼籲 | 僅限最後一張 |

閱讀前置資訊以了解每個版面的限制和約束。

### 步驟 3：輪播圖規劃

**典型的輪播圖結構（5-10 張）：**

```markdown
| # | 版面 | 內容 |
|---|------|------|
| 1 | hook-slide | 吸引注意力的開場 |
| 2-8 | single-point 或 numbered-point | 內文內容 |
| 9/10 | cta-slide | 行動呼籲 |
```

**輪播圖內容規則：**
- **每張一個重點** - 不要塞入多個要點
- **大字體** - 必須在手機上可讀
- **簡短文案** - 標題最多 50 字元，內文最多 150 字元
- **清晰流程** - 每張投影片獨立觀看也要有意義
- **強力開場** - 第一張投影片要讓人停下滑動
- **明確行動呼籲** - 最後一張告訴他們該做什麼

### 步驟 4：產生輪播圖

**輪播圖尺寸（正方形 1:1）：**
```python
prs.slide_width = Inches(7.5)
prs.slide_height = Inches(7.5)
```

將所有投影片產生為單一 PPTX 檔案（輪播圖通常為 5-10 張，很少需要批次處理）。

**執行：**
```bash
uv run --with python-pptx==1.0.2 python << 'SCRIPT'
# 使用 7.5" x 7.5" 尺寸的輪播圖產生程式碼
SCRIPT
```

### 步驟 5：匯出為 PDF

LinkedIn 要求輪播圖貼文使用 PDF。將 PPTX 轉換為 PDF：

**方案 A：使用 LibreOffice（建議）**
```bash
libreoffice --headless --convert-to pdf --outdir output/rasmus output/rasmus/carousel.pptx
```

**方案 B：使用 soffice**
```bash
soffice --headless --convert-to pdf output/rasmus/carousel.pptx
```

**注意：** 必須安裝 LibreOffice。在 macOS 上：`brew install --cask libreoffice`

### 步驟 6：輸出

儲存兩個檔案：
- `output/{brand}/{name}-carousel.pptx` - 可編輯原始檔
- `output/{brand}/{name}-carousel.pdf` - LinkedIn 就緒版本

### 輪播圖檢查清單

- [ ] 讀取品牌設定
- [ ] 從 `cookbook/carousels/` 讀取輪播圖版面前置資訊
- [ ] 規劃輪播圖結構（開場 → 內文 → 行動呼籲）
- [ ] 保持文字簡短（檢查前置資訊中的字元限制）
- [ ] 使用 7.5" × 7.5" 尺寸
- [ ] 產生 PPTX
- [ ] 驗證輸出
- [ ] 匯出為 PDF
- [ ] 在 LinkedIn 預覽中測試 PDF

---

## 模式 3：版面 CRUD 操作

### 建立新版面

當使用者要求新的版面類型時：

1. **研究現有版面的模式：**
   ```
   Glob: .claude/skills/pptx-generator/cookbook/*.py
   ```
   閱讀 2-3 個版面以了解：
   - 程式碼結構和匯入
   - 品牌變數的使用方式
   - 裝飾元素模式
   - 定位慣例

2. **依照以下品質標準設計：**

   **必須達到生產品質：**
   - 專業、精緻的外觀
   - 視覺吸引力（不是平淡或通用的）
   - 獨特的裝飾元素
   - 強烈的視覺層次
   - 正確使用留白

   **使用適當的元素：**
   - **圖表** - 圓餅圖、甜甜圈圖、長條圖、柱狀圖用於資料視覺化
   - **圖片** - 佔位符形狀用於螢幕截圖、照片
   - **形狀** - 圓形、矩形、平行四邊形增加視覺趣味
   - **卡片** - 帶陰影的浮動卡片增加深度
   - **幾何圖案** - 錨定在角落/邊緣的粗體形狀

   **避免：**
   - 純文字版面
   - 沒有樣式的通用項目符號
   - 太小而沒有影響力的裝飾元素
   - 全部置中的無聊構圖

3. **撰寫帶有詳細前置資訊的版面檔案：**

   **⚠️ 關鍵：前置資訊是為未來 AI 代理撰寫的文件。**

   每個版面都必須包含完整的前置資訊，教導未來的 AI 代理：
   - 何時使用此版面（以及何時不使用）
   - 如何正確使用
   - 存在哪些限制和約束
   - 為什麼某些選擇很重要

   ```python
   #!/usr/bin/env -S uv run
   # /// script
   # requires-python = ">=3.11"
   # dependencies = [
   #     "python-pptx==1.0.2",
   # ]
   # ///
   # /// layout
   # name = "layout-name"
   # purpose = "When to use this layout - be specific"
   # best_for = [
   #     "Ideal use case 1",
   #     "Ideal use case 2",
   # ]
   # avoid_when = [
   #     "Situation to avoid 1 - and what to use instead",
   #     "Situation to avoid 2 - and what to use instead",
   # ]
   # max_items = 5  # or other relevant limits
   # instructions = [
   #     "Specific tip 1",
   #     "Specific tip 2",
   # ]
   # ///
   """
   LAYOUT: [Name]
   PURPOSE: [When to use this layout - be specific]

   CUSTOMIZE:
   - [List customizable elements]
   """

   # ... implementation
   ```

   **必填前置資訊欄位（要詳細且具體）：**

   | 欄位 | 說明 | 範例 |
   |------|------|------|
   | `name` | 版面識別名稱（與檔案名稱相符） | `"multi-card-slide"` |
   | `purpose` | 清晰的一行說明 | `"Multiple items as cards in a row, 3-5 cards"` |
   | `best_for` | **詳細的**理想情境陣列 | `["Exactly 3 related features", "Process with 3 steps"]` |
   | `avoid_when` | **具體的**需避免情況及替代方案 | `["More than 3 items - use multi-card-slide instead"]` |
   | `instructions` | **可操作的**正確使用技巧 | `["Card titles must be SHORT: 1-2 words, max 15 chars"]` |

   **選填但建議的欄位：**
   | 欄位 | 說明 | 範例 |
   |------|------|------|
   | `max_*` / `min_*` | 項目的硬性限制 | `max_cards = 3`、`min_surrounding_items = 4` |
   | `*_max_chars` | 文字的字元限制 | `card_title_max_chars = 15` |

   **撰寫良好前置資訊的方法：**

   ✅ **要：** 具體且可操作
   ```python
   # avoid_when = [
   #     "More than 3 items - use multi-card-slide instead",
   #     "Long card titles (over 15 characters) - abbreviate or use content-slide",
   # ]
   # instructions = [
   #     "EXACTLY 3 cards required - no more, no less",
   #     "Card titles must be SHORT: 1-2 words, max 15 characters",
   #     "If titles are too long, abbreviate or use different layout",
   # ]
   ```

   ❌ **不要：** 含糊或無用
   ```python
   # avoid_when = ["Too many items", "Wrong content"]
   # instructions = ["Use correctly", "Follow the pattern"]
   ```

   **將前置資訊視為教導同事** - 他們需要知道什麼才能在不問你問題的情況下正確使用此版面？

4. **儲存到版面範本庫：**
   ```
   .claude/skills/pptx-generator/cookbook/{layout-name}-slide.py
   ```

5. **透過產生**範例來測試新版面

### 編輯現有版面

1. **找到版面：**
   ```
   Glob: .claude/skills/pptx-generator/cookbook/*{name}*.py
   ```

2. **閱讀並了解目前結構**，包括前置資訊

3. **進行修改**，同時保留：
   - 腳本標頭格式
   - 品牌變數命名慣例
   - 文件字串格式（LAYOUT、PURPOSE、CUSTOMIZE）

4. **更新前置資訊**，如果你的變更影響到：
   - 版面最適合的用途（`best_for`）
   - 何時應避免使用（`avoid_when`）
   - 項目限制（`max_*`、`min_*`）
   - 使用說明（`instructions`）

5. **儲存回同一檔案**

6. **測試修改後的版面**

### 更新/改善版面

當被要求改善版面品質時：

1. **分析目前的弱點：**
   - 是否具有視覺吸引力？
   - 是否有足夠的裝飾元素？
   - 是否有良好的視覺層次？
   - 空間使用是否得當？

2. **套用改善：**
   - 加入粗體幾何形狀
   - 改善色彩使用
   - 增加深度（陰影、重疊）
   - 更好的字型大小
   - 更獨特的裝飾元素

3. **保留功能** - 不要破壞已經可用的功能

4. **審查並強化前置資訊：**
   - `best_for` 和 `avoid_when` 是否仍然準確？
   - `instructions` 是否反映了新的約束？
   - 加入從改善中學到的經驗
   - 如果元素大小/數量改變，更新限制

### 刪除版面

只需移除檔案：
```bash
rm .claude/skills/pptx-generator/cookbook/{layout-name}.py
```

---

## 編輯現有 PPTX 檔案

當使用者提供現有的 PPTX 時：

1. **讀取檔案：**
   ```python
   from pptx import Presentation
   prs = Presentation("path/to/existing.pptx")
   ```

2. **分析：** 投影片數量、樣式、內容結構

3. **套用變更：** 新增/移除投影片、更新內容、修改樣式

4. **儲存到輸出目錄**（除非要求，否則不要覆蓋原始檔案）

---

## 技術參考

**投影片尺寸（16:9）：**
- 寬度：13.333 英吋
- 高度：7.5 英吋
- 安全邊距：0.5 英吋

**始終使用：**
- 空白版面：`prs.slide_layouts[6]`
- python-pptx 版本：1.0.2

**常用匯入：**
```python
from pptx import Presentation
from pptx.chart.data import CategoryChartData
from pptx.dml.color import RGBColor
from pptx.enum.chart import XL_CHART_TYPE, XL_LEGEND_POSITION
from pptx.enum.shapes import MSO_SHAPE
from pptx.enum.text import PP_ALIGN, MSO_ANCHOR
from pptx.util import Inches, Pt
```

**可用的圖表類型：**
- `XL_CHART_TYPE.PIE` - 圓餅圖
- `XL_CHART_TYPE.DOUGHNUT` - 甜甜圈圖
- `XL_CHART_TYPE.BAR_CLUSTERED` - 水平長條圖
- `XL_CHART_TYPE.COLUMN_CLUSTERED` - 垂直柱狀圖
- `XL_CHART_TYPE.LINE` - 折線圖

**新增圖表：**
```python
chart_data = CategoryChartData()
chart_data.categories = ["A", "B", "C"]
chart_data.add_series("Values", [10, 20, 30])

slide.shapes.add_chart(
    XL_CHART_TYPE.DOUGHNUT,
    Inches(x), Inches(y),
    Inches(width), Inches(height),
    chart_data
)
```

**新增圖片：**
```python
slide.shapes.add_picture(
    "path/to/image.png",
    Inches(x), Inches(y),
    width=Inches(w)  # 高度自動計算
)
```

---

## 預覽所有版面

查看所有可用版面：
```bash
uv run .claude/skills/pptx-generator/generate-cookbook-preview.py
```

這會產生包含每個版面的 `cookbook-preview.pptx`。

---

## 檢查清單

**投影片產生用：**
- [ ] 讀取 brand.json 取得色彩/字型
- [ ] 讀取 config.json 取得輸出目錄和設定
- [ ] 讀取 Markdown 檔案取得語氣/風格
- [ ] **讀取所有版面範本庫版面的前置資訊**（每個 .py 的前 40 行）：
  - [ ] Glob 所有 `.claude/skills/pptx-generator/cookbook/*.py`
  - [ ] 讀取每個檔案以提取 `# /// layout` 區塊
  - [ ] 建立對所有 `best_for`、`avoid_when`、限制的理解
- [ ] **然後選擇版面**（基於完整的了解）：
  - [ ] 將內容比對 `best_for` 標準
  - [ ] 遵守 `avoid_when` 警告
  - [ ] 檢查 `max_*` / `min_*` 限制
  - [ ] 遵循所選版面的 `instructions`
- [ ] **在產生前建立投影片計畫表：**
  - [ ] 列出所有投影片的版面、標題、內容
  - [ ] 檢查重複標題
  - [ ] 驗證邏輯流程
  - [ ] 分組為批次（每批最多 5 張）
  - [ ] **多樣性檢查：content-slide <25%**
  - [ ] **多樣性檢查：同一版面連續使用不超過 2-3 次**
  - [ ] **多樣性檢查：視覺版面佔 50% 以上**
  - [ ] **多樣性檢查：使用決策樹評估每個 content-slide**
- [ ] 在繼續前簡要呈現計畫
- [ ] 套用品牌色彩和字型
- [ ] 以品牌語氣撰寫內容
- [ ] **每批次最多產生 5 張投影片**（或 config 中的 `slides_per_batch`）
- [ ] 透過 UV REPL 執行
- [ ] **繼續前驗證輸出：**
  - [ ] 沒有重複標題
  - [ ] 元素間距正確
  - [ ] 內容在邊界內
  - [ ] 顏色符合品牌
  - [ ] 標題/項目符號沒有結尾標點
- [ ] 將批次儲存到 config 中的輸出目錄
- [ ] 如需要，重複下一批次
- [ ] **將所有批次合併為最終 PPTX**（如果 `auto_combine` 為 true）
- [ ] 清理分割檔案（如果 `keep_parts` 為 false）

**建立版面用：**
- [ ] 研究現有版面的模式
- [ ] 依生產品質標準設計
- [ ] 包含獨特的裝飾元素
- [ ] 適當使用圖表/圖片/形狀
- [ ] **加入 TOML 風格前置資訊**，包含：
  - [ ] `name`、`purpose`
  - [ ] `best_for`、`avoid_when` 陣列
  - [ ] `instructions` 陣列與使用技巧
  - [ ] 需要時加入 `max_*` / `min_*` 限制
- [ ] 撰寫正確的文件字串（LAYOUT、PURPOSE、CUSTOMIZE）
- [ ] 測試新版面
