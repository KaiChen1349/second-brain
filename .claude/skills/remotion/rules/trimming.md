---
name: trimming
description: Trimming patterns for Remotion - cut the beginning or end of animations
metadata:
  tags: sequence, trim, clip, cut, offset
---

使用帶有負 `from` 值的 `<Sequence>` 來裁剪動畫的開頭。

## 裁剪開頭

負的 `from` 值會將時間向後移動，使動畫從中途開始：

```tsx
import { Sequence, useVideoConfig } from "remotion";

const fps = useVideoConfig();

<Sequence from={-0.5 * fps}>
  <MyAnimation />
</Sequence>
```

動畫會從進度的第 15 影格處開始顯示 - 前 15 影格被裁剪掉。
在 `<MyAnimation>` 內部，`useCurrentFrame()` 從 15 開始而非 0。

## 裁剪結尾

使用 `durationInFrames` 在指定的持續時間後卸載內容：

```tsx

<Sequence durationInFrames={1.5 * fps}>
  <MyAnimation />
</Sequence>
```

動畫播放 45 影格後，元件被卸載。

## 裁剪並延遲

巢狀序列可以同時裁剪開頭和延遲顯示時間：

```tsx
<Sequence from={30}>
  <Sequence from={-15}>
    <MyAnimation />
  </Sequence>
</Sequence>
```

內層序列裁剪開頭的 15 影格，外層序列將結果延遲 30 影格。
