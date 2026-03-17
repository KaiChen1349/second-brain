---
name: audio
description: Using audio and sound in Remotion - importing, trimming, volume, speed, pitch
metadata:
  tags: audio, media, trim, volume, speed, loop, pitch, mute, sound, sfx
---

# 在 Remotion 中使用音訊

## 前置條件

首先，需要安裝 @remotion/media 套件。
如果尚未安裝，請使用以下命令：

```bash
npx remotion add @remotion/media # 如果專案使用 npm
bunx remotion add @remotion/media # 如果專案使用 bun
yarn remotion add @remotion/media # 如果專案使用 yarn
pnpm exec remotion add @remotion/media # 如果專案使用 pnpm
```

## 匯入音訊

使用 `@remotion/media` 的 `<Audio>` 來將音訊加入你的合成。

```tsx
import { Audio } from "@remotion/media";
import { staticFile } from "remotion";

export const MyComposition = () => {
  return <Audio src={staticFile("audio.mp3")} />;
};
```

也支援遠端 URL：

```tsx
<Audio src="https://remotion.media/audio.mp3" />
```

預設情況下，音訊從開頭播放，音量最大且播放完整長度。
可以透過新增多個 `<Audio>` 元件來疊加多條音軌。

## 裁剪

使用 `trimBefore` 和 `trimAfter` 來移除音訊的部分片段。值以影格為單位。

```tsx
const { fps } = useVideoConfig();

return (
  <Audio
    src={staticFile("audio.mp3")}
    trimBefore={2 * fps} // 跳過前 2 秒
    trimAfter={10 * fps} // 在第 10 秒處結束
  />
);
```

音訊仍然從合成的開頭開始播放 - 只播放指定的部分。

## 延遲播放

將音訊包裹在 `<Sequence>` 中以延遲播放時間：

```tsx
import { Sequence, staticFile } from "remotion";
import { Audio } from "@remotion/media";

const { fps } = useVideoConfig();

return (
  <Sequence from={1 * fps}>
    <Audio src={staticFile("audio.mp3")} />
  </Sequence>
);
```

音訊將在 1 秒後開始播放。

## 音量

設定靜態音量（0 到 1）：

```tsx
<Audio src={staticFile("audio.mp3")} volume={0.5} />
```

或使用回呼函式根據當前影格動態調整音量：

```tsx
import { interpolate } from "remotion";

const { fps } = useVideoConfig();

return (
  <Audio
    src={staticFile("audio.mp3")}
    volume={(f) =>
      interpolate(f, [0, 1 * fps], [0, 1], { extrapolateRight: "clamp" })
    }
  />
);
```

`f` 的值從音訊開始播放時為 0，而非合成的影格。

## 靜音

使用 `muted` 來靜音。可以動態設定：

```tsx
const frame = useCurrentFrame();
const { fps } = useVideoConfig();

return (
  <Audio
    src={staticFile("audio.mp3")}
    muted={frame >= 2 * fps && frame <= 4 * fps} // 在 2 秒到 4 秒之間靜音
  />
);
```

## 速度

使用 `playbackRate` 來改變播放速度：

```tsx
<Audio src={staticFile("audio.mp3")} playbackRate={2} /> {/* 2 倍速 */}
<Audio src={staticFile("audio.mp3")} playbackRate={0.5} /> {/* 半速 */}
```

不支援倒轉播放。

## 循環播放

使用 `loop` 來無限循環播放音訊：

```tsx
<Audio src={staticFile("audio.mp3")} loop />
```

使用 `loopVolumeCurveBehavior` 來控制循環時影格計數的行為：

- `"repeat"`：每次循環影格計數重置為 0（預設）
- `"extend"`：影格計數持續遞增

```tsx
<Audio
  src={staticFile("audio.mp3")}
  loop
  loopVolumeCurveBehavior="extend"
  volume={(f) => interpolate(f, [0, 300], [1, 0])} // 跨多次循環淡出
/>
```

## 音高

使用 `toneFrequency` 來調整音高而不影響速度。值範圍從 0.01 到 2：

```tsx
<Audio
  src={staticFile("audio.mp3")}
  toneFrequency={1.5} // 較高音高
/>
<Audio
  src={staticFile("audio.mp3")}
  toneFrequency={0.8} // 較低音高
/>
```

音高調整僅在伺服器端渲染時有效，在 Remotion Studio 預覽或 `<Player />` 中無效。
