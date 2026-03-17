---
name: display-captions
description: Displaying captions in Remotion with TikTok-style pages and word highlighting
metadata:
  tags: captions, subtitles, display, tiktok, highlight
---

# 在 Remotion 中顯示字幕

本指南說明如何在 Remotion 中顯示字幕，假設你已經擁有 `Caption` 格式的字幕。

## 前置條件

首先，需要安裝 @remotion/captions 套件。
如果尚未安裝，請使用以下命令：

```bash
npx remotion add @remotion/captions # 如果專案使用 npm
bunx remotion add @remotion/captions # 如果專案使用 bun
yarn remotion add @remotion/captions # 如果專案使用 yarn
pnpm exec remotion add @remotion/captions # 如果專案使用 pnpm
```

## 建立分頁

使用 `createTikTokStyleCaptions()` 將字幕分組為頁面。`combineTokensWithinMilliseconds` 選項控制一次顯示多少字詞：

```tsx
import {useMemo} from 'react';
import {createTikTokStyleCaptions} from '@remotion/captions';
import type {Caption} from '@remotion/captions';

// 字幕切換頻率（以毫秒為單位）
// 較高的值 = 每頁更多字詞
// 較低的值 = 更少字詞（更接近逐字顯示）
const SWITCH_CAPTIONS_EVERY_MS = 1200;

const {pages} = useMemo(() => {
  return createTikTokStyleCaptions({
    captions,
    combineTokensWithinMilliseconds: SWITCH_CAPTIONS_EVERY_MS,
  });
}, [captions]);
```

## 使用 Sequence 渲染

遍歷頁面，並將每個頁面渲染在 `<Sequence>` 中。根據頁面時間計算起始影格和持續時間：

```tsx
import {Sequence, useVideoConfig, AbsoluteFill} from 'remotion';
import type {TikTokPage} from '@remotion/captions';

const CaptionedContent: React.FC = () => {
  const {fps} = useVideoConfig();

  return (
    <AbsoluteFill>
      {pages.map((page, index) => {
        const nextPage = pages[index + 1] ?? null;
        const startFrame = (page.startMs / 1000) * fps;
        const endFrame = Math.min(
          nextPage ? (nextPage.startMs / 1000) * fps : Infinity,
          startFrame + (SWITCH_CAPTIONS_EVERY_MS / 1000) * fps,
        );
        const durationInFrames = endFrame - startFrame;

        if (durationInFrames <= 0) {
          return null;
        }

        return (
          <Sequence
            key={index}
            from={startFrame}
            durationInFrames={durationInFrames}
          >
            <CaptionPage page={page} />
          </Sequence>
        );
      })}
    </AbsoluteFill>
  );
};
```

## 字詞高亮

字幕頁面包含 `tokens`，你可以用它來高亮當前正在朗讀的字詞：

```tsx
import {AbsoluteFill, useCurrentFrame, useVideoConfig} from 'remotion';
import type {TikTokPage} from '@remotion/captions';

const HIGHLIGHT_COLOR = '#39E508';

const CaptionPage: React.FC<{page: TikTokPage}> = ({page}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();

  // 相對於 sequence 開始的當前時間
  const currentTimeMs = (frame / fps) * 1000;
  // 加上頁面起始時間轉換為絕對時間
  const absoluteTimeMs = page.startMs + currentTimeMs;

  return (
    <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center'}}>
      <div style={{fontSize: 80, fontWeight: 'bold', whiteSpace: 'pre'}}>
        {page.tokens.map((token) => {
          const isActive =
            token.fromMs <= absoluteTimeMs && token.toMs > absoluteTimeMs;

          return (
            <span
              key={token.fromMs}
              style={{color: isActive ? HIGHLIGHT_COLOR : 'white'}}
            >
              {token.text}
            </span>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```
