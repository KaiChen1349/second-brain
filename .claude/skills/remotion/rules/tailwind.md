---
name: tailwind
description: Using TailwindCSS in Remotion.
metadata:
---

如果專案中已安裝 TailwindCSS，你可以也應該在 Remotion 中使用它。

不要使用 `transition-*` 或 `animate-*` class - 始終使用 `useCurrentFrame()` hook 來製作動畫。

在 Remotion 專案中使用 Tailwind 前必須先安裝並啟用 - 請透過 WebFetch 獲取 https://www.remotion.dev/docs/tailwind 的安裝說明。
