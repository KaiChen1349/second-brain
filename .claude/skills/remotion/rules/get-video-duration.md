---
name: get-video-duration
description: Getting the duration of a video file in seconds with Mediabunny
metadata:
  tags: duration, video, length, time, seconds
---

# 使用 Mediabunny 取得影片時長

Mediabunny 可以提取影片檔案的時長。它在瀏覽器、Node.js 和 Bun 環境中都可運作。

## 取得影片時長

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const getVideoDuration = async (src: string) => {
  const input = new Input({
    formats: ALL_FORMATS,
    source: new UrlSource(src, {
      getRetryDelay: () => null,
    }),
  });

  const durationInSeconds = await input.computeDuration();
  return durationInSeconds;
};
```

## 使用方式

```tsx
const duration = await getVideoDuration("https://remotion.media/video.mp4");
console.log(duration); // 例如 10.5（秒）
```

## 搭配本機檔案使用

對於本機檔案，使用 `FileSource` 而非 `UrlSource`：

```tsx
import { Input, ALL_FORMATS, FileSource } from "mediabunny";

const input = new Input({
  formats: ALL_FORMATS,
  source: new FileSource(file), // 來自 input 或拖放的 File 物件
});

const durationInSeconds = await input.computeDuration();
```

## 在 Remotion 中搭配 staticFile 使用

```tsx
import { staticFile } from "remotion";

const duration = await getVideoDuration(staticFile("video.mp4"));
```
