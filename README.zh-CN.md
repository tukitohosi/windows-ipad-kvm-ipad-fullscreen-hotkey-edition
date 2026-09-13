# Windows iPad KVM——iPad 全屏操控快捷键版

[English](README.md) | 简体中文

通过 ESP32-C3 蓝牙 HID 桥，让一套 Windows 键盘和鼠标直接操控 iPad。将鼠标从 Windows 桌面右侧移出即可进入 iPad。本版本不会在 iPad 左边缘自动返回，因此可以操控完整的 iPad 画面；按 `Ctrl+Alt+Esc` 返回 Windows。

`Windows 输入 -> Python 主程序 -> USB 串口 -> ESP32-C3 -> 蓝牙 HID -> iPad`

## 功能

- 从 Windows 桌面右边缘进入 iPad。
- 不设置自动返回区域，完整保留 iPad 全屏可操作范围。
- 使用 `Ctrl+Alt+Esc` 返回；`Scroll Lock` 作为备用返回键。
- 转发鼠标移动、五个鼠标按键、滚轮和键盘输入。
- Windows/GUI 键和 Alt 键分别对应 iPad Command 和 Option 修饰键。
- iPad 未连接时拒绝进入远端；USB、蓝牙或输入捕获发生故障时释放 Windows 输入。
- iPad 无需安装常驻 App，也不依赖云服务。

## 环境要求

- Windows 10 或 Windows 11。
- Python 3.11 或更高版本。
- ESP32-C3 开发板和支持数据传输的 USB 线。
- iPadOS 13.4 或更高版本。
- 用于构建和刷写固件的 PlatformIO。

## 安装与使用

1. 安装固件构建工具，并构建、刷写 ESP32-C3：

   ```powershell
   py -m pip install platformio
   cd firmware
   pio run
   pio run --target upload --upload-port COM3
   cd ..
   ```

2. 在 iPad 蓝牙设置中配对 `MouseLink-iPad`。
3. 创建 Windows Python 环境：

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```

4. 启动桥接程序：

   ```powershell
   .\.venv\Scripts\python.exe host\bridge.py
   ```

5. 将鼠标移动到 Windows 桌面右边缘进入 iPad；按 `Ctrl+Alt+Esc` 返回 Windows。如果键盘能够提供独立的 `Scroll Lock` 键，也可以将其作为备用方式。

程序会自动查找 Espressif 串口设备。如果没有检测到开发板，请检查 USB 数据线、COM 端口、固件以及 iPad 蓝牙连接。

## 重要说明

- 本版本特意关闭 iPad 左边缘自动返回。
- 只有进入 iPad 模式后才会屏蔽 Windows 本地输入。
- 中文通过 iPad 自带输入法输入；Windows 输入法的候选及组合状态不会直接传递。
- 本项目不包含屏幕投射、文件拖放或跨设备剪贴板同步。
- 请始终保留 `Ctrl+Alt+Esc` 作为返回 Windows 的主要方式。

## 源码结构

- `host/bridge.py`：Windows 输入路由和故障恢复。
- `firmware/`：ESP32-C3 蓝牙 HID 固件和 PlatformIO 配置。
- `tools/hid_diagnostic.py`：独立 HID 诊断工具。

## 许可证与来源

本项目派生自 [KMChris/esp32-kvm](https://github.com/KMChris/esp32-kvm)，开发时固定在提交 `99c52bc36128867d2a7ed85417c1043a7d06ed5c`。详见 [LICENSE](LICENSE) 和 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

