---
name: parameters
description: Make a video parametrizable by adding a Zod schema
metadata:
  tags: parameters, zod, schema
---

要使影片可參數化，可以在合成中加入 Zod schema。

首先，必須安裝 `zod` - 版本必須剛好是 `3.22.3`。

搜尋專案中的 lockfile 並根據套件管理器執行對應的命令：

如果找到 `package-lock.json`，使用以下命令：

```bash
npm i zod@3.22.3
```

如果找到 `bun.lockb`，使用以下命令：

```bash
bun i zod@3.22.3
```

如果找到 `yarn.lock`，使用以下命令：

```bash
yarn add zod@3.22.3
```

如果找到 `pnpm-lock.yaml`，使用以下命令：

```bash
pnpm i zod@3.22.3
```

然後，可以在元件旁邊定義 Zod schema：

```tsx title="src/MyComposition.tsx"
import {z} from 'zod';

export const MyCompositionSchema = z.object({
  title: z.string(),
});

const MyComponent: React.FC<z.infer<typeof MyCompositionSchema>> = () => {
  return (
    <div>
      <h1>{props.title}</h1>
    </div>
  );
};
```

在根檔案中，可以將 schema 傳遞給合成：

```tsx title="src/Root.tsx"
import {Composition} from 'remotion';
import {MycComponent, MyCompositionSchema} from './MyComposition';

export const RemotionRoot = () => {
  return <Composition id="MyComposition" component={MyComponent} durationInFrames={100} fps={30} width={1080} height={1080} defaultProps={{title: 'Hello World'}} schema={MyCompositionSchema} />;
};
```

現在，使用者可以在側邊欄中視覺化地編輯參數。

Zod 支援的所有 schema 都被 Remotion 支援。

Remotion 要求頂層類型是 z.object()，因為 React 元件的 props 集合始終是一個物件。

## 色彩選擇器

要新增色彩選擇器，使用 `@remotion/zod-types` 的 `zColor()`。

如果尚未安裝，使用以下命令：

```bash
npx remotion add @remotion/zod-types # 如果專案使用 npm
bunx remotion add @remotion/zod-types # 如果專案使用 bun
yarn remotion add @remotion/zod-types # 如果專案使用 yarn
pnpm exec remotion add @remotion/zod-types # 如果專案使用 pnpm
```

然後從 `@remotion/zod-types` 匯入 `zColor`：

```tsx
import {zColor} from '@remotion/zod-types';
```

然後在 schema 中使用：

```tsx
export const MyCompositionSchema = z.object({
  color: zColor(),
});
```
