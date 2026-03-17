---
name: transitions
description: Fullscreen scene transitions for Remotion.
metadata:
  tags: transitions, fade, slide, wipe, scenes
---

## 全螢幕轉場

使用 `<TransitionSeries>` 在多個場景或片段之間製作動畫。
這會將子元素設為絕對定位。

## 前置條件

首先，需要安裝 @remotion/transitions 套件。
如果尚未安裝，請使用以下命令：

```bash
npx remotion add @remotion/transitions # 如果專案使用 npm
bunx remotion add @remotion/transitions # 如果專案使用 bun
yarn remotion add @remotion/transitions # 如果專案使用 yarn
pnpm exec remotion add @remotion/transitions # 如果專案使用 pnpm
```

## 使用範例

```tsx
import {TransitionSeries, linearTiming} from '@remotion/transitions';
import {fade} from '@remotion/transitions/fade';

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneA />
  </TransitionSeries.Sequence>
  <TransitionSeries.Transition presentation={fade()} timing={linearTiming({durationInFrames: 15})} />
  <TransitionSeries.Sequence durationInFrames={60}>
    <SceneB />
  </TransitionSeries.Sequence>
</TransitionSeries>;
```

## 可用的轉場類型

從各自的模組匯入轉場：

```tsx
import {fade} from '@remotion/transitions/fade';
import {slide} from '@remotion/transitions/slide';
import {wipe} from '@remotion/transitions/wipe';
import {flip} from '@remotion/transitions/flip';
import {clockWipe} from '@remotion/transitions/clock-wipe';
```

## 帶方向的滑動轉場

指定進入/退出動畫的滑動方向。

```tsx
import {slide} from '@remotion/transitions/slide';

<TransitionSeries.Transition presentation={slide({direction: 'from-left'})} timing={linearTiming({durationInFrames: 20})} />;
```

方向：`"from-left"`、`"from-right"`、`"from-top"`、`"from-bottom"`

## 時間選項

```tsx
import {linearTiming, springTiming} from '@remotion/transitions';

// 線性時間 - 等速
linearTiming({durationInFrames: 20});

// 彈簧時間 - 有機運動
springTiming({config: {damping: 200}, durationInFrames: 25});
```

## 持續時間計算

轉場會與相鄰場景重疊，因此合成的總長度會比所有序列持續時間的總和**更短**。

例如，兩個 60 影格的序列和一個 15 影格的轉場：

- 無轉場：`60 + 60 = 120` 影格
- 有轉場：`60 + 60 - 15 = 105` 影格

轉場持續時間被減去，因為兩個場景在轉場期間同時播放。

### 取得轉場的持續時間

使用時間物件上的 `getDurationInFrames()` 方法：

```tsx
import {linearTiming, springTiming} from '@remotion/transitions';

const linearDuration = linearTiming({durationInFrames: 20}).getDurationInFrames({fps: 30});
// 回傳 20

const springDuration = springTiming({config: {damping: 200}}).getDurationInFrames({fps: 30});
// 回傳根據彈簧物理計算的持續時間
```

對於沒有明確 `durationInFrames` 的 `springTiming`，持續時間取決於 `fps`，因為它會計算彈簧動畫何時穩定。

### 計算合成的總持續時間

```tsx
import {linearTiming} from '@remotion/transitions';

const scene1Duration = 60;
const scene2Duration = 60;
const scene3Duration = 60;

const timing1 = linearTiming({durationInFrames: 15});
const timing2 = linearTiming({durationInFrames: 20});

const transition1Duration = timing1.getDurationInFrames({fps: 30});
const transition2Duration = timing2.getDurationInFrames({fps: 30});

const totalDuration = scene1Duration + scene2Duration + scene3Duration - transition1Duration - transition2Duration;
// 60 + 60 + 60 - 15 - 20 = 145 影格
```
