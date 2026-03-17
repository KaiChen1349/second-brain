---
name: import-srt-captions
description: Importing .srt subtitle files into Remotion using @remotion/captions
metadata:
  tags: captions, subtitles, srt, import, parse
---

# 將 .srt 字幕匯入 Remotion

如果你有現有的 `.srt` 字幕檔案，可以使用 `@remotion/captions` 的 `parseSrt()` 將其匯入 Remotion。

## 前置條件

首先需要安裝 @remotion/captions 套件。
如果尚未安裝，使用以下命令：

```bash
npx remotion add @remotion/captions # 如果專案使用 npm
bunx remotion add @remotion/captions # 如果專案使用 bun
yarn remotion add @remotion/captions # 如果專案使用 yarn
pnpm exec remotion add @remotion/captions # 如果專案使用 pnpm
```

## 讀取 .srt 檔案

使用 `staticFile()` 來引用 `public` 資料夾中的 `.srt` 檔案，然後獲取並解析它：

```tsx
import {useState, useEffect, useCallback} from 'react';
import {AbsoluteFill, staticFile, useDelayRender} from 'remotion';
import {parseSrt} from '@remotion/captions';
import type {Caption} from '@remotion/captions';

export const MyComponent: React.FC = () => {
  const [captions, setCaptions] = useState<Caption[] | null>(null);
  const {delayRender, continueRender, cancelRender} = useDelayRender();
  const [handle] = useState(() => delayRender());

  const fetchCaptions = useCallback(async () => {
    try {
      const response = await fetch(staticFile('subtitles.srt'));
      const text = await response.text();
      const {captions: parsed} = parseSrt({input: text});
      setCaptions(parsed);
      continueRender(handle);
    } catch (e) {
      cancelRender(e);
    }
  }, [continueRender, cancelRender, handle]);

  useEffect(() => {
    fetchCaptions();
  }, [fetchCaptions]);

  if (!captions) {
    return null;
  }

  return <AbsoluteFill>{/* 在此使用字幕 */}</AbsoluteFill>;
};
```

也支援遠端 URL - 你可以透過 URL `fetch()` 遠端檔案，而不使用 `staticFile()`。

## 使用匯入的字幕

解析完成後，字幕會以 `Caption` 格式呈現，可以搭配所有 `@remotion/captions` 工具使用。
