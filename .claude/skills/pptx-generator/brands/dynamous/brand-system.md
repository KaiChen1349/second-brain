# Dynamous — 品牌系統

> AI 教育內容、社群和產品的完整設計系統。

---

## 品牌哲學

### 核心原則

1. **技術但親切**
   複雜的 AI 主題以易懂方式呈現，但不會過度簡化。展示精密度，清楚解釋。

2. **簡潔勝於花俏**
   沒有不必要的裝飾。每個視覺元素都有其目的。透過節制達成優雅。

3. **預設深色模式**
   現代、以開發者為導向的美學。對整天盯著螢幕的人來說更護眼。

4. **圖表應該論述，而非展示**
   視覺內容要傳達觀點。形狀反映意義。如果你移除文字，結構本身就應該能傳達概念。

---

## Logo

### Dynamous 標誌

一個風格化的藍色火焰/水滴形狀 — 流動、動態且現代。代表能量、轉化和前進動力。

**Logo 檔案**：`.claude/DynamousLogo.png`

**使用規則：**
- 維持原始比例
- 最小淨空間：Logo 寬度的 20%（四周）
- 深色背景上：直接使用
- 淺色背景上：直接使用（藍色配白色可行）
- 不要扭曲、旋轉或加特效

**Logo 顏色：**
- 主要填充：`#3B82F6`（Dynamous 藍）
- 高光漸層：朝左上方較淺的藍色
- 外框：`#2563EB`（稍深）

---

## 顏色系統

### 主要強調色 — Dynamous 藍

品牌標誌性顏色。用於主要操作、連結、高亮和 Logo。

| 名稱 | Hex | HSL | 用途 |
|------|-----|-----|------|
| Dynamous 藍 | `#3B82F6` | 217° 91% 60% | 主要按鈕、連結、Logo |
| Dynamous 深 | `#2563EB` | 217° 91% 53% | 懸停狀態、外框 |
| Dynamous 淺 | `#60A5FA` | 217° 91% 67% | 次要強調、高亮 |
| Dynamous 青 | `#0EA5E9` | 199° 89% 48% | 第三強調、變化 |
| Dynamous 淡 | `#DBEAFE` | 214° 95% 93% | 淺色模式背景、填充 |

**規則：** Dynamous 藍是主角。用於操作和強調。不要過度使用以避免稀釋效果。

**重要：禁止紫色。** Dynamous 只使用藍色變體。絕對不要使用紫色（#6D28D9 或類似色）。對於第三強調或 AI/LLM 表示，使用 Dynamous 青（#0EA5E9）或更深的藍色（#1E40AF）替代。

### 次要強調色 — 功能性顏色

謹慎使用於特定語義目的。

| 用途 | 填充 | 描邊 | 使用時機 |
|------|------|------|----------|
| AI/LLM | `#DBEAFE` | `#0EA5E9` | 在圖表中表示 AI、代理、LLM |
| 成功/開始 | `#A7F3D0` | `#047857` | 完成狀態、起點、公告 |
| 警告/決策 | `#FEF3C7` | `#B45309` | 決策點、注意、評分 |
| 錯誤/停止 | `#FECACA` | `#B91C1C` | 錯誤、破壞性操作、結束狀態 |
| 觸發/輸入 | `#FED7AA` | `#C2410C` | 輸入點、工作流程中的觸發器 |

**規則：** 這些是語義性的，不是裝飾性的。用它們在圖表和 UI 狀態中編碼含義。

### 主題基底

**深色主題（預設）**
```
背景：          #07090F（帶藍色調的近黑色）
替代背景：      #080A13（稍暖）
表面：          rgba(0, 0, 0, 0.4)（玻璃卡片）
邊框：          rgba(255, 255, 255, 0.1)
主要文字：      rgba(255, 255, 255, 0.98)
次要文字：      rgba(255, 255, 255, 0.80)
淡化文字：      rgba(255, 255, 255, 0.70)
```

**淺色主題（圖表/Excalidraw）**
```
背景：          #FFFFFF
表面：          #F1F5F9 (slate-100)
邊框：          #E5E7EB (gray-200)
主要文字：      #1E293B (slate-800)
次要文字：      #374151 (gray-700)
淡化文字：      #64748B (slate-500)
```

### 漸層背景

**主視覺漸層（網站）：**
```css
background: linear-gradient(to bottom, #07090f, rgba(30, 58, 138, 0.9), #080a13);
background-attachment: fixed;
```

**藍色光暈效果：**
```css
box-shadow: 0 0 15px rgba(59, 130, 246, 0.5), 0 0 30px rgba(59, 130, 246, 0.3);
```

---

## 字型排版

### 字型組合

- **主要字型：** Inter — 乾淨、現代的無襯線字型，用於所有 UI 和內文
- **縮圖：** Montserrat Bold 或 Inter Black — 高衝擊力，小尺寸也清晰可讀
- **程式碼：** JetBrains Mono 或系統等寬字型

### 字型大小比例

| 代號 | 大小 | 用途 |
|------|------|------|
| `xs` | 0.75rem (12px) | 標籤、徽章、後設資料 |
| `sm` | 0.875rem (14px) | 次要文字、描述 |
| `base` | 1rem (16px) | 內文、段落 |
| `lg` | 1.125rem (18px) | 引導段落、強調 |
| `xl` | 1.25rem (20px) | 章節標題 |
| `2xl` | 1.5rem (24px) | 卡片標題 |
| `3xl` | 1.875rem (30px) | 頁面標題 |
| `4xl` | 2.25rem (36px) | 主視覺副標題 |
| `5xl` | 3rem (48px) | 主視覺標題（手機） |
| `6xl` | 3.75rem (60px) | 主視覺標題（桌面） |

### 字型排版規則

| 元素 | 字型 | 字重 | 大小 | 行高 |
|------|------|------|------|------|
| 展示/主視覺 | Inter | 700 (Bold) | 4xl-6xl | 1.1 |
| H1 | Inter | 700 | 3xl | 1.2 |
| H2 | Inter | 600 | 2xl | 1.3 |
| H3 | Inter | 600 | xl | 1.4 |
| 內文 | Inter | 400 | base | 1.6 |
| 小字 | Inter | 400 | sm | 1.5 |
| 程式碼 | JetBrains Mono | 400 | sm | 1.5 |
| 按鈕 | Inter | 500 | sm-base | 1 |
| 縮圖文字 | Montserrat | 800 | - | 1.1 |

### 文字顏色使用

| 情境 | 顏色 | 備註 |
|------|------|------|
| 標題 | white (98%) | 完全對比以強調 |
| 內文 | white/80 | 舒適閱讀 |
| 描述 | white/70 | 次要資訊 |
| 淡化/後設 | white/60 | 時間戳、標籤 |
| 連結 | Dynamous 藍 | `#3B82F6` 帶懸停光暈 |

---

## 間距系統

**基礎單位：** 4px

| 代號 | 值 | 用途 |
|------|------|------|
| `xs` | 4px | 緊湊間距、圖示間隙 |
| `sm` | 8px | 相關元素 |
| `md` | 12px | 表單輸入內部 |
| `base` | 16px | 標準元件內距 |
| `lg` | 24px | 區段內距 |
| `xl` | 32px | 卡片內距 |
| `2xl` | 48px | 區段外距 |
| `3xl` | 64px | 主要區段間隔 |

**規則：** 不確定時，使用 8px 的倍數。

---

## 按鈕

### 按鈕類型

| 類型 | 背景 | 文字 | 用途 |
|------|------|------|------|
| 主要 | Dynamous 藍 | 白色 | 主要操作、CTA |
| 次要 | white/10 | 白色 | 次要操作 |
| 外框 | 透明 | 白色 | 第三層級，搭配主要按鈕 |
| 幽靈 | 透明 | 白色 | 導覽、低調操作 |

### 按鈕規格

- **字型：** Inter, 500 字重
- **圓角：** 8px (0.5rem)
- **內距：**
  - sm: 6px 12px
  - md: 8px 16px
  - lg: 12px 24px
- **過渡：** 200ms ease-out
- **主要陰影：** `0 4px 14px -4px rgba(59, 130, 246, 0.4)`

### 標誌性按鈕效果

懸停時的微光動畫：
```css
.shimmer {
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
  animation: shimmer 2s linear infinite;
}
```

---

## 動效

### 時間設定

| 類型 | 持續時間 | 用途 |
|------|----------|------|
| 微型 | 150ms | 按鈕狀態、切換 |
| 快速 | 200ms | 下拉選單、小型過渡 |
| 正常 | 300ms | 卡片、彈窗 |
| 緩慢 | 600ms | 頁面轉場、淡入 |

**規則：** 任何動效都不應超過 600ms。俐落 > 滑順。

### 緩動

- **進場：** `ease-out` 或 `cubic-bezier(0, 0, 0.2, 1)`
- **離場：** `ease-in` 或 `cubic-bezier(0.4, 0, 1, 1)`
- **絕不使用：** Linear 用於 UI 過渡（感覺機械化）

### 標誌性動畫

**浮動（裝飾元素）：**
```css
@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}
animation: float 6s ease-in-out infinite;
```

**淡入（內容進場）：**
```css
@keyframes fade-in {
  0% { opacity: 0; transform: translateY(10px); }
  100% { opacity: 1; transform: translateY(0); }
}
animation: fade-in 0.6s ease-out forwards;
```

**光暈脈動（強調）：**
```css
@keyframes glow {
  0%, 100% { box-shadow: 0 0 5px rgba(59, 130, 246, 0.5); }
  50% { box-shadow: 0 0 20px rgba(59, 130, 246, 0.8); }
}
```

### 會動的元素

- 按鈕（懸停、點擊狀態）
- 卡片（懸停上浮）
- 裝飾性 Logo（浮動）
- 頁面區段（滾動顯現）
- 彈窗（進場/離場）

### 不動的元素

- 內文
- 導覽列（保持穩固）
- 表單輸入（穩定）
- 關鍵 UI 元素

---

## 圖示

### 規格

| 屬性 | 值 |
|------|------|
| 圖示庫 | Lucide React |
| 樣式 | 線條、2px 描邊 |
| 大小（行內） | 14-16px |
| 大小（UI） | 18-20px |
| 大小（強調） | 24px |
| 預設顏色 | currentColor（繼承文字色） |
| 互動 | 懸停時改變透明度 |

### 使用規則

- 圖示配合文字使用，很少單獨出現
- 在元件內使用一致的大小
- 圖示粗細與周圍文字粗細匹配
- 不要純粹為了裝飾而使用圖示

---

## 標誌性元素

### 玻璃卡片

Dynamous 的定義性 UI 模式。深色背景上的毛玻璃效果。

**適用於：**
- 內容卡片
- 影片容器
- 功能亮點
- 推薦見證

**不適用於：**
- 按鈕（太微妙）
- 導覽列
- 表單輸入

**實作：**
```css
.glass-card {
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 12px;
}
```

### 藍色光暈

標誌性強調效果。在重要元素周圍創造柔和光環。

**適用於：**
- 主視覺文字高亮
- 主要按鈕
- 精選內容
- 主視覺區段的 Logo

**規則：** 謹慎使用。如果所有東西都發光，就什麼都不突出了。

**實作：**
```css
.blue-glow {
  box-shadow: 0 0 15px rgba(59, 130, 246, 0.5),
              0 0 30px rgba(59, 130, 246, 0.3);
}

.text-glow {
  text-shadow: 0 0 10px rgba(59, 130, 246, 0.5),
               0 0 20px rgba(59, 130, 246, 0.3);
}
```

### 格線圖案

微妙的背景紋理，增加深度但不分散注意力。

**僅用於：** 主視覺區段、大面積空白背景

**實作：**
```css
.grid-pattern {
  background-image:
    linear-gradient(to right, rgba(99, 102, 241, 0.05) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(99, 102, 241, 0.05) 1px, transparent 1px);
  background-size: 40px 40px;
}
```

---

## 圖表（Excalidraw）

### 圖表色彩調色盤

| 語義用途 | 填充 | 描邊 |
|----------|------|------|
| 主要/中性 | `#3B82F6` | `#1E3A5F` |
| 次要 | `#60A5FA` | `#1E3A5F` |
| 第三/AI | `#DBEAFE` | `#0EA5E9` |
| 開始/觸發 | `#FED7AA` | `#C2410C` |
| 結束/成功 | `#A7F3D0` | `#047857` |
| 警告/重置 | `#FEE2E2` | `#DC2626` |
| 決策 | `#FEF3C7` | `#B45309` |

### 圖表規則

- **背景：** 白色（`#FFFFFF`）以獲得最大可讀性
- **文字顏色：** `#1E3A5F`（深藍色）
- **字型：** Virgil（Excalidraw 預設）或手繪風格
- **描邊寬度：** 2px
- **粗糙度：** 1（輕微手繪感）
- **規則：** 始終將較深的描邊與較淺的填充配對

### 形狀含義

| 概念類型 | 形狀 |
|----------|------|
| 開始、觸發、輸入 | 橢圓 |
| 結束、輸出、結果 | 橢圓 |
| 決策、條件 | 菱形 |
| 處理、動作、步驟 | 矩形 |
| 抽象狀態、上下文 | 重疊橢圓（雲朵） |

---

## YouTube 縮圖

### 規格

- **尺寸：** 1280 × 720px
- **文字：** 最多 2-5 個字
- **字型：** Montserrat Extra Bold (800) 或 Inter Black
- **文字顏色：** 白色帶深色外框/陰影以確保可讀性
- **背景：** 深色帶藍色點綴，或基於截圖

### 常見模式

1. **GitHub 儲存庫風格** — 截圖配 Logo「swoosh」
2. **Excalidraw 圖表** — 工作流程視覺化
3. **你 + Logo** — 個人照片周圍環繞科技 Logo
4. **粗體文字 + 概念** — 大字搭配輔助視覺

### 縮圖規則

- 盡可能包含人臉（提高點擊率）
- 高對比文字（小尺寸也可讀）
- 畫面中某處有一致的藍色點綴
- 文字不超過 5 個字
- 在手機大小（小縮圖預覽）測試可讀性

---

## 圖片處理

### 截圖與 UI

- **圓角：** 12px
- **邊框：** 1px solid rgba(255, 255, 255, 0.1)
- **陰影：** `0 4px 12px -2px rgba(0, 0, 0, 0.3)`
- **選配：** 精選截圖加藍色光暈

### 照片

- **圓角：** 8px 或完整圓形（用於頭像）
- **濾鏡：** 輕微冷色調以配合藍色調色盤
- **背景：** 偏好深色，背景雜亂時模糊處理

---

## CSS 變數參考

```css
:root {
  /* 顏色 - 主要 */
  --dynamous-blue: #3B82F6;
  --dynamous-dark: #2563EB;
  --dynamous-light: #60A5FA;
  --dynamous-cyan: #0EA5E9;
  --dynamous-pale: #DBEAFE;

  /* 顏色 - 語義 */
  --success: #047857;
  --success-bg: #A7F3D0;
  --warning: #B45309;
  --warning-bg: #FEF3C7;
  --error: #B91C1C;
  --error-bg: #FECACA;
  --ai-cyan: #0EA5E9;
  --ai-cyan-bg: #DBEAFE;

  /* 顏色 - 主題（深色） */
  --background: #07090F;
  --background-alt: #080A13;
  --surface: rgba(0, 0, 0, 0.4);
  --border: rgba(255, 255, 255, 0.1);
  --text-primary: rgba(255, 255, 255, 0.98);
  --text-secondary: rgba(255, 255, 255, 0.8);
  --text-muted: rgba(255, 255, 255, 0.7);

  /* 間距 */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 12px;
  --space-base: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;

  /* 字型排版 */
  --font-sans: 'Inter', ui-sans-serif, system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', ui-monospace, monospace;
  --font-display: 'Montserrat', var(--font-sans);

  /* 動效 */
  --duration-micro: 150ms;
  --duration-fast: 200ms;
  --duration-normal: 300ms;
  --duration-slow: 600ms;
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in: cubic-bezier(0.4, 0, 1, 1);

  /* 圓角 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-full: 9999px;
}
```

---

## 快速參考

### Dynamous 藍 vs 其他藍色

| Dynamous 藍 | 其他藍色 |
|-------------|----------|
| 主要按鈕 | 絕不 |
| 連結 | - |
| Logo | - |
| 強調光暈 | 次要強調使用語義顏色 |
| 主視覺高亮 | 圖表填充使用調色盤系統 |
| 精選元素 | - |

### Inter vs Montserrat

| Inter | Montserrat |
|-------|------------|
| 所有 UI 文字 | YouTube 縮圖文字 |
| 內文 | 粗體標題（選用） |
| 按鈕 | - |
| 導覽 | - |
| 其他所有場合 | 只在需要最大衝擊力時使用 |

---

*最後更新：2026-01-13*
