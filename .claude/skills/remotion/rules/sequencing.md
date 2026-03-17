---
name: sequencing
description: Sequencing patterns for Remotion - delay, trim, limit duration of items
metadata:
  tags: sequence, series, timing, delay, trim
---

使用 `<Sequence>` 來延遲元素在時間軸上出現的時間。

```tsx
import { Sequence } from "remotion";

const {fps} = useVideoConfig();

<Sequence from={1 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
  <Title />
</Sequence>
<Sequence from={2 * fps} durationInFrames={2 * fps} premountFor={1 * fps}>
  <Subtitle />
</Sequence>
```

預設情況下，這會將元件包裝在一個絕對填滿的元素中。
如果不想被包裝，使用 `layout` 屬性：

```tsx
<Sequence layout="none">
  <Title />
</Sequence>
```

## 預掛載

這會在元件實際播放之前，就在時間軸中載入它。
務必預掛載任何 `<Sequence>`！

```tsx
<Sequence premountFor={1 * fps}>
  <Title />
</Sequence>
```

## 系列

當元素應該一個接一個播放且不重疊時，使用 `<Series>`。

```tsx
import {Series} from 'remotion';

<Series>
  <Series.Sequence durationInFrames={45}>
    <Intro />
  </Series.Sequence>
  <Series.Sequence durationInFrames={60}>
    <MainContent />
  </Series.Sequence>
  <Series.Sequence durationInFrames={30}>
    <Outro />
  </Series.Sequence>
</Series>;
```

與 `<Sequence>` 相同，使用 `<Series.Sequence>` 時，項目預設會被包裝在絕對填滿的元素中，除非 `layout` 屬性設定為 `none`。

### 帶重疊的系列

使用負偏移來建立重疊的序列：

```tsx
<Series>
  <Series.Sequence durationInFrames={60}>
    <SceneA />
  </Series.Sequence>
  <Series.Sequence offset={-15} durationInFrames={60}>
    {/* 在 SceneA 結束前 15 幀開始 */}
    <SceneB />
  </Series.Sequence>
</Series>
```

## Sequence 內的影格參考

在 Sequence 內部，`useCurrentFrame()` 回傳本地影格（從 0 開始）：

```tsx
<Sequence from={60} durationInFrames={30}>
  <MyComponent />
  {/* 在 MyComponent 內，useCurrentFrame() 回傳 0-29，而不是 60-89 */}
</Sequence>
```

## 巢狀 Sequence

Sequence 可以巢狀使用以實現複雜的時序：

```tsx
<Sequence from={0} durationInFrames={120}>
  <Background />
  <Sequence from={15} durationInFrames={90} layout="none">
    <Title />
  </Sequence>
  <Sequence from={45} durationInFrames={60} layout="none">
    <Subtitle />
  </Sequence>
</Sequence>
```

## 在合成中巢狀嵌入合成

要在另一個合成中加入合成，可以使用帶有 `width` 和 `height` 屬性的 `<Sequence>` 元件來指定合成的大小。

```tsx
<AbsoluteFill>
  <Sequence width={COMPOSITION_WIDTH} height={COMPOSITION_HEIGHT}>
    <CompositionComponent />
  </Sequence>
</AbsoluteFill>
```
