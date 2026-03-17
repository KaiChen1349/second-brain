---
name: transcribe-captions
description: Transcribing audio to generate captions in Remotion
metadata:
  tags: captions, transcribe, whisper, audio, speech-to-text
---

# 轉錄音訊

Remotion 提供了多種內建選項來轉錄音訊以產生字幕：

- `@remotion/install-whisper-cpp` - 使用 Whisper.cpp 在伺服器上本機轉錄。速度快且免費，但需要伺服器基礎設施。
  https://remotion.dev/docs/install-whisper-cpp

- `@remotion/whisper-web` - 使用 WebAssembly 在瀏覽器中轉錄。不需要伺服器且免費，但由於 WASM 開銷較慢。
  https://remotion.dev/docs/whisper-web

- `@remotion/openai-whisper` - 使用 OpenAI Whisper API 進行雲端轉錄。速度快且不需要伺服器，但需要付費。
  https://remotion.dev/docs/openai-whisper/openai-whisper-api-to-captions
