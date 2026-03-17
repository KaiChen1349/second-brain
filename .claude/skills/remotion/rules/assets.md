---
name: assets
description: Importing images, videos, audio, and fonts into Remotion
metadata:
  tags: assets, staticFile, images, fonts, public
---

# 在 Remotion 中匯入素材

## public 資料夾

將素材放置在專案根目錄的 `public/` 資料夾中。

## 使用 staticFile()

你必須使用 `staticFile()` 來參照 `public/` 資料夾中的檔案：

```tsx
import {Img, staticFile} from 'remotion';

export const MyComposition = () => {
  return <Img src={staticFile('logo.png')} />;
};
```

此函式會回傳一個編碼過的 URL，在部署到子目錄時也能正確運作。

## 搭配元件使用

**圖片：**

```tsx
import {Img, staticFile} from 'remotion';

<Img src={staticFile('photo.png')} />;
```

**影片：**

```tsx
import {Video} from '@remotion/media';
import {staticFile} from 'remotion';

<Video src={staticFile('clip.mp4')} />;
```

**音訊：**

```tsx
import {Audio} from '@remotion/media';
import {staticFile} from 'remotion';

<Audio src={staticFile('music.mp3')} />;
```

**字型：**

```tsx
import {staticFile} from 'remotion';

const fontFamily = new FontFace('MyFont', `url(${staticFile('font.woff2')})`);
await fontFamily.load();
document.fonts.add(fontFamily);
```

## 遠端 URL

遠端 URL 可以直接使用，無需 `staticFile()`：

```tsx
<Img src="https://example.com/image.png" />
<Video src="https://remotion.media/video.mp4" />
```

## 重要注意事項

- Remotion 元件（`<Img>`、`<Video>`、`<Audio>`）會確保素材在渲染前完全載入
- 檔案名稱中的特殊字元（`#`、`?`、`&`）會自動編碼
