---
name: brand-voice-generator
description: |
  Generate tone-of-voice and brand-system files for use with the PPTX Generator and other brand-aware skills.

  TRIGGERS - Use this skill when user says:
  - "help me create a brand system" / "set up my brand"
  - "generate my tone of voice" / "create my voice guidelines"
  - "configure my brand for presentations" / "set up brand for slides"
  - "create brand files" / "initialize my brand"
  - Any request about defining brand identity, voice, or visual system for content creation

  Creates brand.json, config.json, brand-system.md, and tone-of-voice.md files.
---

# 品牌與語調產生器

產生完整的品牌設定檔，供 PPTX 產生器和其他品牌感知技能使用。此技能引導使用者透過互動流程來定義品牌識別和寫作語調。

## 產生的檔案

| 檔案 | 用途 | 使用者 |
|------|---------|---------|
| `brand.json` | 顏色、字型、素材 | PPTX 產生器、Excalidraw |
| `config.json` | 輸出設定 | PPTX 產生器 |
| `brand-system.md` | 設計哲學與準則 | 所有內容技能 |
| `tone-of-voice.md` | 寫作語調與個性 | LinkedIn、X、PPTX 內容 |

## 輸出位置

檔案建立在：
```
.claude/skills/pptx-generator/brands/{brand-name}/
```

---

## 流程概覽

1. **收集品牌基本資訊** - 名稱、描述、主要用途
2. **定義顏色** - 完整系統的 10 個色值
3. **定義字型** - 標題、內文和程式碼字型
4. **定義素材** - Logo 和圖示路徑
5. **探索語調** - 個性、詞彙、句型模式
6. **建立設計哲學** - 核心原則和標誌性元素
7. **產生檔案** - 用收集到的資訊建立所有四個檔案
8. **驗證設定** - 確認檔案正確放置

---

## 步驟 1：收集品牌基本資訊

詢問使用者：

| 欄位 | 說明 | 範例 |
|-------|-------------|---------|
| **品牌名稱** | 資料夾名稱（小寫、無空格、可用連字號） | `my-brand`、`acme-corp` |
| **顯示名稱** | 易讀的名稱 | "My Brand"、"ACME Corporation" |
| **描述** | 一行品牌描述 | "AI 教育內容和社群" |
| **主要用途** | 主要內容類型 | 簡報、社群媒體、文件 |

**建議提問：**
> 「讓我們來設定你的品牌。你的品牌名稱是什麼？（這將作為資料夾名稱——使用小寫和連字號，例如 'my-brand'）」

---

## 步驟 2：定義顏色

收集 10 個色值。顏色應為不含 `#` 前綴的十六進位碼。

### 必要顏色

| 顏色 | 用途 | 指引 |
|-------|---------|----------|
| `background` | 主要投影片/頁面背景 | 深色主題：接近黑色。淺色主題：白色/灰白 |
| `background_alt` | 替代背景增加變化 | 背景色的略微不同色調 |
| `text` | 主要文字顏色 | 與背景高對比 |
| `text_secondary` | 次要/較淡文字 | 比主要文字對比略低 |
| `accent` | 主要強調色（行動呼籲、高亮） | 你的品牌標誌色 |
| `accent_secondary` | 次要強調色 | 主要強調色的補色 |
| `accent_tertiary` | 第三強調色（選填的多樣性） | 另一個補色，或與次要相同 |
| `code_bg` | 程式碼區塊背景 | 比主背景更深 |
| `card_bg` | 卡片/表面背景 | 介於背景和文字之間 |
| `card_bg_alt` | 替代卡片背景 | card_bg 的輕微變化 |

### 顏色探索提問

如果使用者沒有完整的顏色系統，引導他們：

1. 「你的品牌標誌色是什麼？（這會成為你的主要強調色）」
2. 「你偏好深色主題（深色背景、淺色文字）還是淺色主題？」
3. 「你有次要顏色嗎？還是需要我建議補色？」

### 依主題建議顏色

**深色主題**，建議：
```
background: 0a0a0a 到 1a1a2e 範圍
text: f5f5f5 到 ffffff 範圍
card_bg: 1a1a1a 到 2d2d44 範圍
```

**淺色主題**，建議：
```
background: ffffff 到 f8f9fa 範圍
text: 1a1a1a 到 333333 範圍
card_bg: f0f0f0 到 e8e8e8 範圍
```

---

## 步驟 3：定義字型

收集 3 個字型名稱：

| 字型 | 用途 | 常見選擇 |
|------|---------|----------------|
| `heading` | 標題、大標題、按鈕 | Inter、Montserrat、Poppins、Roboto |
| `body` | 內文、描述 | Inter、Open Sans、Lato、Source Sans Pro |
| `code` | 程式碼區塊、終端 | JetBrains Mono、Fira Code、Monaco、Consolas |

**提問：**
> 「我們應該使用什麼字型？我需要標題字型、內文字型和程式碼/等寬字型。常見選擇是標題和內文都用 Inter，程式碼用 JetBrains Mono。」

**不確定時的預設：**
- 標題：Inter
- 內文：Inter
- 程式碼：JetBrains Mono

---

## 步驟 4：定義素材

收集素材路徑（相對於品牌資料夾）：

| 素材 | 說明 | 常見 |
|-------|-------------|--------|
| `logo` | 主要 Logo 檔案 | `assets/logo.png` |
| `logo_dark` | 深色背景用的 Logo 變體（選填） | `null` 或 `assets/logo-dark.png` |
| `icon` | 方形圖示（選填） | `null` 或 `assets/icon.png` |

**提問：**
> 「你有 Logo 檔案嗎？如果有，你需要將它複製到品牌資料夾。檔名是什麼？（例如 'logo.png'）」

如果他們有 Logo，將路徑設為 `assets/logo.png`（他們會複製過去）。
如果沒有 Logo，設為 `null`。

---

## 步驟 5：探索語調

這是對內容品質最重要的步驟。引導使用者進行語調探索。

### 語調探索提問

提出以下問題來理解他們的語調：

1. **用 3 個詞描述個性：**
   > 「用恰好 3 個詞描述你的品牌個性。例如：『大膽、技術、平易近人』或『沉穩、權威、有幫助』」

2. **語調影響：**
   > 「你欣賞哪 2-3 位創作者、品牌或人物的溝通風格？這幫助我理解氛圍。」

3. **詞彙模式：**
   > 「你常用什麼詞或短語？有任何口頭禪、你喜歡的加強詞（'超級'、'非常'），或你自然會用的詞嗎？」

4. **避免什麼：**
   > 「你想避免什麼樣的語調或用語？（例如，企業腔、炒作語言、過於隨意）」

5. **句型風格：**
   > 「你偏好：簡短有力的句子、較長流暢的解釋，還是混合使用？你會隨意使用縮寫嗎？」

6. **教學風格（如適用）：**
   > 「當解釋複雜的東西時，你怎麼處理？展示過程/迭代？使用類比？先講實務再講理論？」

### 語調範本參考

閱讀 `references/voice-templates.md` 以取得語調設定範例來展示給使用者。

---

## 步驟 6：建立設計哲學

根據收集的資訊，幫助定義：

### 核心原則（3-4 個）

提問：
> 「有哪 3-4 個設計原則引導你的視覺選擇？例如：『簡潔勝過花俏』、『技術但平易近人』、『預設深色模式』」

### 標誌性元素

提問：
> 「你有任何標誌性的視覺元素嗎？例如：玻璃卡片效果、特定的陰影風格、漸層模式、幾何形狀、發光效果」

如果不確定，根據他們的顏色和主題來建議。

---

## 步驟 7：產生檔案

用收集到的資訊建立所有四個檔案。

### brand.json

```json
{
  "name": "{display_name}",
  "description": "{description}",

  "colors": {
    "background": "{background}",
    "background_alt": "{background_alt}",
    "text": "{text}",
    "text_secondary": "{text_secondary}",
    "accent": "{accent}",
    "accent_secondary": "{accent_secondary}",
    "accent_tertiary": "{accent_tertiary}",
    "code_bg": "{code_bg}",
    "card_bg": "{card_bg}",
    "card_bg_alt": "{card_bg_alt}"
  },

  "fonts": {
    "heading": "{heading_font}",
    "body": "{body_font}",
    "code": "{code_font}"
  },

  "assets": {
    "logo": "{logo_path}",
    "logo_dark": {logo_dark_path},
    "icon": {icon_path}
  }
}
```

### config.json

```json
{
  "output": {
    "directory": "output/{brand_name}",
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

### tone-of-voice.md

使用 `references/tone-template.md` 中的範本，填入收集到的語調資訊。

### brand-system.md

使用 `references/brand-template.md` 中的範本，填入收集到的設計資訊。

---

## 步驟 8：驗證設定

產生檔案後：

1. **列出已建立的檔案：**
   ```
   Glob: .claude/skills/pptx-generator/brands/{brand-name}/*
   ```

2. **提醒素材：**
   > 「別忘了將你的 Logo 檔案複製到 `.claude/skills/pptx-generator/brands/{brand-name}/assets/`」

3. **建議測試：**
   > 「試著產生一個簡單的簡報：『為 {brand-name} 建立一個關於 [主題] 的 3 張投影片簡報』」

---

## 快速模式

如果使用者只想快速開始：

1. 只詢問：品牌名稱、標誌色和深/淺色偏好
2. 為其他一切產生合理的預設值
3. 建立最小但功能完整的檔案
4. 告知他們之後可以調整

**快速模式觸發語：**
- 「快速設定」
- 「只要基本的」
- 「最小品牌」
- 「快速開始」

---

## 更新現有品牌

如果品牌已存在：

1. **先讀取現有檔案**
2. **詢問要更新什麼：**
   - 只更新顏色？
   - 只更新語調？
   - 全部更新？
3. **保留未變更的區段**
4. **寫入前顯示變更的差異**

---

## 檢查清單

- [ ] 收集品牌名稱和描述
- [ ] 收集所有 10 個色值（或建議預設值）
- [ ] 收集 3 個字型名稱（或使用預設值）
- [ ] 確定素材路徑
- [ ] 透過提問探索語調
- [ ] 定義設計原則
- [ ] 產生 brand.json
- [ ] 產生 config.json
- [ ] 產生 tone-of-voice.md
- [ ] 產生 brand-system.md
- [ ] 建立 assets 資料夾
- [ ] 驗證所有檔案已建立
- [ ] 提醒使用者複製 Logo
- [ ] 建議測試產生
