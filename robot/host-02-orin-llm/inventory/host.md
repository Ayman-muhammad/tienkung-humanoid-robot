# Host 02 Inventory

## Platform

- Host role: ORIN_LLM
- IP observed: `192.168.41.2`
- Ubuntu: 22.04.4 LTS
- Architecture: aarch64 / arm64
- Kernel: `5.15.148-tegra`
- ROS 2: Humble
- Hardware: NVIDIA Jetson AGX Orin Developer Kit

## Relevant software identified during reverse engineering

- ROS 2 Humble
- tkvoice
- Ollama-compatible LLM endpoint
- Qwen local model deployment
- Piper TTS
- FunASR client integration
- llama.cpp installation
- Foxglove tooling
- Legacy/alternative Lyre voice stack

## tkvoice workspace

The Host 2 `~/tkvoice` workspace contains source, build, install, and launch material for the audio service. The source tree is itself a Git repository.

Important source packages identified include:

- `audio_message`
- `audio_service`

## Publication boundary

Machine IDs, boot IDs, credentials, private keys, certificates, model files, Hugging Face caches, and sensitive logs are deliberately omitted.
