# ASUS ROG Zephyrus G14 GA402RJ OpenCore EFI

适用于 ASUS ROG Zephyrus G14（2022，GA402RJ）的 OpenCore EFI 分享副本。

## EFI 内容

EFI 包含 OpenCore、启动文件、ACPI 补丁及驱动。配置中启用了 AirportItlwm、IntelBluetoothFirmware 等 Intel 无线网卡相关驱动；驱动的存在不代表所有功能或所有 macOS 版本均已验证。实际兼容性与 BIOS、macOS 版本及硬件配置有关。

此副本不包含 Windows / Ubuntu 启动配置、旧配置或原始 VBIOS 备份。

## 使用前

在 `EFI/OC/config.plist` 的 `PlatformInfo` 中，将以下空值或零值替换为自己的设备标识：

| 配置路径 | 需要填写 |
| --- | --- |
| `DataHub → SystemSerialNumber` / `SystemUUID` | 系统序列号 / UUID |
| `Generic → SystemSerialNumber` / `SystemUUID` / `MLB` / `ROM` | 系统序列号 / UUID / 主板序列号 / 6 字节 ROM |
| `PlatformNVRAM → SystemSerialNumber` / `SystemUUID` / `MLB` / `ROM` | 系统序列号 / UUID / 主板序列号 / 6 字节 ROM |
| `SMBIOS → SystemSerialNumber` / `SystemUUID` / `BoardSerialNumber` / `ChassisSerialNumber` | 系统序列号 / UUID / 主板序列号 / 机箱序列号 |

使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 生成 `MacBookPro16,3` 的机型信息，按上表填写；同名字段填相同的值，`BoardSerialNumber` 填 `MLB`，`ChassisSerialNumber` 可填系统序列号。

## 本机配置

| 项目 | 配置 |
| --- | --- |
| BIOS 版本 | 319 |
| 处理器 | AMD Ryzen 7 6800HS，8 核 16 线程 |
| 核心显卡 | AMD Radeon 680M |
| 独立显卡 | AMD Radeon RX 6700S |
| 内存 | 40 GB，8 GB + 32 GB，4800 MHz |
| 硬盘 | Micron 2450 1 TB NVMe SSD，PCIe x4 |
| 屏幕 | 14 英寸，2560 × 1600，120 Hz；面板 TL140ADXP01 |
| 无线网卡 | Intel AX210，已由原 MediaTek（MTK）网卡更换 |
| OpenCore 机型 | MacBookPro16,3 |

## 已工作的组件

| 组件 | 状态 | 说明 |
| --- | --- | --- |
| AMD Ryzen 7 6800HS | ✅ | 正常 |
| AMD Radeon RX 6700S | ✅ | PCI 伪装 + X6000 原生驱动 |
| AMD Radeon 680M | ✅ | Lilu Hook + X6000 原生驱动 |
| DDR5 4800 MHz 内存 | ✅ | 正常 |
| Micron 2450 1 TB NVMe SSD | ✅ | 正常 |
| Intel AX210 Wi-Fi | ✅ | 可用，但不太稳定 |
| Intel AX210 蓝牙 | ✅ | 正常 |
| 电池 | ✅ | 正常 |
| 屏幕亮度控制 | ✅ | 正常，原生控制 |
| 睡眠 | ✅ | S3 模式下正常 |
| ALC285 音频 | ✅ | 正常，可使用 [eqMac](https://eqmac.app/) 增强音效 |
| 触控板 | ✅ | 正常 |
| 键盘 | ✅ | 正常 |
| Fn 键控制 | ✅ | 正常，使用 R-Helper 控制 |

以上为当前系统观察到的状态；其他组件和功能尚未逐项验证。

## macOS 版本

- macOS Sequoia 15：EFI 配置包含对应的无线网卡驱动，未验证启动。
- macOS Tahoe 26：本机当前运行 26.7.2。

## 如何使用独显

1. 在 Windows 的 Armoury Crate 中将 GPU 模式设为 **Ultimate（独显直连）**，按提示重启。
2. EFI 已对 RX 6700S 进行 PCI 设备 ID 伪装，可使用 macOS 自带的 AMD Radeon X6000 驱动，无需 NootRX。
3. 重启后进入 macOS，即可使用独显。可在“系统信息 → 图形卡/显示器”中检查显卡和 Metal 支持。

## 如何使用核显

1. 在 Windows 的 Armoury Crate 中将 GPU 模式设为 **Eco（节能）**，然后重启。
2. 本 EFI 已启用 `Lilu.kext` 和 `AMDRadeonRembrandtHook.kext`。首次启动测试成功后，在 macOS 终端执行以下命令以持续启用核显驱动：

   ```bash
   sudo nvram '7C436110-AB2A-4BBB-A880-FE41995C9F82:AMDRadeonRembrandtHookEnabled=%01'
   ```

3. 在“系统信息 → 图形卡/显示器”中检查核显和 Metal 支持。
4. 更多说明请参阅 [AMDRadeonRembrandtHook](https://github.com/SaKongA/AMDRadeonRembrandtHook)。

## 感谢

本 EFI 基于 [ttpro7/ZephyrusG14-2022-Hackintosh](https://github.com/ttpro7/ZephyrusG14-2022-Hackintosh) 修改，非常感谢原作者。
