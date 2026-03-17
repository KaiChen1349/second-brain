---
name: timing
description: Interpolation curves in Remotion - linear, easing, spring animations
metadata:
  tags: spring, bounce, easing, interpolation
---

簡單的線性插值使用 `interpolate` 函式完成。

```ts title="在 100 幀內從 0 到 1"
import {interpolate} from 'remotion';

const opacity = interpolate(frame, [0, 100], [0, 1]);
```

預設情況下，值不會被限制，所以值可能超出 [0, 1] 的範圍。
以下是限制值的方法：

```ts title="在 100 幀內從 0 到 1，帶外推限制"
const opacity = interpolate(frame, [0, 100], [0, 1], {
  extrapolateRight: 'clamp',
  extrapolateLeft: 'clamp',
});
```

## 彈簧動畫

彈簧動畫具有更自然的運動感。
它們隨時間從 0 到 1。

```ts title="在 100 幀內從 0 到 1 的彈簧動畫"
import {spring, useCurrentFrame, useVideoConfig} from 'remotion';

const frame = useCurrentFrame();
const {fps} = useVideoConfig();

const scale = spring({
  frame,
  fps,
});
```

### 物理屬性

預設配置為：`mass: 1, damping: 10, stiffness: 100`。
這會導致動畫在穩定前有一些彈跳。

可以這樣覆蓋配置：

```ts
const scale = spring({
  frame,
  fps,
  config: {damping: 200},
});
```

推薦的無彈跳自然運動配置為：`{ damping: 200 }`。

以下是一些常見配置：

```tsx
const smooth = {damping: 200}; // 平滑，無彈跳（細微的顯現效果）
const snappy = {damping: 20, stiffness: 200}; // 迅速，最小彈跳（UI 元素）
const bouncy = {damping: 8}; // 彈跳式進入（活潑的動畫）
const heavy = {damping: 15, stiffness: 80, mass: 2}; // 厚重，緩慢，小幅彈跳
```

### 延遲

動畫預設立即開始。
使用 `delay` 參數來延遲動畫指定的影格數。

```tsx
const entrance = spring({
  frame: frame - ENTRANCE_DELAY,
  fps,
  delay: 20,
});
```

### 持續時間

`spring()` 有基於物理屬性的自然持續時間。
要將動畫拉伸到特定持續時間，使用 `durationInFrames` 參數。

```tsx
const spring = spring({
  frame,
  fps,
  durationInFrames: 40,
});
```

### 結合 spring() 和 interpolate()

將彈簧輸出（0-1）對應到自訂範圍：

```tsx
const springProgress = spring({
  frame,
  fps,
});

// 對應到旋轉
const rotation = interpolate(springProgress, [0, 1], [0, 360]);

<div style={{rotate: rotation + 'deg'}} />;
```

### 加法彈簧

彈簧只回傳數字，所以可以進行數學運算：

```tsx
const frame = useCurrentFrame();
const {fps, durationInFrames} = useVideoConfig();

const inAnimation = spring({
  frame,
  fps,
});
const outAnimation = spring({
  frame,
  fps,
  durationInFrames: 1 * fps,
  delay: durationInFrames - 1 * fps,
});

const scale = inAnimation - outAnimation;
```

## 緩動

可以為 `interpolate` 函式加入緩動：

```ts
import {interpolate, Easing} from 'remotion';

const value1 = interpolate(frame, [0, 100], [0, 1], {
  easing: Easing.inOut(Easing.quad),
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

預設緩動為 `Easing.linear`。
還有各種其他凸度：

- `Easing.in` 用於慢速開始並加速
- `Easing.out` 用於快速開始並減速
- `Easing.inOut`

以及曲線（從最線性到最彎曲排列）：

- `Easing.quad`
- `Easing.sin`
- `Easing.exp`
- `Easing.circle`

凸度和曲線需要組合才能構成緩動函式：

```ts
const value1 = interpolate(frame, [0, 100], [0, 1], {
  easing: Easing.inOut(Easing.quad),
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

也支援三次貝茲曲線：

```ts
const value1 = interpolate(frame, [0, 100], [0, 1], {
  easing: Easing.bezier(0.8, 0.22, 0.96, 0.65),
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```
