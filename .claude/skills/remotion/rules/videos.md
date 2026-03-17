---
name: videos
description: Embedding videos in Remotion - trimming, volume, speed, looping, pitch
metadata:
  tags: video, media, trim, volume, speed, loop, pitch
---

# 在 Remotion 中使用影片

## 前置條件

首先，需要安裝 @remotion/media 套件。
如果尚未安裝，請使用以下命令：

```bash
npx remotion add @remotion/media # 如果專案使用 npm
bunx remotion add @remotion/media # 如果專案使用 bun
yarn remotion add @remotion/media # 如果專案使用 yarn
pnpm exec remotion add @remotion/media # 如果專案使用 pnpm
```

使用 `@remotion/media` 的 `<Video>` 來將影片嵌入你的合成。

```tsx
import { Video } from "@remotion/media";
import { staticFile } from "remotion";

export const MyComposition = () => {
  return <Video src={staticFile("video.mp4")} />;
};
```

也支援遠端 URL：

```tsx
<Video src="https://remotion.media/video.mp4" />
```

## 裁剪

使用 `trimBefore` 和 `trimAfter` 來移除影片的部分片段。值以秒為單位。

```tsx
const { fps } = useVideoConfig();

return (
  <Video
    src={staticFile("video.mp4")}
    trimBefore={2 * fps} // 跳過前 2 秒
    trimAfter={10 * fps} // 在第 10 秒處結束
  />
);
```

## 延遲播放

將影片包裹在 `<Sequence>` 中以延遲顯示時間：

```tsx
import { Sequence, staticFile } from "remotion";
import { Video } from "@remotion/media";

const { fps } = useVideoConfig();

return (
  <Sequence from={1 * fps}>
    <Video src={staticFile("video.mp4")} />
  </Sequence>
);
```

影片將在 1 秒後顯示。

## 大小和位置

使用 `style` 屬性來控制大小和位置：

```tsx
<Video
  src={staticFile("video.mp4")}
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

## 音量

設定靜態音量（0 到 1）：

```tsx
<Video src={staticFile("video.mp4")} volume={0.5} />
```

或使用回呼函式根據當前影格動態調整音量：

```tsx
import { interpolate } from "remotion";

const { fps } = useVideoConfig();

return (
  <Video
    src={staticFile("video.mp4")}
    volume={(f) =>
      interpolate(f, [0, 1 * fps], [0, 1], { extrapolateRight: "clamp" })
    }
  />
);
```

使用 `muted` 來完全靜音影片：

```tsx
<Video src={staticFile("video.mp4")} muted />
```

## 速度

使用 `playbackRate` 來改變播放速度：

```tsx
<Video src={staticFile("video.mp4")} playbackRate={2} /> {/* 2 倍速 */}
<Video src={staticFile("video.mp4")} playbackRate={0.5} /> {/* 半速 */}
```

不支援倒轉播放。

## 循環播放

使用 `loop` 來無限循環播放影片：

```tsx
<Video src={staticFile("video.mp4")} loop />
```

使用 `loopVolumeCurveBehavior` 來控制循環時影格計數的行為：

- `"repeat"`：每次循環影格計數重置為 0（用於 `volume` 回呼）
- `"extend"`：影格計數持續遞增

```tsx
<Video
  src={staticFile("video.mp4")}
  loop
  loopVolumeCurveBehavior="extend"
  volume={(f) => interpolate(f, [0, 300], [1, 0])} // 跨多次循環淡出
/>
```

## 音高

使用 `toneFrequency` 來調整音高而不影響速度。值範圍從 0.01 到 2：

```tsx
<Video
  src={staticFile("video.mp4")}
  toneFrequency={1.5} // 較高音高
/>
<Video
  src={staticFile("video.mp4")}
  toneFrequency={0.8} // 較低音高
/>
```

音高調整僅在伺服器端渲染時有效，在 Remotion Studio 預覽或 `<Player />` 中無效。
