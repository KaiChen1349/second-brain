---
name: can-decode
description: Check if a video can be decoded by the browser using Mediabunny
metadata:
  tags: decode, validation, video, audio, compatibility, browser
---

# 檢查影片是否可解碼

使用 Mediabunny 在嘗試播放影片之前，檢查瀏覽器是否能解碼該影片。

## `canDecode()` 函式

此函式可以直接複製貼上到任何專案中。

```tsx
import { Input, ALL_FORMATS, UrlSource } from "mediabunny";

export const canDecode = async (src: string) => {
  const input = new Input({
    formats: ALL_FORMATS,
    source: new UrlSource(src, {
      getRetryDelay: () => null,
    }),
  });

  try {
    await input.getFormat();
  } catch {
    return false;
  }

  const videoTrack = await input.getPrimaryVideoTrack();
  if (videoTrack && !(await videoTrack.canDecode())) {
    return false;
  }

  const audioTrack = await input.getPrimaryAudioTrack();
  if (audioTrack && !(await audioTrack.canDecode())) {
    return false;
  }

  return true;
};
```

## 使用方式

```tsx
const src = "https://remotion.media/video.mp4";
const isDecodable = await canDecode(src);

if (isDecodable) {
  console.log("影片可以解碼");
} else {
  console.log("此瀏覽器無法解碼該影片");
}
```

## 搭配 Blob 使用

對於檔案上傳或拖放功能，使用 `BlobSource`：

```tsx
import { Input, ALL_FORMATS, BlobSource } from "mediabunny";

export const canDecodeBlob = async (blob: Blob) => {
  const input = new Input({
    formats: ALL_FORMATS,
    source: new BlobSource(blob),
  });

  // 與上方相同的驗證邏輯
};
```
