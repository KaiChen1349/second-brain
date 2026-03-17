---
name: animations
description: Fundamental animation skills for Remotion
metadata:
  tags: animations, transitions, frames, useCurrentFrame
---

所有動畫必須由 `useCurrentFrame()` hook 驅動。
以秒為單位撰寫動畫，並乘以 `useVideoConfig()` 的 `fps` 值。

```tsx
import { useCurrentFrame } from "remotion";

export const FadeIn = () => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();

  const opacity = interpolate(frame, [0, 2 * fps], [0, 1], {
    extrapolateRight: 'clamp',
  });

  return (
    <div style={{ opacity }}>Hello World!</div>
  );
};
```

禁止使用 CSS 過渡或動畫 - 它們無法正確渲染。
禁止使用 Tailwind 動畫類別名稱 - 它們無法正確渲染。
