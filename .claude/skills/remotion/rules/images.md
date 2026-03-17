---
name: images
description: Embedding images in Remotion using the <Img> component
metadata:
  tags: images, img, staticFile, png, jpg, svg, webp
---

# 在 Remotion 中使用圖片

## `<Img>` 元件

始終使用 `remotion` 的 `<Img>` 元件來顯示圖片：

```tsx
import { Img, staticFile } from "remotion";

export const MyComposition = () => {
  return <Img src={staticFile("photo.png")} />;
};
```

## 重要限制

**你必須使用 `remotion` 的 `<Img>` 元件。** 不要使用：

- 原生 HTML `<img>` 元素
- Next.js `<Image>` 元件
- CSS `background-image`

`<Img>` 元件確保圖片在渲染前完全載入，防止影片匯出時出現閃爍和空白影格。

## 使用 staticFile() 載入本機圖片

將圖片放在 `public/` 資料夾中，並使用 `staticFile()` 來引用它們：

```
my-video/
├─ public/
│  ├─ logo.png
│  ├─ avatar.jpg
│  └─ icon.svg
├─ src/
├─ package.json
```

```tsx
import { Img, staticFile } from "remotion";

<Img src={staticFile("logo.png")} />
```

## 遠端圖片

遠端 URL 可以直接使用，不需要 `staticFile()`：

```tsx
<Img src="https://example.com/image.png" />
```

確保遠端圖片已啟用 CORS。

對於動態 GIF，請改用 `@remotion/gif` 的 `<Gif>` 元件。

## 尺寸和定位

使用 `style` 屬性來控制大小和位置：

```tsx
<Img
  src={staticFile("photo.png")}
  style={{
    width: 500,
    height: 300,
    position: "absolute",
    top: 100,
    left: 50,
    objectFit: "cover",
  }}
/>
```

## 動態圖片路徑

使用模板字串來動態引用檔案：

```tsx
import { Img, staticFile, useCurrentFrame } from "remotion";

const frame = useCurrentFrame();

// 圖片序列
<Img src={staticFile(`frames/frame${frame}.png`)} />

// 根據屬性選擇
<Img src={staticFile(`avatars/${props.userId}.png`)} />

// 條件式圖片
<Img src={staticFile(`icons/${isActive ? "active" : "inactive"}.svg`)} />
```

此模式適用於：

- 圖片序列（逐幀動畫）
- 使用者專屬的頭像或個人照片
- 主題式圖示
- 狀態相關的圖形

## 取得圖片尺寸

使用 `getImageDimensions()` 來取得圖片的尺寸：

```tsx
import { getImageDimensions, staticFile } from "remotion";

const { width, height } = await getImageDimensions(staticFile("photo.png"));
```

這對於計算長寬比或設定合成大小很有用：

```tsx
import { getImageDimensions, staticFile, CalculateMetadataFunction } from "remotion";

const calculateMetadata: CalculateMetadataFunction = async () => {
  const { width, height } = await getImageDimensions(staticFile("photo.png"));
  return {
    width,
    height,
  };
};
```
