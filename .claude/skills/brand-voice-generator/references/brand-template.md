# 品牌系統範本

使用此範本產生 brand-system.md 檔案。將所有 `{placeholder}` 值替換為收集到的資訊。

---

```markdown
# {brand_name} — 品牌系統

> {brand_tagline}

---

## 品牌哲學

### 核心原則

1. **{principle_1_name}**
   {principle_1_description}

2. **{principle_2_name}**
   {principle_2_description}

3. **{principle_3_name}**
   {principle_3_description}

{additional_principles}

---

## Logo

### {brand_name} 標誌

{logo_description}

**Logo 檔案**：`.claude/skills/pptx-generator/brands/{brand_folder}/assets/logo.png`

**使用規則：**
- 維持長寬比
- 最小淨空間：Logo 寬度的 20%（四邊皆是）
{additional_logo_rules}

---

## 顏色系統

### 主要強調色 — {accent_name}

品牌標誌色。用於主要動作、連結、高亮。

| 名稱 | Hex | 用途 |
|------|-----|-----|
| {accent_name} | `#{accent}` | 主要按鈕、連結、高亮 |
| {accent_name} 深色 | `#{accent_dark}` | 懸停狀態、外框 |
| {accent_name} 淺色 | `#{accent_secondary}` | 次要強調 |

**規則：** {accent_usage_rule}

### 主題基底

**{theme_name} 主題（預設）**
```
背景：          #{background}
替代背景：      #{background_alt}
表面：          #{card_bg}
主要文字：      #{text}
次要文字：      #{text_secondary}
```

{additional_color_notes}

---

## 字型排版

### 字型組合

- **標題：** {heading_font} — {heading_description}
- **內文：** {body_font} — {body_description}
- **程式碼：** {code_font} — {code_description}

### 排版規則

| 元素 | 字型 | 字重 | 大小 |
|---------|------|--------|------|
| 展示/主視覺 | {heading_font} | 700 | 36px+ |
| H1 | {heading_font} | 700 | 30px |
| H2 | {heading_font} | 600 | 24px |
| 內文 | {body_font} | 400 | 16-18px |
| 程式碼 | {code_font} | 400 | 14px |

### 文字顏色使用

| 情境 | 顏色 | 備註 |
|---------|-------|-------|
| 標題 | #{text} | 完全對比 |
| 內文 | #{text_secondary} | 舒適閱讀 |
| 連結 | #{accent} | 帶懸停效果 |

---

## 間距系統

**基本單位：** 4px

| Token | 值 | 用途 |
|-------|-------|-----|
| `xs` | 4px | 緊密間距、圖示間隙 |
| `sm` | 8px | 相關元素 |
| `base` | 16px | 標準內距 |
| `lg` | 24px | 區段內距 |
| `xl` | 32px | 卡片內距 |
| `2xl` | 48px | 區段外距 |

**規則：** 不確定時，使用 8px 的倍數。

---

## 按鈕

### 按鈕類型

| 類型 | 背景 | 文字 | 用途 |
|------|------------|------|-----|
| 主要 | #{accent} | 白色 | 主要動作、行動呼籲 |
| 次要 | #{card_bg} | #{text} | 次要動作 |
| 幽靈 | 透明 | #{text} | 第三層動作 |

### 按鈕規格

- **字型：** {heading_font}，500 字重
- **圓角：** 8px
- **內距：** 8px 16px（標準）、12px 24px（大型）

---

## 標誌性元素

{signature_elements_description}

{signature_element_1}

{signature_element_2}

{signature_element_3}

---

## 圖表

### 圖表用色盤

| 語意用途 | 填充 | 描邊 |
|------------------|------|--------|
| 主要/中性 | `#{accent}` | `#{accent_dark}` |
| 次要 | `#{accent_secondary}` | `#{accent_dark}` |
| 開始/觸發 | `#FED7AA` | `#C2410C` |
| 結束/成功 | `#A7F3D0` | `#047857` |
| 警告/決策 | `#FEF3C7` | `#B45309` |
| 錯誤/停止 | `#FECACA` | `#B91C1C` |

### 圖表規則

- **背景：** {diagram_bg} 以獲得最佳可讀性
- **文字顏色：** {diagram_text}
- **描邊寬度：** 2px
- **規則：** 始終將較深的描邊與較淺的填充配對

---

## 快速參考

### 何時使用主要強調色

| 使用 {accent_name} | 不要使用 |
|-------------------|-----------|
{accent_usage_table}

### 字型使用

| {heading_font} | {body_font} |
|----------------|-------------|
{font_usage_table}

---

*最後更新：{date}*
```

---

## 填寫範本

### brand_tagline
一行品牌描述。
範例："AI 教育內容、社群和產品的完整設計系統。"

### principle_1/2/3_name 和 description
核心設計原則。
範例：
- "技術但平易近人" - "複雜主題變得可理解，但不會過度簡化。"
- "簡潔勝過花俏" - "沒有不必要的裝飾。每個視覺元素都有目的。"
- "預設深色模式" - "現代、開發者導向的美學。"

### logo_description
Logo 的簡短描述。
範例："一個風格化的藍色火焰/水滴形狀——流暢、動態且現代。"

如果沒有 Logo，寫："未設定 Logo。有可用時將 Logo 加到 `assets/logo.png`。"

### accent_name
主要強調色的名稱。
範例："Dynamous 藍"、"品牌橘"、"主色"

### accent/accent_dark/accent_secondary
不含 # 前綴的十六進位值。

### accent_usage_rule
如何使用強調色。
範例："Dynamous 藍是主角。用於動作和強調。不要過度使用以免稀釋效果。"

### theme_name
依據背景色選擇「深色」或「淺色」。

### heading_description/body_description/code_description
每個字型角色的簡短描述。
範例："乾淨、現代的無襯線字體，用於所有 UI 和內文"

### signature_elements_description
標誌性視覺元素的介紹。
範例："這些模式定義了 Dynamous 的視覺識別。"

### signature_element_1/2/3
獨特視覺模式的描述。
範例：
- 帶背景模糊的玻璃卡片效果
- 漸層背景
- 主要元素周圍的發光效果

如果未定義標誌性元素，根據顏色建議：
- 深色主題：玻璃效果、微妙的發光
- 淺色主題：乾淨的陰影、清晰的邊框

### diagram_bg
圖表的背景色。
深色品牌通常使用白色（#FFFFFF）作為圖表背景。
淺色品牌可以匹配品牌背景。

### diagram_text
圖表的文字顏色。
通常使用深色以確保可讀性。

### accent_usage_table
顯示何時使用/不使用強調色的表格行。

### font_usage_table
顯示何時使用每個字型的表格行。

### date
當前日期，格式為 YYYY-MM-DD。
