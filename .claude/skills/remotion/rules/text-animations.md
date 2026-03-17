---
name: text-animations
description: Typography and text animation patterns for Remotion.
metadata:
  tags: typography, text, typewriter, highlighter ken
---

## 文字動畫

基於 `useCurrentFrame()`，逐字元減少字串以建立打字機效果。

## 打字機效果

參見 [Typewriter](assets/text-animations-typewriter.tsx) 了解帶有閃爍游標和第一句話後暫停的進階範例。

始終使用字串切片來實現打字機效果。永遠不要使用逐字元透明度。

## 文字高亮

參見 [Word Highlight](assets/text-animations-word-highlight.tsx) 了解文字高亮動畫的範例，像螢光筆一樣的效果。
