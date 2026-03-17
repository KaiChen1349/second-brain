---
name: compositions
description: Defining compositions, stills, folders, default props and dynamic metadata
metadata:
  tags: composition, still, folder, props, metadata
---

`<Composition>` 定義了可渲染影片的元件、寬度、高度、fps 和持續時間。

通常放置在 `src/Root.tsx` 檔案中。

```tsx
import {Composition} from 'remotion';
import {MyComposition} from './MyComposition';

export const RemotionRoot = () => {
  return <Composition id="MyComposition" component={MyComposition} durationInFrames={100} fps={30} width={1080} height={1080} />;
};
```

## 預設屬性

傳遞 `defaultProps` 來提供元件的初始值。
值必須是 JSON 可序列化的（支援 `Date`、`Map`、`Set` 和 `staticFile()`）。

```tsx
import {Composition} from 'remotion';
import {MyComposition, MyCompositionProps} from './MyComposition';

export const RemotionRoot = () => {
  return (
    <Composition
      id="MyComposition"
      component={MyComposition}
      durationInFrames={100}
      fps={30}
      width={1080}
      height={1080}
      defaultProps={
        {
          title: 'Hello World',
          color: '#ff0000',
        } satisfies MyCompositionProps
      }
    />
  );
};
```

使用 `type` 宣告而非 `interface` 來定義屬性，以確保 `defaultProps` 的型別安全。

## 資料夾

使用 `<Folder>` 在側邊欄中組織合成。
資料夾名稱只能包含字母、數字和連字號。

```tsx
import {Composition, Folder} from 'remotion';

export const RemotionRoot = () => {
  return (
    <>
      <Folder name="Marketing">
        <Composition id="Promo" /* ... */ />
        <Composition id="Ad" /* ... */ />
      </Folder>
      <Folder name="Social">
        <Folder name="Instagram">
          <Composition id="Story" /* ... */ />
          <Composition id="Reel" /* ... */ />
        </Folder>
      </Folder>
    </>
  );
};
```

## 靜態圖片

使用 `<Still>` 來製作單幀圖片。不需要 `durationInFrames` 或 `fps`。

```tsx
import {Still} from 'remotion';
import {Thumbnail} from './Thumbnail';

export const RemotionRoot = () => {
  return <Still id="Thumbnail" component={Thumbnail} width={1280} height={720} />;
};
```

## 計算中繼資料

使用 `calculateMetadata` 根據資料動態設定尺寸、持續時間或屬性。

```tsx
import {Composition, CalculateMetadataFunction} from 'remotion';
import {MyComposition, MyCompositionProps} from './MyComposition';

const calculateMetadata: CalculateMetadataFunction<MyCompositionProps> = async ({props, abortSignal}) => {
  const data = await fetch(`https://api.example.com/video/${props.videoId}`, {
    signal: abortSignal,
  }).then((res) => res.json());

  return {
    durationInFrames: Math.ceil(data.duration * 30),
    props: {
      ...props,
      videoUrl: data.url,
    },
  };
};

export const RemotionRoot = () => {
  return (
    <Composition
      id="MyComposition"
      component={MyComposition}
      durationInFrames={100} // 預設值，會被覆蓋
      fps={30}
      width={1080}
      height={1080}
      defaultProps={{videoId: 'abc123'}}
      calculateMetadata={calculateMetadata}
    />
  );
};
```

此函式可回傳 `props`、`durationInFrames`、`width`、`height`、`fps` 和編解碼器相關的預設值。在渲染開始前執行一次。

## 在合成中嵌套合成

要在一個合成中加入另一個合成，可以使用帶有 `width` 和 `height` 屬性的 `<Sequence>` 元件來指定合成的大小。

```tsx
<AbsoluteFill>
  <Sequence width={COMPOSITION_WIDTH} height={COMPOSITION_HEIGHT}>
    <CompositionComponent />
  </Sequence>
</AbsoluteFill>
```
