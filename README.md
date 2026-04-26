> [!注意]
> 以下内容为官方readme.md文件的内容。
> 翻译者:来自 [Open—Code—Studio](https://github.com/Open-code-Studio/)—— 的 [仓颉](https://github.com/cangcang-zh-2025)

<br/>
<div align="center">
  <h3 align="center">OpCore Simplify</h3>

  <p align="center">
    一个专业的工具，用于简化 <a href="https://github.com/acidanthera/OpenCorePkg">OpenCore</a> EFI 创建过程。
    <br />
    <br />
    <a href="#-features">功能</a> •
    <a href="#-how-to-use">使用方法</a> •
    <a href="#-contributing">贡献代码</a> •
    <a href="#-license">许可证</a> •
    <a href="#-credits">贡献者</a> •
    <a href="#-contact">联系我们</a>
  </p>

  <p align="center">
    <a href="https://trendshift.io/repositories/15410" target="_blank"><img src="https://trendshift.io/api/badge/repositories/15410" alt="lzhoang2801%2FOpCore-Simplify | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>
  </p>
</div>

> [!注意]
> **OpenCore 旧机型修补器 3.0.0 – 现已支持 macOS Tahoe 26！**
>
> 期待已久的 OpenCore 旧机型修补器 3.0.0 版本现已发布，为社区带来了对 **macOS Tahoe 26 的初步支持**！
>
> 🚨 **请注意:**
>
> - 只有来自 **[lzhoang2801/OpenCore-Legacy-Patcher](https://github.com/lzhoang2801/OpenCore-Legacy-Patcher/releases/tag/3.0.0)** 仓库的 OpenCore-Patcher 3.0.0 提供对 macOS Tahoe 26 的早期补丁支持。
> - 官方的 Dortania 版本或旧的补丁 **将无法** 在 macOS Tahoe 26 上工作。

> [!警告]
> 虽然 OpCore Simplify 显著减少了设置时间，但 Hackintosh 的过程仍然需要：
>
> - 理解来自 [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/) 的基本概念。
> - 在安装过程中进行测试和故障排除。
> - 有耐心和毅力，解决任何问题。

我们的工具不能保证第一次安装成功，但它可以帮助你开始。

## ✨ **功能**

1. **全面的硬件和 macOS 支持**\
   完全支持现代硬件。使用 `兼容性检查器` 检查支持的/不支持的设备和 macOS 版本。

| **支持组件**    | **支持**                                                                                                                                                                            |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CPU**     | 英特尔：Nehalem 和 Westmere（第一代）→ Arrow Lake（第十五代/核心超系列 2）  AMD：Ryzen 和 Threadripper 搭配 [AMD Vanilla](https://github.com/AMD-OSX/AMD_Vanilla)                                          |
| **GPU**     | IIntel iGPU：Iron Lake（第一代）→ Ice Lake（第十代）AMD APU：整个Vega Raven ASIC系列（Ryzen 1xxx → 5xxx，7x30系列）AMD dGPU：Navi 23、Navi 22、Navi 21世代，以及更早系列NVIDIA：Kepler、Pascal、Maxwell、Fermi、Tesla世代 |
| **MacOS版本** | macOS High Sierra → macOS Tahoe                                                                                                                                                   |

1. **ACPI 修补程序和内核扩展**\
   根据硬件配置自动检测并添加 ACPI 补丁和 kext。
   - 集成了 [SSDTTime](https://github.com/corpnewt/SSDTTime) 用于常见补丁（例如 FakeEC、FixHPET、PLUG、RTCAWAC）。
   - 包括自定义补丁：
     - 禁用不受支持或未使用的 PCI 设备，例如 GPU（使用 Optimus 和 Bumblebee 方法或添加 disable-gpu 属性）、Wi-Fi 卡和 NVMe 存储控制器。
     - 修复 \_PRW 方法（GPRW、UPRW、HP special）中的睡眠状态值以防止立即唤醒。
     - 添加设备，包括 ALS0、BUS0、MCHC、PMCR、PNLF、RMNE、IMEI、USBX、XOSI，以及 Surface 补丁。
     - 启用 ALSD 和 GPI0 设备。
2. **自动更新**\
   在每次 EFI 构建之前，自动检查并更新 OpenCorePkg 和 kexts，来源于 [Dortania Builds](https://dortania.github.io/builds/) 和 GitHub 发布。
3. **EFI 配置**
   根据广泛使用的资料和个人经验，进行额外的定制化。
   - 伪造某些 AMD GPU 在 macOS 中未被识别的 GPU ID。
   - 使用 CpuTopologyRebuild kext 用于支持 P 核和 E 核的 Intel CPU 以提升性能。
   - 禁用系统完整性保护（SIP）。
   - 针对Intel Pentium、Celeron、Core和Xeon处理器的伪造CPU ID。
   - 为AMD处理器以及Rocket Lake（第11代）及以后更新一代的Intel Pentium、Celeron、Xeon和Core系列添加自定义CPU名称。
   - 添加补丁以允许在不支持的 SMBIOS 上启动 macOS。
   - 添加 NVRAM 条目以绕过内部蓝牙控制器的检查。
   - 根据特定的可调整 BAR（Resizable BAR）信息正确配置 ResizeAppleGpuBars。
   - 在存在支持的独立 GPU 时，允许在无头模式和驱动显示器之间灵活配置 iGPU。
   - 强制将 Intel GPU 设置为带 HDMI 和 DVI 接口的 VESA 模式，以简化安装过程。
   - 提供使用 OpenCore Legacy Patcher 所需的配置。
   - 为网络设备添加内置设备属性（修复使用 iServices 时出现的“无法与服务器通信”问题）和存储控制器（修复内部驱动器显示为外部驱动器的问题）。
   - 优先使用同时优化电源管理和性能的 SMBIOS。
   - 在 macOS Ventura 13 及更高版本中重新启用对旧款 Intel CPU 的 CPU 电源管理。
   - 为 itlwm kext 应用 WiFi 配置文件，以便在启动时自动连接 WiFi。
     以及更多…
4. **轻松自定义**\
   除了应用默认设置之外，用户如有需要可以轻松进行进一步的自定义。
   - 自定义 ACPI 修补、kext 和 SMBIOS 调整（**不推荐**）。
   - 在不支持的 macOS 版本上强制加载 kext。

## 🚀 **How To Use**

1. **下载 OpCore Simplify**：
   - 点击 **Code** → **Download ZIP**，或者通过这个[链接](https://github.com/lzhoang2801/OpCore-Simplify/archive/refs/heads/main.zip)直接下载。
   - 将下载的 ZIP 文件解压到您想要的位置。![下载 OpCore Simplify](https://i.imgur.com/mcE7OSX.png)
2. **运行 OpCore Simplify**：
   - 在 **Windows** 上，运行 `OpCore-Simplify.bat`。
   - 在 **macOS** 上，运行 `OpCore-Simplify.command`。
   - 在 **Linux** 上，使用已有的 Python 解释器运行 `OpCore-Simplify.py`。
   ![OpCore Simplify 菜单](https://i.imgur.com/vTr1V9D.png)
3. **选择硬件报告**：
   - 在 Windows 上，会有一个选项 `E. 导出硬件报告`。建议使用此选项，以获取在构建时与您的硬件配置和 BIOS 最匹配的最佳结果。
   - 或者，可以使用 **[Hardware Sniffer](https://github.com/lzhoang2801/Hardware-Sniffer)** 来创建 `Report.json` 和 ACPI 转储，以便手动配置。
   ![选择硬件报告](https://i.imgur.com/MbRmIGJ.png)
   ![加载 ACPI 表](https://i.imgur.com/SbL6N6v.png)
   ![兼容性检查器](https://i.imgur.com/kuDGMmp.png)

4. **选择 macOS 版本并自定义 OpenCore EFI**：
   - 默认情况下，会为您的硬件选择最新兼容的 macOS 版本。
   - OpCore Simplify 会自动应用必要的 ACPI 补丁和 kext。
   - 您可以根据需要手动查看并自定义这些设置。
   ![OpCore Simplify 菜单](https://i.imgur.com/TSk9ejy.png)
5. **构建 OpenCore EFI**：
   - 一旦自定义了所有选项，选择 **Build OpenCore EFI** 来生成你的 EFI。
   - 工具会自动下载所需的引导程序和 kexts，这可能需要几分钟时间。
   ![WiFi 配置提取器](https://i.imgur.com/71TkJkD.png)
   ![选择 Codec Layout ID](https://i.imgur.com/Mcm20EQ.png)
   ![构建 OpenCore EFI](https://i.imgur.com/deyj5de.png)
6. **USB 映射**：
   - 构建 EFI 后，按照步骤进行 USB 端口映射。
   ![结果](https://i.imgur.com/MIPigPF.png)
   7. **创建 USB 并安装 macOS**：
   - 在 Windows 上使用 **[UnPlugged](https://github.com/corpnewt/UnPlugged)** 创建 macOS 安装 USB，或按照 [此指南](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html) 在 macOS 上进行。
   - 如需排障，请参考 [OpenCore 排障指南](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/troubleshooting.html)。

> [! 注]
>
> 1.安装成功后，如果需要OpenCore Legacy Patcher，只需应用根补丁即可激活缺失的功能（如现代Broadcom Wi-Fi卡和图形加速）。
> 2.对于 AMD 显卡，在从 OpenCore Legacy Patcher 应用根补丁后，你需要移除启动参数 '-radvesa'/'-amd_no_dgpu_accel'，这样图形加速才会正常。

## 🤝 **贡献**

非常感谢任何贡献！如果你有改进这个项目的想法，欢迎分支仓库并创建一个拉取请求，或者用“增强”标签开启一个问题。

别忘了给⭐项目加星号！感谢你的支持！ 🌟

## 📜 **许可证**

根据BSD 3条款许可证分发。更多信息请参见“LICENSE”。
## 🙌 **致谢**

   - [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) 和 [kexts](https://github.com/lzhoang2801/OpCore-Simplify/blob/main/Scripts/datasets/kext_data.py) – 本项目的核心基础。
   - [SSDTTime](https://github.com/corpnewt/SSDTTime) – SSDT 修补工具。

## 📞 **联系方式**

**Hoang Hong Quan**

> Facebook [@macforce2601](https://facebook.com/macforce2601)  · 
> Telegram [@lzhoang2601](https://t.me/lzhoang2601)  · 
> 邮箱: <lzhoang2601@gmail.com>

## 🌟 **星标历史**

[![星标历史图表](https://api.star-history.com/svg?repos=lzhoang2801/OpCore-Simplify&type=Date)](https://star-history.com/#lzhoang2801/OpCore-Simplify&Date)
