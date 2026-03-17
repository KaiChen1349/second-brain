---
name: gif
description: Displaying GIFs, APNG, AVIF and WebP in Remotion
metadata:
  tags: gif, animation, images, animated, apng, avif, webp
---

# 在 Remotion 中使用動態圖片

## 基本用法

使用 `<AnimatedImage>` 來顯示與 Remotion 時間軸同步的 GIF、APNG、AVIF 或 WebP 圖片：

```tsx
import {AnimatedImage, staticFile} from 'remotion';

export const MyComposition = () => {
  return <AnimatedImage src={staticFile('animation.gif')} width={500} height={500} />;
};
```

也支援遠端 URL（必須啟用 CORS）：

```tsx
<AnimatedImage src="https://example.com/animation.gif" width={500} height={500} />
```

## 尺寸和適應方式

使用 `fit` 屬性控制圖片如何填滿容器：

```tsx
// 拉伸填滿（預設）
<AnimatedImage src={staticFile("animation.gif")} width={500} height={300} fit="fill" />

// 保持長寬比，適應容器內部
<AnimatedImage src={staticFile("animation.gif")} width={500} height={300} fit="contain" />

// 填滿容器，必要時裁剪
<AnimatedImage src={staticFile("animation.gif")} width={500} height={300} fit="cover" />
```

## 播放速度

使用 `playbackRate` 來控制動畫速度：

```tsx
<AnimatedImage src={staticFile("animation.gif")} width={500} height={500} playbackRate={2} /> {/* 2 倍速 */}
<AnimatedImage src={staticFile("animation.gif")} width={500} height={500} playbackRate={0.5} /> {/* 半速 */}
```

## 循環行為

控制動畫結束後的行為：

```tsx
// 無限循環（預設）
<AnimatedImage src={staticFile("animation.gif")} width={500} height={500} loopBehavior="loop" />

// 播放一次，顯示最後一幀
<AnimatedImage src={staticFile("animation.gif")} width={500} height={500} loopBehavior="pause-after-finish" />

// 播放一次，然後清除畫布
<AnimatedImage src={staticFile("animation.gif")} width={500} height={500} loopBehavior="clear-after-finish" />
```

## 樣式

使用 `style` 屬性設定額外的 CSS（使用 `width` 和 `height` 屬性設定尺寸）：

```tsx
<AnimatedImage
  src={staticFile('animation.gif')}
  width={500}
  height={500}
  style={{
    borderRadius: 20,
    position: 'absolute',
    top: 100,
    left: 50,
  }}
/>
```

## 取得 GIF 時長

使用 `@remotion/gif` 的 `getGifDurationInSeconds()` 來取得 GIF 的時長。

```bash
npx remotion add @remotion/gif # 如果專案使用 npm
bunx remotion add @remotion/gif # 如果專案使用 bun
yarn remotion add @remotion/gif # 如果專案使用 yarn
pnpm exec remotion add @remotion/gif # 如果專案使用 pnpm
```

```tsx
import {getGifDurationInSeconds} from '@remotion/gif';
import {staticFile} from 'remotion';

const duration = await getGifDurationInSeconds(staticFile('animation.gif'));
console.log(duration); // 例如 2.5
```

這對於設定合成時長以匹配 GIF 很有用：

```tsx
import {getGifDurationInSeconds} from '@remotion/gif';
import {staticFile, CalculateMetadataFunction} from 'remotion';

const calculateMetadata: CalculateMetadataFunction = async () => {
  const duration = await getGifDurationInSeconds(staticFile('animation.gif'));
  return {
    durationInFrames: Math.ceil(duration * 30),
  };
};
```

## 替代方案

如果 `<AnimatedImage>` 無法使用（僅在 Chrome 和 Firefox 中支援），你可以改用 `@remotion/gif` 的 `<Gif>`。

```bash
npx remotion add @remotion/gif # 如果專案使用 npm
bunx remotion add @remotion/gif # 如果專案使用 bun
yarn remotion add @remotion/gif # 如果專案使用 yarn
pnpm exec remotion add @remotion/gif # 如果專案使用 pnpm
```

```tsx
import {Gif} from '@remotion/gif';
import {staticFile} from 'remotion';

export const MyComposition = () => {
  return <Gif src={staticFile('animation.gif')} width={500} height={500} />;
};
```

`<Gif>` 元件與 `<AnimatedImage>` 具有相同的屬性，但僅支援 GIF 檔案。
