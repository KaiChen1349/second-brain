---
name: fonts
description: Loading Google Fonts and local fonts in Remotion
metadata:
  tags: fonts, google-fonts, typography, text
---

# 在 Remotion 中使用字型

## 使用 @remotion/google-fonts 載入 Google Fonts

使用 Google Fonts 的推薦方式。具備型別安全，並自動阻擋渲染直到字型準備就緒。

### 前置條件

首先，需要安裝 @remotion/google-fonts 套件。
如果尚未安裝，請使用以下命令：

```bash
npx remotion add @remotion/google-fonts # 如果專案使用 npm
bunx remotion add @remotion/google-fonts # 如果專案使用 bun
yarn remotion add @remotion/google-fonts # 如果專案使用 yarn
pnpm exec remotion add @remotion/google-fonts # 如果專案使用 pnpm
```

```tsx
import { loadFont } from "@remotion/google-fonts/Lobster";

const { fontFamily } = loadFont();

export const MyComposition = () => {
  return <div style={{ fontFamily }}>Hello World</div>;
};
```

建議只指定需要的字重和子集以減少檔案大小：

```tsx
import { loadFont } from "@remotion/google-fonts/Roboto";

const { fontFamily } = loadFont("normal", {
  weights: ["400", "700"],
  subsets: ["latin"],
});
```

### 等待字型載入

如果你需要知道字型何時準備就緒，使用 `waitUntilDone()`：

```tsx
import { loadFont } from "@remotion/google-fonts/Lobster";

const { fontFamily, waitUntilDone } = loadFont();

await waitUntilDone();
```

## 使用 @remotion/fonts 載入本地字型

對於本地字型檔案，使用 `@remotion/fonts` 套件。

### 前置條件

首先，安裝 @remotion/fonts：

```bash
npx remotion add @remotion/fonts # 如果專案使用 npm
bunx remotion add @remotion/fonts # 如果專案使用 bun
yarn remotion add @remotion/fonts # 如果專案使用 yarn
pnpm exec remotion add @remotion/fonts # 如果專案使用 pnpm
```

### 載入本地字型

將字型檔案放在 `public/` 資料夾中，並使用 `loadFont()`：

```tsx
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

await loadFont({
  family: "MyFont",
  url: staticFile("MyFont-Regular.woff2"),
});

export const MyComposition = () => {
  return <div style={{ fontFamily: "MyFont" }}>Hello World</div>;
};
```

### 載入多種字重

使用相同的 family 名稱分別載入每種字重：

```tsx
import { loadFont } from "@remotion/fonts";
import { staticFile } from "remotion";

await Promise.all([
  loadFont({
    family: "Inter",
    url: staticFile("Inter-Regular.woff2"),
    weight: "400",
  }),
  loadFont({
    family: "Inter",
    url: staticFile("Inter-Bold.woff2"),
    weight: "700",
  }),
]);
```

### 可用選項

```tsx
loadFont({
  family: "MyFont", // 必要：CSS 中使用的名稱
  url: staticFile("font.woff2"), // 必要：字型檔案 URL
  format: "woff2", // 選填：從副檔名自動偵測
  weight: "400", // 選填：字重
  style: "normal", // 選填：normal 或 italic
  display: "block", // 選填：font-display 行為
});
```

## 在元件中使用

在元件的頂層或在早期匯入的單獨檔案中呼叫 `loadFont()`：

```tsx
import { loadFont } from "@remotion/google-fonts/Montserrat";

const { fontFamily } = loadFont("normal", {
  weights: ["400", "700"],
  subsets: ["latin"],
});

export const Title: React.FC<{ text: string }> = ({ text }) => {
  return (
    <h1
      style={{
        fontFamily,
        fontSize: 80,
        fontWeight: "bold",
      }}
    >
      {text}
    </h1>
  );
};
```
