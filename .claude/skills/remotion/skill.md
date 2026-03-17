---
name: remotion-best-practices
description: |
  Create and edit Remotion videos with domain-specific knowledge.
  TRIGGERS - Use this skill when:
  - User mentions "Remotion" in their request
  - User references a Remotion project path (remotion-videos/*, contains remotion.config.ts)
  - User asks to create, animate, or edit video content in a Remotion context
  - User wants to build compositions, animations, or video sequences in React
  Always invoke this skill BEFORE writing any Remotion code.
metadata:
  tags: remotion, video, react, animation, composition
---

## 使用時機

在處理 Remotion 專案時使用此技能。包括：
- 建立新的影片合成或動畫
- 編輯現有的 Remotion 元件
- 將素材（圖片、音訊、影片）加入 Remotion 專案
- 建構動畫序列、轉場或特效
- 任何涉及 Remotion 專案目錄的工作

**關鍵觸發詞：** 「Remotion」一詞、包含 `remotion-videos/` 的路徑、`remotion.config.ts` 等檔案，或在 React 中建立動畫影片內容的請求。

## 使用方式

閱讀各個規則檔案以獲取詳細說明和程式碼範例：

- [rules/3d.md](rules/3d.md) - 在 Remotion 中使用 Three.js 和 React Three Fiber 的 3D 內容
- [rules/animations.md](rules/animations.md) - Remotion 的基礎動畫技巧
- [rules/assets.md](rules/assets.md) - 在 Remotion 中匯入圖片、影片、音訊和字型
- [rules/audio.md](rules/audio.md) - 在 Remotion 中使用音訊和聲音 - 匯入、裁剪、音量、速度、音高
- [rules/calculate-metadata.md](rules/calculate-metadata.md) - 動態設定合成的時長、尺寸和屬性
- [rules/can-decode.md](rules/can-decode.md) - 使用 Mediabunny 檢查影片是否能被瀏覽器解碼
- [rules/charts.md](rules/charts.md) - Remotion 的圖表和資料視覺化模式
- [rules/compositions.md](rules/compositions.md) - 定義合成、靜態圖片、資料夾、預設屬性和動態中繼資料
- [rules/display-captions.md](rules/display-captions.md) - 在 Remotion 中顯示字幕，支援 TikTok 風格分頁和文字高亮
- [rules/extract-frames.md](rules/extract-frames.md) - 使用 Mediabunny 從影片中提取特定時間戳的影格
- [rules/fonts.md](rules/fonts.md) - 在 Remotion 中載入 Google Fonts 和本機字型
- [rules/get-audio-duration.md](rules/get-audio-duration.md) - 使用 Mediabunny 取得音訊檔案的時長（秒）
- [rules/get-video-dimensions.md](rules/get-video-dimensions.md) - 使用 Mediabunny 取得影片的寬度和高度
- [rules/get-video-duration.md](rules/get-video-duration.md) - 使用 Mediabunny 取得影片檔案的時長（秒）
- [rules/gifs.md](rules/gifs.md) - 顯示與 Remotion 時間軸同步的 GIF
- [rules/images.md](rules/images.md) - 使用 Img 元件在 Remotion 中嵌入圖片
- [rules/import-srt-captions.md](rules/import-srt-captions.md) - 使用 @remotion/captions 匯入 .srt 字幕檔
- [rules/lottie.md](rules/lottie.md) - 在 Remotion 中嵌入 Lottie 動畫
- [rules/measuring-dom-nodes.md](rules/measuring-dom-nodes.md) - 在 Remotion 中測量 DOM 元素尺寸
- [rules/measuring-text.md](rules/measuring-text.md) - 測量文字尺寸、將文字填入容器，以及檢查溢出
- [rules/sequencing.md](rules/sequencing.md) - Remotion 的序列模式 - 延遲、裁剪、限制項目時長
- [rules/tailwind.md](rules/tailwind.md) - 在 Remotion 中使用 TailwindCSS
- [rules/text-animations.md](rules/text-animations.md) - Remotion 的文字排版和文字動畫模式
- [rules/timing.md](rules/timing.md) - Remotion 中的插值曲線 - 線性、緩動、彈簧動畫
- [rules/transcribe-captions.md](rules/transcribe-captions.md) - 轉錄音訊以在 Remotion 中產生字幕
- [rules/transitions.md](rules/transitions.md) - Remotion 的場景轉場模式
- [rules/trimming.md](rules/trimming.md) - Remotion 的裁剪模式 - 裁剪動畫的開頭或結尾
- [rules/videos.md](rules/videos.md) - 在 Remotion 中嵌入影片 - 裁剪、音量、速度、循環、音高
- [rules/parameters.md](rules/parameters.md) - 透過新增 Zod schema 使影片可參數化
- [rules/maps.md](rules/maps.md) - 使用 Mapbox 加入地圖並製作動畫
