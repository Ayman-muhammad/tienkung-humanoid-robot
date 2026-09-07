# Host 02 — ORIN_LLM Reverse-Engineering Notes

This directory documents the verified software and interface findings for Walker TienKung Host 2 (ORIN_LLM).

## Host identity

- Role: ORIN_LLM / AI and audio domain
- Platform: NVIDIA Jetson AGX Orin Developer Kit
- OS: Ubuntu 22.04.4 LTS
- Architecture: aarch64 / arm64
- Kernel: `5.15.148-tegra`
- ROS 2: Humble
- Host address observed during investigation: `192.168.41.2`

## Current voice baseline

The working voice-conversation stack is documented as:

```text
Microphone / robot audio subsystem
        |
        v
TCP audio stream: 10.42.0.127:9080
        |
        v
tk_audio_publisher
        |
        v
ROS 2 AudioFrame
   |                    \
   |                     \
   v                      v
audio_frames      audio_sentence_frames
                         |
                         v
                 tk_asr_text_publisher
                         |
                         v
                 FunASR WebSocket
                 192.168.41.1:10097
                         |
                         v
                    /asr_sentence
                         |
                         v
                   tk_audio_process
                         |
                         v
                    LLMClient
                         |
                         v
                Ollama-compatible API
                       :11434
                         |
                         v
                       Qwen
                         |
                         v
                  Piper TTS provider
                         |
                         v
                    AudioPlayer
                         |
                         v
                     speaker
```

## Important engineering notes

- `tkvoice` is the current voice baseline.
- The LLM client probes `192.168.41.3` before `192.168.41.2` for an Ollama endpoint, so the active LLM host should be verified at runtime rather than hard-coded in documentation.
- The default model names in `LLMClient` are `qwen2.5:7b`, subject to runtime model resolution.
- The older `walker_llm_bridge` is experimental and is not the baseline architecture.
- Lyre/AIUI exists on the robot but is treated as an alternative/legacy path unless live runtime evidence shows that it is part of the active tkvoice conversation.

## Publication boundary

This repository intentionally excludes credentials, private keys, certificates, machine identifiers, model weights, Hugging Face caches, sensitive runtime logs, and vendor-essential secrets or configuration.
