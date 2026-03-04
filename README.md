# 使用前必需仔细阅读注意事项，否则无法使用
## This solution is only for Chinese version computers that cannot properly drive Intel network cards, and making an English version of the readme is not considered for now

# 网卡驱动只针对此列表内的网卡
 [link](https://openintelwireless.github.io/itlwm/Compat.html) 原装网卡为瑞昱的机型不支持，建议额外购买黑苹果免驱网卡

# lenovo-thinkpad-l430 
## Monterey 12.6.5
![lenovo thinkpad l430](https://res.cloudinary.com/dk0053zbe/image/upload/v1683217467/monterey_gbnihv.png)

## 测试机型硬件信息 :
- Opencore版本 1.0.6
- 测试主机CPU型号 i7 3720QM (Ivy Bridge)
- 测试主机集成GPU型号 intel hd 4000
- 音频：audio alc269 layout-id 29
- 以太（有线）网卡 RTL8168E-VL
- 无线网卡Intel(R) Centrino(R) Advanced-N 6205
- Intel 7 Series Chipset
- Touchpad ELAN

## 运行情况 :
- GPU intel HD 4000 (需要手动使用Opencore Legacy Patcher工具安装补丁)
- USB接口
- 声卡Alc269
- 触摸板
- etc
- 关机功能
- 重启功能
- 来自主仓库的电源管理Must be set SMBIOS MacBookPro12,1 to be install Monterey (on instalations) and SET to MacBookPro10.2 with boot args no_compat_check (POST Istall) to fixing Power management Issue in ivy bridge Monterey (SSDT-PM.aml generated from catalina)(！详见后文中的安装说明！)
- 亮度快捷键, Fn + p (亮度加) and Fn + k (亮度减)
- 无线网卡 Intel(R) Centrino(R) Advanced-N 6205(不完美，无法实现原生支持)
- 不完美的睡眠(慎用)

## 不工作 :
- VGA port(底层原因无法解决)

## Patch SSDT(建议遵照教程或官方文档根据自己的硬件使用工具手动编译，替换除快捷键支持外的SSDT文件):
- PNLF (Fix Brightness)
- PM (CPU power management) (POST Install)
- EC (Fixes the embedded controller)
- XOSI (Makes all _OSI calls specific to Windows work for macOS (Darwin) Identifier. This may help enabling some features like XHCI and others.)
- IMEI (Needed to add a missing IMEI device on Ivy Bridge)
- HPET (Fixing IRQ Conflicts) https://dortania.github.io/Getting-Started-With-ACPI/Universal/irq.html
- SBUS-MCHC fix SMBUS
- SSDT-THINK (enable fan control YOGA SMC kext) (POST INSTALL)
- SSDT-RCSM (virtual power control YOGA SMC) (POST Install) 
- SSDT-ECRW (EC Reading YOGA SMC) (POST Install)


# 由Catalina升级的重要注意事项（必须由Catalina升级）（以下事项并行，请完整阅读把握大致信息后逐条设置）
- 请先使用来自主分支的Catalina版EFI安装Catalina版系统 [link](https://github.com/yaza-putu/lenovo-thinkpad-l430/)
- 在系统内安装 [heliport](https://github.com/OpenIntelWireless/HeliPort/) 以连接无线网络，详情见 [link](https://openintelwireless.github.io/)
- 由于网卡驱动原因，建议使用 [聪聪黑苹果工具](https://b23.tv/BV1iE41157Vd)  安装Catalina系统，常规的恢复镜像安装系统的方式会无法联网下载
## 1 安装显卡驱动（苹果已经放弃了对HD 4000显卡的支持，所以在运行补丁前，确保在config.plist中设置）：
- 进入 Misc/Security，找到 SecureBootModel 条目并将其设置为禁用
- 进入NVRAM/Add/7C436110-AB2A-4BBB-A880-FE41995C9F82，找到csr-active-config，设置为030A0000
- 重启后运行 [Opencore Legacy Patcher](https://dortania.github.io/OpenCore-Legacy-Patcher/) 安装驱动补丁
## 2 完善电源管理
- 你在升级macOS Monterey之前需要将SMBIOS改为MacBookPro12,1（先移除ssdt-pm.aml），可以使用 [OCAT](https://github.com/ic005k/OCAuxiliaryTools/) 工具进行更改
- 安装完成后，你需要将 ssdt-pm.aml 复制到 ACPI 文件夹，并将 SMBIOS 设置为 MacBookPro10.2，以实现完美的电源管理


# 较原仓库的改动
- 由AirportItlwm.kext改动为itlwm.kext  V2.3.0 解决中国版Thinkpad L430 使用AirportItlwm.kext时点击网络设置按钮后系统100%死机的问题
- 更新open core版本
- 一些杂项设置适配国行版电脑
- 更加详细的安装步骤和注意事项


# Bug
- 睡眠不完美，不建议使用此功能，以免丢失重要数据
- Wifi不完美需额外安装itlwm.kext配套GUI软件(黑苹果系统内)才能连接无线网，系统顶栏的原生Wifi标识无法隐藏
