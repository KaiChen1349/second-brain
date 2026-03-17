---
name: calculate-metadata
description: Dynamically set composition duration, dimensions, and props
metadata:
  tags: calculateMetadata, duration, dimensions, props, dynamic
---

# 使用 calculateMetadata

在 `<Composition>` 上使用 `calculateMetadata` 來在渲染前動態設定時長、尺寸和轉換屬性。

```tsx
<Composition id="MyComp" component={MyComponent} durationInFrames={300} fps={30} width={1920} height={1080} defaultProps={{videoSrc: 'https://remotion.media/video.mp4'}} calculateMetadata={calculateMetadata} />
```

## 根據影片設定時長

使用 mediabunny/metadata 技能的 `getMediaMetadata()` 函式來取得影片時長：

```tsx
import {CalculateMetadataFunction} from 'remotion';
import {getMediaMetadata} from '../get-media-metadata';

const calculateMetadata: CalculateMetadataFunction<Props> = async ({props}) => {
  const {durationInSeconds} = await getMediaMetadata(props.videoSrc);

  return {
    durationInFrames: Math.ceil(durationInSeconds * 30),
  };
};
```

## 匹配影片尺寸

```tsx
const calculateMetadata: CalculateMetadataFunction<Props> = async ({props}) => {
  const {durationInSeconds, dimensions} = await getMediaMetadata(props.videoSrc);

  return {
    durationInFrames: Math.ceil(durationInSeconds * 30),
    width: dimensions?.width ?? 1920,
    height: dimensions?.height ?? 1080,
  };
};
```

## 根據多部影片設定時長

```tsx
const calculateMetadata: CalculateMetadataFunction<Props> = async ({props}) => {
  const metadataPromises = props.videos.map((video) => getMediaMetadata(video.src));
  const allMetadata = await Promise.all(metadataPromises);

  const totalDuration = allMetadata.reduce((sum, meta) => sum + meta.durationInSeconds, 0);

  return {
    durationInFrames: Math.ceil(totalDuration * 30),
  };
};
```

## 設定預設輸出檔名

根據屬性設定預設輸出檔名：

```tsx
const calculateMetadata: CalculateMetadataFunction<Props> = async ({props}) => {
  return {
    defaultOutName: `video-${props.id}.mp4`,
  };
};
```

## 轉換屬性

在渲染前獲取資料或轉換屬性：

```tsx
const calculateMetadata: CalculateMetadataFunction<Props> = async ({props, abortSignal}) => {
  const response = await fetch(props.dataUrl, {signal: abortSignal});
  const data = await response.json();

  return {
    props: {
      ...props,
      fetchedData: data,
    },
  };
};
```

當屬性在 Studio 中變更時，`abortSignal` 會取消過時的請求。

## 回傳值

所有欄位都是選填的。回傳的值會覆蓋 `<Composition>` 的屬性：

- `durationInFrames`：影格數
- `width`：合成寬度（像素）
- `height`：合成高度（像素）
- `fps`：每秒影格數
- `props`：傳遞給元件的轉換後屬性
- `defaultOutName`：預設輸出檔名
- `defaultCodec`：預設渲染編解碼器
