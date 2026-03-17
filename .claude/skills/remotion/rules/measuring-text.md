---
name: measuring-text
description: Measuring text dimensions, fitting text to containers, and checking overflow
metadata:
  tags: measure, text, layout, dimensions, fitText, fillTextBox
---

# 在 Remotion 中測量文字

## 前置條件

如果尚未安裝 @remotion/layout-utils，請先安裝：

```bash
npx remotion add @remotion/layout-utils # 如果專案使用 npm
bunx remotion add @remotion/layout-utils # 如果專案使用 bun
yarn remotion add @remotion/layout-utils # 如果專案使用 yarn
pnpm exec remotion add @remotion/layout-utils # 如果專案使用 pnpm
```

## 測量文字尺寸

使用 `measureText()` 來計算文字的寬度和高度：

```tsx
import { measureText } from "@remotion/layout-utils";

const { width, height } = measureText({
  text: "Hello World",
  fontFamily: "Arial",
  fontSize: 32,
  fontWeight: "bold",
});
```

結果會被快取 - 重複呼叫會回傳快取的結果。

## 將文字適應指定寬度

使用 `fitText()` 來找到容器的最佳字型大小：

```tsx
import { fitText } from "@remotion/layout-utils";

const { fontSize } = fitText({
  text: "Hello World",
  withinWidth: 600,
  fontFamily: "Inter",
  fontWeight: "bold",
});

return (
  <div
    style={{
      fontSize: Math.min(fontSize, 80), // 上限為 80px
      fontFamily: "Inter",
      fontWeight: "bold",
    }}
  >
    Hello World
  </div>
);
```

## 檢查文字溢出

使用 `fillTextBox()` 來檢查文字是否超出方框：

```tsx
import { fillTextBox } from "@remotion/layout-utils";

const box = fillTextBox({ maxBoxWidth: 400, maxLines: 3 });

const words = ["Hello", "World", "This", "is", "a", "test"];
for (const word of words) {
  const { exceedsBox } = box.add({
    text: word + " ",
    fontFamily: "Arial",
    fontSize: 24,
  });
  if (exceedsBox) {
    // 文字會溢出，相應處理
    break;
  }
}
```

## 最佳實踐

**先載入字型：** 只在字型載入完成後才呼叫測量函式。

```tsx
import { loadFont } from "@remotion/google-fonts/Inter";

const { fontFamily, waitUntilDone } = loadFont("normal", {
  weights: ["400"],
  subsets: ["latin"],
});

waitUntilDone().then(() => {
  // 現在可以安全地測量
  const { width } = measureText({
    text: "Hello",
    fontFamily,
    fontSize: 32,
  });
})
```

**使用 validateFontIsLoaded：** 及早發現字型載入問題：

```tsx
measureText({
  text: "Hello",
  fontFamily: "MyCustomFont",
  fontSize: 32,
  validateFontIsLoaded: true, // 如果字型未載入則拋出錯誤
});
```

**匹配字型屬性：** 測量和渲染使用相同的屬性：

```tsx
const fontStyle = {
  fontFamily: "Inter",
  fontSize: 32,
  fontWeight: "bold" as const,
  letterSpacing: "0.5px",
};

const { width } = measureText({
  text: "Hello",
  ...fontStyle,
});

return <div style={fontStyle}>Hello</div>;
```

**避免 padding 和 border：** 使用 `outline` 代替 `border` 以防止佈局差異：

```tsx
<div style={{ outline: "2px solid red" }}>Text</div>
```
