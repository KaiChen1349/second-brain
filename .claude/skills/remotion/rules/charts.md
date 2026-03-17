---
name: charts
description: Chart and data visualization patterns for Remotion. Use when creating bar charts, pie charts, histograms, progress bars, or any data-driven animations.
metadata:
  tags: charts, data, visualization, bar-chart, pie-chart, graphs
---

# Remotion 中的圖表

你可以使用一般的 React 程式碼在 Remotion 中建立長條圖 - 允許使用 HTML、SVG 以及 D3.js。

## 禁止非 `useCurrentFrame()` 驅動的動畫

停用所有第三方函式庫的動畫。
它們會在渲染時導致閃爍。
改為從 `useCurrentFrame()` 驅動所有動畫。

## 長條圖動畫

參見 [Bar Chart Example](assets/charts/bar-chart.tsx) 了解基本實作範例。

### 交錯長條

你可以這樣為長條的高度製作動畫並加入交錯效果：

```tsx
const STAGGER_DELAY = 5;
const frame = useCurrentFrame();
const {fps} = useVideoConfig();

const bars = data.map((item, i) => {
  const delay = i * STAGGER_DELAY;
  const height = spring({
    frame,
    fps,
    delay,
    config: {damping: 200},
  });
  return <div style={{height: height * item.value}} />;
});
```

## 圓餅圖動畫

使用 stroke-dashoffset 為扇區製作動畫，從 12 點鐘方向開始。

```tsx
const frame = useCurrentFrame();
const {fps} = useVideoConfig();

const progress = interpolate(frame, [0, 100], [0, 1]);

const circumference = 2 * Math.PI * radius;
const segmentLength = (value / total) * circumference;
const offset = interpolate(progress, [0, 1], [segmentLength, 0]);

<circle r={radius} cx={center} cy={center} fill="none" stroke={color} strokeWidth={strokeWidth} strokeDasharray={`${segmentLength} ${circumference}`} strokeDashoffset={offset} transform={`rotate(-90 ${center} ${center})`} />;
```
