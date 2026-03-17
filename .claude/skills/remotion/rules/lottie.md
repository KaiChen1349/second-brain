---
name: lottie
description: Embedding Lottie animations in Remotion.
metadata:
  category: Animation
---

# 在 Remotion 中使用 Lottie 動畫

## 前置條件

首先需要安裝 @remotion/lottie 套件。
如果尚未安裝，使用以下命令：

```bash
npx remotion add @remotion/lottie # 如果專案使用 npm
bunx remotion add @remotion/lottie # 如果專案使用 bun
yarn remotion add @remotion/lottie # 如果專案使用 yarn
pnpm exec remotion add @remotion/lottie # 如果專案使用 pnpm
```

## 顯示 Lottie 檔案

要匯入 Lottie 動畫：

- 獲取 Lottie 素材
- 將載入過程包裝在 `delayRender()` 和 `continueRender()` 中
- 將動畫資料儲存在 state 中
- 使用 `@remotion/lottie` 套件的 `Lottie` 元件渲染動畫

```tsx
import {Lottie, LottieAnimationData} from '@remotion/lottie';
import {useEffect, useState} from 'react';
import {cancelRender, continueRender, delayRender} from 'remotion';

export const MyAnimation = () => {
  const [handle] = useState(() => delayRender('Loading Lottie animation'));

  const [animationData, setAnimationData] = useState<LottieAnimationData | null>(null);

  useEffect(() => {
    fetch('https://assets4.lottiefiles.com/packages/lf20_zyquagfl.json')
      .then((data) => data.json())
      .then((json) => {
        setAnimationData(json);
        continueRender(handle);
      })
      .catch((err) => {
        cancelRender(err);
      });
  }, [handle]);

  if (!animationData) {
    return null;
  }

  return <Lottie animationData={animationData} />;
};
```

## 樣式和動畫

Lottie 支援 `style` 屬性以允許設定樣式和動畫：

```tsx
return <Lottie animationData={animationData} style={{width: 400, height: 400}} />;
```
