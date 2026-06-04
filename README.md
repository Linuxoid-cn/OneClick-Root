# RootByOneClick

[![Platform](https://img.shields.io/badge/Platform-Android-green.svg)](https://www.android.com/)
[![Shell](https://img.shields.io/badge/Script-Bash-blue.svg)](https://www.gnu.org/software/bash/)
[![KernelSU](https://img.shields.io/badge/Core-KernelSU-orange.svg)](https://github.com/tiann/KernelSU)

## 📌 项目简介 / Description

**RootByOneClick** 是一个专为 Android 玩家打造的自动化 Root 辅助工具，由本人独立开发完成。

本工具的核心脚本 `toRoot.sh` 旨在彻底终结繁琐的搞机流程。它将复杂的运行环境检测、无线连接配对、内核版本匹配以及 KernelSU 等多种管理器 镜像修补刷入等多个离散的步骤完美串联，带你告别枯燥的命令行，实现真正省心、安全的向导式“一键”刷入体验。

- **GitHub 简介：** 支持跨平台运行环境自适应，集成了完善的 ADB/Fastboot 超时重试机制与安全退出保护，让内核修补与镜像刷入流程更高效、更安全。

---

## 🚀 核心特性 / Features

- **🌐 跨平台环境自适应**：智能识别当前运行环境（Windows MINGW/MSYS、Linux、Android）与架构（x86_64、arm64、armv7），动态匹配底层二进制库。
- **📶 双模式连接支持**：完美支持传统有线 ADB 调试与现代无线调试（包含首次连接的配对码鉴权向导）。
- **🛡️ 工业级容错与重试**：针对 ADB 与 Fastboot 连接设计了双层循环重试机制（每 2 秒检测，单轮 10 次，最多允许 3 大轮排查重试），彻底告别因驱动加载慢、断连导致的脚本崩溃。
- **🔒 兜底安全退出保护**：在 Fastboot 刷入阶段，若发生任何不可预知的错误，脚本会自动触发 `fastboot continue` 指令尝试引导手机正常开机，最大程度防砖。

---

## 📂 目录结构 / Directory Structure

在运行本脚本前，请确保您的储存库保持以下目录结构：

```text
RootByOneClick/
├── toRoot.sh                 # 主运行脚本 (核心控制逻辑)
├── init_boot.img             # 您手机的原厂官方 init_boot 镜像（需提前准备）
├── bin/                      # 底层工具二进制库
│   ├── x86_64/
│   │   └── ksud              # PC 端 / Linux x86_64 的 ksud 核心
│   ├── arm64-v8a/
│   │   └── ksud              # 手机端 arm64 的 ksud 核心
│   └── armeabi-v7a/
│       └── ksud              # 手机端 armv7 的 ksud 核心
└── ko/                       # 内核驱动模块目录
    └── <android-version>-<kernel-version>_kernelsu.ko  # 与设备严格匹配的 .ko 文件
