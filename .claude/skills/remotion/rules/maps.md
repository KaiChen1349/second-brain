---
name: maps
description: Make map animations with Mapbox
metadata:
  tags: map, map animation, mapbox
---

可以使用 Mapbox 將地圖加入 Remotion 影片。
[Mapbox 文件](https://docs.mapbox.com/mapbox-gl-js/api/) 有 API 參考。

## 前置條件

需要安裝 Mapbox 和 `@turf/turf`。

搜尋專案中的 lockfile，並根據套件管理器執行對應的命令：

如果找到 `package-lock.json`，使用以下命令：

```bash
npm i mapbox-gl @turf/turf @types/mapbox-gl
```

如果找到 `bun.lock`，使用以下命令：

```bash
bun i mapbox-gl @turf/turf @types/mapbox-gl
```

如果找到 `yarn.lock`，使用以下命令：

```bash
yarn add mapbox-gl @turf/turf @types/mapbox-gl
```

如果找到 `pnpm-lock.yaml`，使用以下命令：

```bash
pnpm i mapbox-gl @turf/turf @types/mapbox-gl
```

使用者需要建立免費的 Mapbox 帳戶並前往 https://console.mapbox.com/account/access-tokens/ 建立存取權杖。

需要將 mapbox 權杖加入 `.env` 檔案：

```txt title=".env"
REMOTION_MAPBOX_TOKEN==pk.your-mapbox-access-token
```

## 新增地圖

以下是 Remotion 中地圖的基本範例。

```tsx
import {useEffect, useMemo, useRef, useState} from 'react';
import {AbsoluteFill, useDelayRender, useVideoConfig} from 'remotion';
import mapboxgl, {Map} from 'mapbox-gl';

export const lineCoordinates = [
  [6.56158447265625, 46.059891147620725],
  [6.5691375732421875, 46.05679376154153],
  [6.5842437744140625, 46.05059898938315],
  [6.594886779785156, 46.04702502069337],
  [6.601066589355469, 46.0460718554722],
  [6.6089630126953125, 46.0365370783104],
  [6.6185760498046875, 46.018420689207964],
];

mapboxgl.accessToken = process.env.REMOTION_MAPBOX_TOKEN as string;

export const MyComposition = () => {
  const ref = useRef<HTMLDivElement>(null);
  const {delayRender, continueRender} = useDelayRender();

  const {width, height} = useVideoConfig();
  const [handle] = useState(() => delayRender('Loading map...'));
  const [map, setMap] = useState<Map | null>(null);

  useEffect(() => {
    const _map = new Map({
      container: ref.current!,
      zoom: 11.53,
      center: [6.5615, 46.0598],
      pitch: 65,
      bearing: 0,
      style: '⁠mapbox://styles/mapbox/standard',
      interactive: false,
      fadeDuration: 0,
    });

    _map.on('style.load', () => {
      // 隱藏 Mapbox Standard 樣式中的所有功能
      const hideFeatures = [
        'showRoadsAndTransit',
        'showRoads',
        'showTransit',
        'showPedestrianRoads',
        'showRoadLabels',
        'showTransitLabels',
        'showPlaceLabels',
        'showPointOfInterestLabels',
        'showPointsOfInterest',
        'showAdminBoundaries',
        'showLandmarkIcons',
        'showLandmarkIconLabels',
        'show3dObjects',
        'show3dBuildings',
        'show3dTrees',
        'show3dLandmarks',
        'show3dFacades',
      ];
      for (const feature of hideFeatures) {
        _map.setConfigProperty('basemap', feature, false);
      }

      _map.setConfigProperty('basemap', 'colorMotorways', 'rgba(0, 0, 0, 0)');
      _map.setConfigProperty('basemap', 'colorRoads', 'rgba(0, 0, 0, 0)');
      _map.setConfigProperty('basemap', 'colorTrunks', 'rgba(0, 0, 0, 0)');

      _map.addSource('trace', {
        type: 'geojson',
        data: {
          type: 'Feature',
          properties: {},
          geometry: {
            type: 'LineString',
            coordinates: lineCoordinates,
          },
        },
      });
      _map.addLayer({
        type: 'line',
        source: 'trace',
        id: 'line',
        paint: {
          'line-color': 'black',
          'line-width': 5,
        },
        layout: {
          'line-cap': 'round',
          'line-join': 'round',
        },
      });
    });

    _map.on('load', () => {
      continueRender(handle);
      setMap(_map);
    });
  }, [handle, lineCoordinates]);

  const style: React.CSSProperties = useMemo(() => ({width, height, position: 'absolute'}), [width, height]);

  return <AbsoluteFill ref={ref} style={style} />;
};
```

在 Remotion 中以下事項很重要：

- 動畫必須由 `useCurrentFrame()` 驅動，Mapbox 自帶的動畫應該被停用。例如，`fadeDuration` 屬性應設為 `0`，`interactive` 應設為 `false` 等。
- 地圖載入應使用 `useDelayRender()` 延遲，地圖在載入完成前應設為 `null`。
- 包含 ref 的元素必須有明確的寬度和高度以及 `position: "absolute"`。
- 不要新增 `_map.remove();` 清理函式。

## 繪製線條

除非我要求，否則不要為線條添加發光效果。
除非我要求，否則不要為線條添加額外的點。

## 地圖樣式

預設使用 `mapbox://styles/mapbox/standard` 樣式。
隱藏基本地圖樣式中的標籤。

除非我另有要求，否則移除 Mapbox Standard 樣式中的所有功能。

```tsx
// 隱藏 Mapbox Standard 樣式中的所有功能
const hideFeatures = [
  'showRoadsAndTransit',
  'showRoads',
  'showTransit',
  'showPedestrianRoads',
  'showRoadLabels',
  'showTransitLabels',
  'showPlaceLabels',
  'showPointOfInterestLabels',
  'showPointsOfInterest',
  'showAdminBoundaries',
  'showLandmarkIcons',
  'showLandmarkIconLabels',
  'show3dObjects',
  'show3dBuildings',
  'show3dTrees',
  'show3dLandmarks',
  'show3dFacades',
];
for (const feature of hideFeatures) {
  _map.setConfigProperty('basemap', feature, false);
}

_map.setConfigProperty('basemap', 'colorMotorways', 'transparent');
_map.setConfigProperty('basemap', 'colorRoads', 'transparent');
_map.setConfigProperty('basemap', 'colorTrunks', 'transparent');
```

## 動畫攝影機

你可以透過新增 `useEffect` hook 來根據當前影格更新攝影機位置，讓攝影機沿著線條移動。

除非我要求，否則不要在攝影機角度之間跳切。

```tsx
import * as turf from '@turf/turf';
import {interpolate} from 'remotion';
import {Easing} from 'remotion';
import {useCurrentFrame, useVideoConfig, useDelayRender} from 'remotion';

const animationDuration = 20;
const cameraAltitude = 4000;
```

```tsx
const frame = useCurrentFrame();
const {fps} = useVideoConfig();
const {delayRender, continueRender} = useDelayRender();

useEffect(() => {
  if (!map) {
    return;
  }
  const handle = delayRender('Moving point...');

  const routeDistance = turf.length(turf.lineString(lineCoordinates));

  const progress = interpolate(frame / fps, [0.00001, animationDuration], [0, 1], {
    easing: Easing.inOut(Easing.sin),
    extrapolateLeft: 'clamp',
    extrapolateRight: 'clamp',
  });

  const camera = map.getFreeCameraOptions();

  const alongRoute = turf.along(turf.lineString(lineCoordinates), routeDistance * progress).geometry.coordinates;

  camera.lookAtPoint({
    lng: alongRoute[0],
    lat: alongRoute[1],
  });

  map.setFreeCameraOptions(camera);
  map.once('idle', () => continueRender(handle));
}, [lineCoordinates, fps, frame, handle, map]);
```

注意事項：

重要：預設保持攝影機朝北。
重要：對於多步驟動畫，在所有階段設定所有屬性（縮放、位置、線條進度）以防止跳動。覆蓋初始值。

- 進度被鉗制在最小值以避免線條為空，這可能導致 turf 錯誤
- 參見 [Timing](./timing.md) 了解更多時間選項。
- 考慮合成的尺寸，並將線條設定得足夠粗、標籤字型大小足夠大，以便在合成縮小時仍然清晰可辨。

## 動畫線條

### 直線（線性插值）

要動畫在地圖上顯示為直線的線條，使用座標間的線性插值。不要使用 turf 的 `lineSliceAlong` 或 `along` 函式，因為它們使用大地測量（大圓）計算，在麥卡托投影上會顯示為曲線。

```tsx
const frame = useCurrentFrame();
const {durationInFrames} = useVideoConfig();

useEffect(() => {
  if (!map) return;

  const animationHandle = delayRender('Animating line...');

  const progress = interpolate(frame, [0, durationInFrames - 1], [0, 1], {
    extrapolateLeft: 'clamp',
    extrapolateRight: 'clamp',
    easing: Easing.inOut(Easing.cubic),
  });

  // 線性插值以在地圖上繪製直線
  const start = lineCoordinates[0];
  const end = lineCoordinates[1];
  const currentLng = start[0] + (end[0] - start[0]) * progress;
  const currentLat = start[1] + (end[1] - start[1]) * progress;

  const lineData: GeoJSON.Feature<GeoJSON.LineString> = {
    type: 'Feature',
    properties: {},
    geometry: {
      type: 'LineString',
      coordinates: [start, [currentLng, currentLat]],
    },
  };

  const source = map.getSource('trace') as mapboxgl.GeoJSONSource;
  if (source) {
    source.setData(lineData);
  }

  map.once('idle', () => continueRender(animationHandle));
}, [frame, map, durationInFrames]);
```

### 曲線（大地測量/大圓）

要動畫沿著兩點間大地測量（大圓）路徑的線條，使用 turf 的 `lineSliceAlong`。適用於顯示飛行路線或地球上的實際最短距離。

```tsx
import * as turf from '@turf/turf';

const routeLine = turf.lineString(lineCoordinates);
const routeDistance = turf.length(routeLine);

const currentDistance = Math.max(0.001, routeDistance * progress);
const slicedLine = turf.lineSliceAlong(routeLine, 0, currentDistance);

const source = map.getSource('route') as mapboxgl.GeoJSONSource;
if (source) {
  source.setData(slicedLine);
}
```

## 標記

在適當的位置新增標籤和標記。

```tsx
_map.addSource('markers', {
  type: 'geojson',
  data: {
    type: 'FeatureCollection',
    features: [
      {
        type: 'Feature',
        properties: {name: 'Point 1'},
        geometry: {type: 'Point', coordinates: [-118.2437, 34.0522]},
      },
    ],
  },
});

_map.addLayer({
  id: 'city-markers',
  type: 'circle',
  source: 'markers',
  paint: {
    'circle-radius': 40,
    'circle-color': '#FF4444',
    'circle-stroke-width': 4,
    'circle-stroke-color': '#FFFFFF',
  },
});

_map.addLayer({
  id: 'labels',
  type: 'symbol',
  source: 'markers',
  layout: {
    'text-field': ['get', 'name'],
    'text-font': ['DIN Pro Bold', 'Arial Unicode MS Bold'],
    'text-size': 50,
    'text-offset': [0, 0.5],
    'text-anchor': 'top',
  },
  paint: {
    'text-color': '#FFFFFF',
    'text-halo-color': '#000000',
    'text-halo-width': 2,
  },
});
```

確保它們夠大。檢查合成尺寸並相應調整標籤大小。
對於 1920x1080 的合成尺寸，標籤字型大小至少應為 40px。

重要：保持 `text-offset` 足夠小以使其靠近標記。考慮標記圓的半徑。對於半徑為 40 的圓，這是一個好的偏移量：

```tsx
"text-offset": [0, 0.5],
```

## 3D 建築

要啟用 3D 建築，使用以下程式碼：

```tsx
_map.setConfigProperty('basemap', 'show3dObjects', true);
_map.setConfigProperty('basemap', 'show3dLandmarks', true);
_map.setConfigProperty('basemap', 'show3dBuildings', true);
```

## 渲染

渲染地圖動畫時，請確保使用以下旗標進行渲染：

```
npx remotion render --gl=angle --concurrency=1
```
