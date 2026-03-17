---
name: get-audio-duration
description: Getting the duration of an audio file in seconds with Mediabunny
metadata:
  tags: duration, audio, length, time, seconds, mp3, wav
---

# 使用 Mediabunny 取得音訊時長

Mediabunny 可以擷取音訊檔案的時長。適用於瀏覽器、Node.js 和 Bun 環境。

## 取得音訊時長

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const getAudioDuration = async (src: string) => {
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

## 用法

```tsx
const duration = await getAudioDuration("https://remotion.media/audio.mp3");
console.log(duration); // 例如 180.5（秒）
```

## 搭配本地檔案使用

對於本地檔案，使用 `FileSource` 取代 `UrlSource`：

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

const duration = await getAudioDuration(staticFile("audio.mp3"));
```
