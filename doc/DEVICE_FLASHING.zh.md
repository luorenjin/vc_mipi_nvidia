中文版基于[原英文文档](DEVICE_FLASHING.md) v 0.19.0 翻译

# 设备刷机

## 初始刷机
为了使用您选择的 Linux4Tegra 发行版，必须将其刷入目标设备。<br>
调用
```
./flash.sh --all
```
将执行目标设备的初始刷机。

如果已执行快速入门脚本，`flash.sh` 脚本将在构建过程完成后自动调用。<br>
快速入门脚本的简化结构：
```
  quickstart.sh
    ./setup.sh --host
    ./build.sh --all
    ./flash.sh --all
```

如果您在没有使用 `quickstart.sh` 脚本的情况下设置了系统，则必须在构建过程后手动调用初始刷机程序。

对于初始刷机，目标设备必须处于强制恢复模式（forced recovery mode）。<br>
[Quick Start Guide L4T 35.3.1](https://docs.nvidia.com/jetson/archives/r35.3.1/DeveloperGuide/text/IN/QuickStart.html) (NVIDIA Jetson Orin Nano, Orin NX, Xavier NX and AGX Xavier) <br>
[Quick Start Guide L4T 32.7.3](https://docs.nvidia.com/jetson/archives/l4t-archived/l4t-3273/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/quick_start.html) (NVIDIA Jetson Nano, TX2, Xavier NX and AGX Xavier)

## 刷机问题

如果遇到刷机过程不成功的问题，首先应检查 USB 线缆的完整性，或尽可能尝试使用主机系统上的其他 USB 端口。<br>
### Orin 设备
特别是对于 Orin 设备，可能会遇到一些困难，例如超时、空间不足或连接断开。
1. 在为 Orin 设备刷机时，您的内存卡/NVMe 卡应至少有 64 GB 空间。

2. 您应该检查主机上是否正在运行 udisks2 服务：
   ```
   service --status-all
   ```
   如果 udisks2 服务已启动，则应尝试将其暂时停用。请确保已卸载其他驱动器以防止数据丢失。
   ```
   systemctl stop udisks2.service
   ```
3. 您应该检查 USB 自动挂起（autosuspend）是否已激活：
   ```
   cat /sys/module/usbcore/parameters/autosuspend
   ```
   如果结果为 2，则应停用自动挂起功能：
   ```
   sudo -s
   echo -1 > /sys/module/usbcore/parameters/autosuspend
   exit
   ```
4. 在 L4T 35.4.1 中，为刷机脚本引入了一个新的环境变量。如果您遇到提示存储设备可能有问题（特别是磁盘空间不足，即使使用了 64GB 或更大容量）的错误，您可以将存储设备挂载到主机系统并检查可用扇区数：
   ```
   sudo fdisk -l /dev/sda
   ```
   （如果您的 SD 卡挂载为 sda）<br>
   您应该会看到设备的扇区数，例如：121536512。使用获得的数字，您可以尝试再次为 Jetson 设备刷机：
   ```
   sudo ./flash.sh --all EXT_NUM_SECTORS="121536512"
   ```


## 设备树刷机

如果您的系统已成功刷机，并且您想修改摄像头设置（例如通道数），则应执行以下步骤：
1. 修改摄像头设置
   ```
   ./setup.sh --camera
   ```

2. 编译设备树
   ```
   ./build.sh --dt
   ```
3. 刷入设备树
   ```
   ./flash.sh --dt
   ```

对于仅刷入设备树的情况，必须考虑 JetPack 版本：

### JetPack 4 (L4T 32.7.1 - L4T 32.7.4)

这适用于 Jetson Nano、Jetson Xavier NX 和 Jetson AGX Xavier。
设备必须处于强制恢复模式！只需调用：
```
./flash.sh --dt
```

### JetPack 5 (L4T 35.1.0 - L4T 35.4.1)

 对于仅更改设备树，必须区分 Orin 和非 Orin 目标设备：
  * 对于像 Xavier NX 和 AGX Xavier 这样的目标设备，您必须修改目标机器上的 `/boot/extlinux/extlinux.conf`，删除 FDT 条目或使用 '#' 将其注释掉。否则，您必须为每次设备树更改重新刷入完整的 Linux 镜像才能生效。

    ```bash
    # FDT /boot/dtb/kernel_tegra194-p3668-0000-p3509-0000.dtb
    ```

    设备必须处于强制恢复模式！
  * 对于 Jetson Orin Nano 和 Jetson Orin NX，FDT 条目必须存在于 `/boot/extlinux/extlinux.conf` 文件中。`./flash -d` 命令会将相应的文件（例如 NVIDIA DevKit 上 Orin Nano 8GB 的 `tegra234-p3767-0003-p3768-0000-a0.dtb`）复制到 `/boot/dtb/` 目录中。
  
    因此，必须重命名 `extlinux.conf` 的 FDT 条目，例如：<br>
    从

    ```bash
    FDT /boot/dtb/kernel_tegra234-p3767-0003-p3768-0000-a0.dtb 
    ```

    修改为

    ```bash
    FDT /boot/dtb/tegra234-p3767-0003-p3768-0000-a0.dtb
    ```

    设备必须处于运行状态。**不能**处于强制恢复模式！

### JetPack 6 (L4T 36.2.0)

这适用于 Jetson Orin Nano 和 Jetson Orin NX。对摄像头设备树的修改通过设备树叠加层（device tree overlays）实现。这意味着可以通过一个或多个叠加层文件（称为 dtbo）来修改完整的设备树。

当通过调用
```
./setup.sh --camera
```
修改摄像头设置时，`tegra234-p3767-camera-p3768-vc_mipi-dual.dts` 文件将被修改。

构建步骤
```
./build.sh --dt
```
将在主机 PC 的 `kernel/dtb` 目录中生成 `tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo` 文件。

该叠加层文件已在初始刷机过程中写入 UEFI，并可以通过在 `extlinux.conf` 文件中添加 `OVERLAYS` 标签来覆盖。
要覆盖 UEFI dtbo，请将主机 PC 的 `kernel/dtb` 文件夹中新生成的 `tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo` 复制到 Jetson Orin 目标设备的 `/boot/dtb` 目录中。
因此，您应该修改 `/boot/extlinux/extlinux.conf` 文件，方法是在主启动配置中添加以下条目：
```
      OVERLAYS /boot/tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo
```
或者复制整个启动条目：
```
LABEL secondary
      MENU LABEL secondary kernel
      LINUX /boot/Image
      FDT /boot/dtb/kernel_tegra234-p3768-0000+p3767-0005-nv.dtb
      INITRD /boot/initrd
      APPEND ...

      OVERLAYS /boot/tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo
```
dtbo 文件必须在重启前复制到指定位置。

### JetPack 6 (L4T 36.4.0)

更改或应用新 dtbo 的过程与 [L4T 36.2.0](/doc/DEVICE_FLASHING.md#jetpack-6-l4t-3620) 基本相同，但 `/boot/extlinux/extlinux.conf` 部分中的 FDT 条目缺失。因此，必须将 FDT 和 OVERLAYS 这两个条目都添加到该部分，如下所示：

<pre>
LABEL secondary
      MENU LABEL secondary kernel
      LINUX /boot/Image
      FDT /boot/dtb/kernel_tegra234-p3768-0000+p3767-<b>0000</b>-nv.dtb
      INITRD /boot/initrd
      APPEND ...

      OVERLAYS /boot/tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo
</pre>
