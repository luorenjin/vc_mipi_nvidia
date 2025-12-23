中文版基于[原英文文档](VERSION.md) v0.19.0 翻译

# 版本历史

## v0.19.0 (L4T 36.4.3, L4T 32.7.5, 改进)
* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 36.4.3 *(仅限 NVIDIA Jetson Orin NX 和 Orin Nano + Orin Nano 开发套件以及 Auvidea JNX42)*
    * NVIDIA L4T 32.7.5 *(仅限 NVIDIA Jetson Nano + NVIDIA 开发套件, Auvidea JNX30 和 JNX42)* 
* 改进
  * 为 L4T 32.7.5 添加了 v4l 串流的无限超时支持 *(仅限 NVIDIA Jetson Nano)*
  * DRAM 补丁 PCN211181 *(仅限 NVIDIA Jetson Nano)*
  * 在 README.md 中添加了升级警告

## v0.18.3 (L4T 36.4.0, 改进)
* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 36.4.0 *(仅限 NVIDIA Jetson Orin NX 和 Orin Nano + Orin Nano 开发套件以及 Auvidea JNX42)*
* 改进
  * I²C 通信在发生写入错误时会进行重试。
  * IMX252 启动触发流时不再有额外的时间延迟。
  * 为 L4T 35.4.1 和 L4T 36.4.0 添加了 v4l 串流的无限超时支持。
  * 为 L4T 36.2.0 和 L4T 36.4.0 添加了刷机脚本选项 `-m|--module`。
  * 添加了 `frame_rate` 属性的文档。
  * 添加了测试程序的文档。
  * 修改了 README.md 中关于长曝光的描述。

## v0.18.2 (改进)
* 改进
  * 改进了对错误的像素合并/宽度/高度设置的错误处理。
  * 提升了像素合并模式以及像素合并结合帧高缩减时的帧率。
  * 修正了 IMX56x 系列的元数据高度值并添加了文档。
  * 新增对 IMX226 GBRG8 格式的支持。
  * 为 `./setup.sh --target` 添加了默认值 `id_rsa` 和 `nvidia`。

## v0.18.1 (L4T 36.2.0, 支持 IMX900)
* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 36.2.0 *(仅限 NVIDIA Jetson Orin NX + Auvidea JNX42)*
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX900 调整了 Rev02 的数值 (修复曝光 + 移除静态噪声图案)
  * Bug 修复
    * 修复了 L4T 36.2.0 脚本中补丁文件夹的问题。

## v0.18.0 (L4T 36.2.0, L4T 35.4.1, 支持 IMX900, 像素合并)

* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 36.2.0 *(仅限 NVIDIA Jetson Orin NX 和 Orin Nano + Orin Nano 开发套件)*
    * NVIDIA L4T 35.4.1 
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX900 (修复曝光在曝光时间小于帧时间时工作不正常的问题)
  * 新增对像素合并 (Binning) 的支持 *(仅限 IMX412, IMX565, IMX566, IMX567, IMX568)*
* Bug 修复
  * 实现了 L4T 35.4.1 的稳定性补丁。修复了 Nvargus 守护进程在通过 ISP 串流数小时后偶发性卡死的问题。
* 改进
  * 移除了旧版本 L4T 32.3.1 - 32.6.1

## v0.17.1 (L4T 35.3.1)

* Bug 修复
  * 实现了稳定性补丁。修复了 Nvargus 守护进程在通过 ISP 串流数小时后偶发性卡死的问题。

## v0.17.0 (Orin Nano, Orin NX)

* 新特性
  * 新增对核心板 (SoM) 的支持
    * NVIDIA Jetson Orin Nano 8GB/4GB SD/NVME *(仅限 NVIDIA 开发套件和 Auvidea JNX42)*
    * NVIDIA Jetson Orin NX 16GB/8GB NVME *(仅限 Auvidea JNX42)*
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.7.4 *(仅限 NVIDIA Jetson Nano)*
* 改进
  * `setup.sh` 脚本现在可以识别已下载的 L4T 归档文件。
  * 为 NVIDIA Jetson Nano 添加了无限触发超时支持 *(仅限 L4T 32.7.4)*
  * 在设置过程中配置目标设备的量产用户凭据。
  * 调整了 `vc_mod_set_mode` 函数。当模块处于串流模式且格式已设置时，模块不会断电重启。在这种情况下，也会省略额外的等待时间。
  * 扩展了 `./setup.sh --target` 函数：
    * 备份用户的 `known_hosts` 文件。
    * 将 `demo.sh` 和 `max_speed.sh` 脚本直接从 `vc_mipi_nvidia/target/` 目录复制到设备上的 `/home/user/test/` 文件夹。
  * 扩展了 `demo.sh` 和 `max_speed.sh` 脚本。可以通过 `-h` 选项查看这两个脚本的用法。

## v0.16.0 (支持 IMX566, IMX567)

* 新特性
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX566, IMX567
* 改进
  * 为所有摄像头添加了 `io_mode` (闪光灯) 信号的文档。
  * 添加了黑电平 (Black level) 的文档。
* Bug 修复
  * 修复了 NVIDIA Jetson Nano 上 IMX568 的问题。
  * 修复了 IMX335 和 Rev.02 的低帧率问题。
  * 修复了 Pregius S 传感器在使用非标准宽度时的画面撕裂效果。

## v0.15.1 (Bug 修复)

* Bug 修复
  * 修复了 IMX412 缺少从模式 (Slave mode) 设置实现的问题。

## v0.15.0 (JNX42, Bug 修复与改进)

* 新特性
  * 新增对载板的支持
    * Auvidea JNX42 与 NVIDIA Jetson Nano 和 Xavier NX *(JNX42 仅支持一个摄像头 - Jetson Nano, TX2 NX 和 Xavier NX 使用 Cam0)*
* Bug 修复
  * 修复了 `./setup.sh -c` 的问题 (它并非在所有情况下都能加载正确的 DT 文件)。
  * 修正了 README.md 中错误的 `physical_w` 和 `physical_h` 值。
* 改进
  * 简化了构建流水线，只要存在给定 L4T 的目标路径，就会自动复制所有 DT 文件。
  * 构建流水线会检查下载文件的完整性。

## v0.14.1 (Bug 修复)

* Bug 修复
  * 修正了 README.md 和 VERSION.md 中的文档。
  * 修复了 `configure.sh` 中 AGX Xavier 核心板 (SoM) 缺少 DT 文件的问题。

## v0.14.0 (L4T 32.7.3, L4T 35.2.1, 35.3.1)

* 新特性
  * 新增对载板的支持
    * Auvidea JNX30D 与 NVIDIA Jetson Nano 和 Xavier NX
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.7.3
    * NVIDIA L4T 35.2.1 *(仅限 NVIDIA Jetson Xavier NX 和 AGX Xavier)*
    * NVIDIA L4T 35.3.1 *(仅限 NVIDIA Jetson Xavier NX 和 AGX Xavier)*
  * 新增对 VC MIPI 摄像头模块的支持 
    * IMX462
    * IMX565
  * Bug 修复
    * 为 OV9281 添加了触发支持。

## v0.13.0 (L4T 35.1.0)

* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 35.1.0 *(仅限 NVIDIA Jetson Xavier NX 和 AGX Xavier)*

## v0.12.3 (Bug 修复)

* Bug 修复
  * 修复了 IMX183 自触发模式的问题。

## v0.12.2 (Bug 修复)

* Bug 修复
  * 修复了 `vc_fix_image_size` 函数中的一个问题。

## v0.12.1 (改进与 Bug 修复)

* 改进
  * 改进了 ROI 裁剪文档，增加了每个摄像头可调节范围的说明。
  * 在共享文件夹中重新组织了通用的内核补丁。
* Bug 修复
  * 将图像尺寸限制从宽度 32 降低到 4，高度从 4 降低到 1。
  * 修复了 Xavier 和 TX2 核心板 (SoM) 缺失的 `active_l` 和 `active_t` 设备树属性。
  * 修复了异构多摄像头设置中的图像尺寸问题。
  * 在 README.md 中添加了缺失的 `active_l` 和 `active_t` 设备树属性。
  * 修复了新 ROI 裁剪实现中的图像高度规避方案 (workaround)。

## v0.12.0 (通过 V4L 进行 ROI 裁剪)

* 新特性
  * 添加了通过 V4L API 进行 ROI 裁剪的功能。
  * 添加了控制触发和闪光灯信号极性的可能性。
  * 添加了触发模式、IO 模式和 ROI 裁剪的文档。
  * 添加了 V4L2 控件 `single_trigger`。
* 改进
  * 为 IMX412 添加了帧率控制支持。
  * 为 IMX296/IMX297 添加了 ROI 裁剪支持。
* Bug 修复
  * 修复了 IMX290 错误的曝光时间计算。
  * 修复了 IMX290/IMX327 错误的像素格式。
  * 修复了触发模式下计算正确最大曝光时间的问题。
  * 修复了 IMX335 的 vmax 默认值并增加了帧率。

## v0.11.0 (支持 GStreamer, ROI 裁剪及 L4T 32.7.1, 32.7.2)

* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.7.1 (实验性)
    * NVIDIA L4T 32.7.2 (实验性)
  * 添加了对 ROI 裁剪的支持，并在图像高度减小时提高帧率。ROI 裁剪可以通过设备树属性 `active_w` 和 `active_h` 进行设置。
* 改进
  * 针对更好的 GStreamer 支持进行了改进。README.md 现在描述了如何正确设置设备树以获得完整的 `nvarguscamerasrc` 支持。
  * V4L2 控件 `exposure` 和 `frame_rate` 现在会根据像素格式的变化更新其最大值。
* Bug 修复
  * 修复了 IMX335 传感器的黑帧问题。
  * 修复了 `vc_init_image` 中缺失的 Y14 格式处理。
  * 修复了 `VC_MIPI_MANUFACTURER` i2c 地址分配中的不一致问题。

## v0.10.0 (支持 OV7251, IMX297, IMX568)

* 新特性
  * 新增对 VC MIPI 摄像头模块的支持
    * OV7251, IMX297, IMX568
  * 现在也可以为 IMX250, IMX252, IMX264, IMX265, IMX273, IMX392, IMX568 设置黑电平。
  * Bug 修复
    * 修复了一个导致所有 Xavier 和 TX2 平台编译错误的 bug。

## v0.9.0 (支持 IMX335 和 Jetson Nano 2GB)

* 新特性
  * 新增对核心板 (SoM) 的支持
    * NVIDIA Jetson Nano 2GB
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX335
  * 现在也可以为 IMX178 和 IMX226 设置黑电平。
  * Bug 修复
    * 将 IMX178, IMX226 和 IMX335 的默认分辨率更改为符合 NVIDIA 标准的分辨率。

## v0.8.1 (Bug 修复)

* Bug 修复
  * 修复了驱动模块的版本号。
  * 修复了 `STREAM_LEVEL` 触发模式的描述。

## v0.8.0 (支持 Jetson AGX Xavier, TX2)

* 新特性
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.3.1 *(仅限 NVIDIA Jetson AGX Xavier)*
  * 新增对核心板 (SoM) 的支持
    * NVIDIA Jetson AGX Xavier
    * NVIDIA Jetson TX2
  * 新增对载板的支持
    * Auvidea J20 (在 NVIDIA Jetson AGX Xavier 和 TX2 开发套件上) *(仅限连接器 2+3)*
  * 可以通过 V4L2 控件 `frame_rate` 设置帧率 *(IMX412 和 OV9281 除外)*
  * 可以通过 V4L2 控件 `black_level` 设置黑电平 *(仅限 IMX183 和 IMX296)*
* Bug 修复
  * 修复了在自触发模式下更改曝光时间时的一个问题。

## v0.7.1 (改进)

* 改进
  * 在设备树中添加了正确设置 `embedded_metadata_height` 的文档。
  * 优化了设备树中增益和曝光的默认设置，以实现更好的自动曝光控制。
  * 新增对 IMX415 GBRG 像素格式的支持。
  * 优化了 IMX290 和 IMX327 的曝光时间计算。

## v0.7.0 (支持 IMX273, IMX415 和 JNX30 上的 Xavier NX)

* 新特性
  * 新增对载板的支持
    * Auvidea JNX30-LC-PD 与 NVIDIA Jetson Xavier NX
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX273, IMX290, IMX415

## v0.6.0 (支持 IMX250, IMX264, IMX265, IMX392 和 L4T 32.6.1)

* 新特性
  * 添加了此版本描述。
  * 更容易地安装 `demo.sh` 及其依赖项。
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.6.1 (所有核心板)
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX250, IMX264, IMX265, IMX392

## v0.5.0 (支持 IMX412 和 Jetson Xavier NX)

* 新特性
  * 新增对核心板 (SoM) 的支持
    * NVIDIA Jetson Nano (开发套件)
    * NVIDIA Jetson Xavier NX
  * 新增对载板的支持
    * NVIDIA Jetson Xavier NX 开发套件
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.5.0
    * NVIDIA L4T 32.5.2
    * NVIDIA L4T 32.6.1 (仅限 NVIDIA Jetson Nano (量产版))
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX412
  * 快速启动脚本以简化安装过程。
* 已移除特性
  * 移除了对以下板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.3.1
    * NVIDIA L4T 32.4.4

## v0.4.1 (Nano 开发套件上的 L4T 32.5.1)

* 新特性
  * 在 NVIDIA Jetson Nano 开发套件 B01 上新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.5.1

## v0.4.0 (触发和闪光灯模式)

* 新特性
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX178, IMX226, IMX296, OV9281
  * 可以通过设备树或 V4L2 控件 `trigger_mode` 设置触发模式：`0: 禁用`, `1: 外部`, `2: 脉宽`, `3: 自触发`, `4: 单次`, `5: 同步`, `6: 流边缘`, `7: 流电平`。
  * 可以通过设备树或 V4L2 控件 `flash_mode` 设置闪光灯模式：`0: 禁用`, `1: 启用`。

## v0.3.0 (通用驱动)

* 新特性
  * 全新的驱动结构，作为适用于所有 VC MIPI CSI-2 摄像头模块的通用驱动程序。
  * 摄像头模块自动检测。
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX183, IMX252, IMX327

## v0.2.0 (支持 Auvidea JNX30)

* 新特性
  * 新增对载板的支持
    * Auvidea JNX30-LC-PD
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.5.1 (仅限 Auvidea JNX30-LC-PD)

## v0.1.0 (支持 Jetson Nano)

* 新特性
  * 新增对核心板 (SoM) 的支持
    * NVIDIA Jetson Nano (量产版)
  * 新增对板级支持包 (BSP) 的支持
    * NVIDIA L4T 32.3.1
    * NVIDIA L4T 32.4.4
  * 新增对 VC MIPI 摄像头模块的支持
    * IMX183, IMX226, IMX250, IMX252, IMX273, IMX290, IMX296, IMX327, IMX412, IMX415, OV9281
  * 支持 GREY, Y10, Y12, SRGGB8, SRGGB10, SRGGB12 格式的图像串流。
  * 可以通过 V4L2 库设置曝光和增益。
