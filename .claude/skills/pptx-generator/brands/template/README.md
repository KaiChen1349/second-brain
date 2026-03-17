# 品牌範本

使用此範本為 PPTX 產生器技能新增品牌。

## 設定說明

1. **複製此資料夾**到以你品牌名稱命名的新資料夾：
   ```
   cp -r template/ your-brand-name/
   ```

2. **編輯 `brand.json`** 填入你的品牌值：
   - 所有色值應為不含 `#` 前綴的十六進位碼（例如 `"1e1e2e"`）
   - 字型名稱應與系統已安裝的字型匹配
   - 素材路徑相對於品牌資料夾

3. **編輯 `brand-system.md`** 填入你的品牌準則：
   - 此檔案為 AI 產生內容時提供上下文
   - 包含顏色理念、排版規則和視覺原則

4. **編輯 `tone-of-voice.md`** 填入撰寫指南：
   - 此檔案引導 AI 撰寫投影片內容
   - 包含語調屬性、範例用語和該做/不該做

5. **新增素材**到 `assets/` 資料夾：
   - `logo.png` - 主要 Logo
   - `logo-dark.png` - 深色背景用的 Logo 變體（選填）
   - `icon.png` - 方形圖示（選填）

6. **編輯 `config.json`** 填入輸出和產生設定：
   - 輸出目錄（PPTX 檔案存放位置）
   - 批次產生設定
   - 檔案命名慣例

## config.json 參考

```json
{
  "output": {
    "directory": "output/{brand}",  // 檔案存放位置（{brand} = 品牌名稱）
    "naming": "{name}-{date}",      // 檔案命名模式
    "keep_parts": false             // 合併後是否保留分段檔案？
  },
  "generation": {
    "slides_per_batch": 5,          // 每批次投影片數（最多 5）
    "auto_combine": true,           // 自動合併批次為最終檔案？
    "open_after_generate": false    // 產生後是否開啟檔案？
  },
  "defaults": {
    "slide_width_inches": 13.333,   // 16:9 長寬比
    "slide_height_inches": 7.5
  }
}
```

**`directory` 和 `naming` 的佔位符：**
- `{brand}` - 品牌資料夾名稱
- `{name}` - 簡報名稱（來自使用者請求）
- `{date}` - 當前日期（YYYY-MM-DD）
- `{timestamp}` - Unix 時間戳

## brand.json 參考

```json
{
  "name": "Brand Name",
  "description": "一行品牌描述",

  "colors": {
    "background": "1e1e2e",      // 主要投影片背景
    "background_alt": "181825",  // 較深的對比變體
    "text": "cdd6f4",            // 主要文字顏色
    "text_secondary": "bac2de",  // 次要/較淡文字
    "accent": "fab387",          // 主要強調色（行動呼籲、高亮）
    "accent_secondary": "89b4fa", // 次要強調色（多樣性）
    "accent_tertiary": "a6e3a1", // 第三強調色（選填）
    "code_bg": "11111b",         // 程式碼區塊背景
    "card_bg": "313244",         // 卡片/表面背景
    "card_bg_alt": "45475a"      // 替代卡片背景
  },

  "fonts": {
    "heading": "JetBrains Mono", // 大標題、標題、按鈕
    "body": "Inter",             // 內文、描述
    "code": "JetBrains Mono"     // 程式碼區塊、終端
  },

  "assets": {
    "logo": "assets/logo.png",   // 主要 Logo 路徑
    "logo_dark": null,           // 深色變體路徑（或 null）
    "icon": null                 // 圖示路徑（或 null）
  }
}
```

## 注意事項

- 色值的十六進位碼不應包含 `#` 前綴
- AI 會讀取 `brand.json`（用於精確值）和 markdown 檔案（用於上下文）
- 如果你變更了 `brand.json` 中的值，請同步更新 markdown 檔案
