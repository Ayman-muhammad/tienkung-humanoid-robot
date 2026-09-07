# Host 2 Runtime Snapshot

Snapshot evidence collected on Walker TienKung Host 2 (`192.168.41.2`, ORIN_LLM).

## Current baseline

- Ubuntu 22.04.4 LTS, arm64/aarch64
- NVIDIA Jetson AGX Orin Developer Kit
- Linux kernel `5.15.148-tegra`
- ROS 2 Humble environment
- Host IPs observed: `192.168.41.2`, `10.42.0.1`, and a dynamic `172.20.x.x` Wi-Fi address
- Ollama server process observed: `/usr/bin/ollama serve`
- Ollama reported server version `0.17.7` and client version `0.33.3`

## ROS actuator interfaces

- `/head/cmd_pos` -> `bodyctrl_msgs/msg/CmdSetMotorPosition`
- `/head/cmd_vel` -> `bodyctrl_msgs/msg/CmdSetMotorSpeed`
- `/head/status` -> `bodyctrl_msgs/msg/MotorStatusMsg`
- `/arm/cmd_pos` -> `bodyctrl_msgs/msg/CmdSetMotorPosition`
- `/arm/cmd_vel` -> `bodyctrl_msgs/msg/CmdSetMotorSpeed`
- `/arm/status` -> `bodyctrl_msgs/msg/MotorStatusMsg`
- `/leg/cmd_pos` -> `bodyctrl_msgs/msg/CmdSetMotorPosition`
- `/leg/cmd_vel` -> `bodyctrl_msgs/msg/CmdSetMotorSpeed`
- `/leg/status` -> `bodyctrl_msgs/msg/MotorStatusMsg`
- `/waist/cmd_pos` -> `bodyctrl_msgs/msg/CmdSetMotorPosition`
- `/waist/cmd_vel` -> `bodyctrl_msgs/msg/CmdSetMotorSpeed`
- `/waist/status` -> `bodyctrl_msgs/msg/MotorStatusMsg`
- `/inspire_hand/ctrl/left_hand` -> `sensor_msgs/msg/JointState`
- `/inspire_hand/ctrl/right_hand` -> `sensor_msgs/msg/JointState`
- `/hric/robot/cmd_vel` -> `geometry_msgs/msg/TwistStamped`

## Custom message definitions captured

`CmdSetMotorPosition` contains `header` and `SetMotorPosition[] cmds` with `name`, `pos`, `spd`, and `cur`.

`CmdSetMotorSpeed` contains `header` and `SetMotorSpeed[] cmds` with `name`, `spd`, and `cur`.

`MotorStatusMsg` contains `header` and `MotorStatus[] status` with `name`, `pos`, `speed`, `current`, `temperature`, and `error`.

## Live state baseline

A read-only state capture reported:

- Head motors: IDs 1, 2, 3; all `error: 0`
- Arm motors: IDs 11-17 and 21-27; all `error: 0`
- Leg motors: IDs 51-56 and 61-66; all `error: 0`
- Waist motor: ID 31; `error: 0`
- Left and right hands: six joint states each
- IMU: `error: 0`
- Battery telemetry was available and reported active battery/power status

## tkvoice evidence

The snapshot captured the installed/source tree for `~/tkvoice`, including its ROS package metadata, build/install scripts, Docker FunASR resources, Ollama resources, and Python package files.

The working voice architecture established separately is:

`audio subsystem -> TCP 10.42.0.127:9080 -> tk_audio_publisher -> AudioFrame -> tk_asr_text_publisher -> FunASR 192.168.41.1:10097 -> /asr_sentence -> tk_audio_process -> Ollama/Qwen -> Piper -> AudioPlayer -> speaker`

## Important boundary

This is an evidence snapshot, not a motion command log. No actuator command was published during the capture.

The GitHub copy intentionally excludes secrets, private keys, credentials, machine/boot IDs, model weights, caches, and generated artifacts that are not required for reverse-engineering documentation.
