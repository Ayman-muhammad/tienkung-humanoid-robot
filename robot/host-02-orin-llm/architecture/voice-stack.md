# Verified Voice Stack

## Data flow

```text
Audio subsystem
  -> TCP 10.42.0.127:9080
  -> tk_audio_publisher
  -> AudioFrame ROS messages
  -> tk_asr_text_publisher
  -> FunASR WebSocket 192.168.41.1:10097
  -> /asr_sentence
  -> tk_audio_process
  -> LLMClient
  -> Ollama-compatible HTTP API :11434
  -> Qwen
  -> streamed sentence chunks
  -> PiperProvider
  -> AudioPlayer
  -> speaker
```

## tkvoice executables

The `audio_service` package defines these ROS executables:

- `tk_audio_publisher`
- `tk_audio_process`
- `tk_asr_text_publisher`

## Audio publisher

`tk_audio_publisher` uses a socket audio provider and publishes custom `AudioFrame` messages. The observed provider endpoint is:

- IP: `10.42.0.127`
- TCP port: `9080`
- sample rate: 16000 Hz by default
- channels: 1
- sample width: 2 bytes

It publishes `audio_frames` and `audio_sentence_frames`.

## ASR

`tk_asr_text_publisher` consumes sentence-level `AudioFrame` messages and sends audio to FunASR through WebSocket.

Observed endpoint:

- Host: `192.168.41.1`
- Port: `10097`

Recognized text is published to `/asr_sentence`.

## LLM

`LLMClient` discovers an Ollama-compatible server by testing:

1. `192.168.41.3:11434`
2. `192.168.41.2:11434`

It checks `/api/version` and then `/api/tags` to resolve an available model. The OpenAI-compatible client uses `/v1/` and streaming chat completions.

Default model environment values are `qwen2.5:7b`, but runtime model resolution is authoritative.

## TTS

`tk_audio_process` uses `PiperProvider` to convert streamed LLM sentence chunks to audio and `AudioPlayer` to play the resulting segments in order.

## Safety boundary

This document records interfaces and architecture only. It does not include credentials, private keys, certificates, model weights, or sensitive runtime data.
