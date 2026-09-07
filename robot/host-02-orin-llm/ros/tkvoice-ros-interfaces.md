# tkvoice ROS Interfaces

## Nodes / executables

The `audio_service` package provides:

- `tk_audio_publisher`
- `tk_asr_text_publisher`
- `tk_audio_process`

## Topics

The verified voice path uses:

- `audio_frames` — streamed audio frames
- `audio_sentence_frames` — sentence-level audio frames used by ASR
- `/asr_sentence` — recognized speech text

The exact custom message is `audio_message/AudioFrame`.

## Processing responsibilities

### `tk_audio_publisher`

Receives audio from the socket audio provider, parses VAD/frame information, constructs `AudioFrame` messages, and publishes the audio streams.

### `tk_asr_text_publisher`

Consumes sentence audio, calls FunASR, and publishes recognized text on `/asr_sentence`.

### `tk_audio_process`

Consumes recognized speech, sends requests to the LLM client, streams response sentence chunks, sends them through Piper TTS, and plays them in order.

## Engineering observation

The voice pipeline is therefore a ROS-integrated orchestration layer around external audio, ASR, LLM, and TTS services rather than a single monolithic speech process.
