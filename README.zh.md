中文版基于[原英文文档](README.md) v0.19.0 翻译

# Vision Components MIPI CSI-2 驱动程序，适用于 NVIDIA Jetson Nano, Xavier NX, AGX Xavier, TX2, Orin Nano 和 Orin NX

![VC MIPI 摄像头](doc/images/mipi_sensor_front_back.png)

## 版本 0.19.0 ([历史记录](VERSION.md))

* 支持的系统模块 (SoM)
  * [NVIDIA Jetson Nano 4GB/2GB (正式版 + 开发套件)](https://developer.nvidia.com/embedded/jetson-nano)
  * [NVIDIA Jetson Xavier NX (正式版 + 开发套件)](https://developer.nvidia.com/embedded/jetson-xavier-nx)
  * [NVIDIA Jetson AGX Xavier](https://developer.nvidia.com/embedded/jetson-agx-xavier-developer-kit)
  * [NVIDIA Jetson TX2](https://developer.nvidia.com/embedded/jetson-tx2-developer-kit)
  * [NVIDIA Jetson TX2 NX](https://developer.nvidia.com/embedded/jetson-tx2-nx)
  * [NVIDIA Jetson Orin Nano 8GB/4GB](https://developer.nvidia.com/embedded/jetson-modules#jetson_orin_nano)
  * [NVIDIA Jetson Orin NX 16GB/8GB](https://developer.nvidia.com/embedded/jetson-modules#jetson_orin_nx)
* 支持的载板
  * [NVIDIA Jetson Nano Developer Kit B01](https://developer.nvidia.com/embedded/jetson-nano-developer-kit)
  * [NVIDIA Jetson Nano 2GB Developer Kit](https://developer.nvidia.com/embedded/jetson-nano-2gb-developer-kit)
  * [NVIDIA Jetson Xavier NX Developer Kit](https://developer.nvidia.com/embedded/jetson-xavier-nx-devkit)
  * [NVIDIA Jetson Orin Nano Developer Kit](https://developer.nvidia.com/embedded/learn/get-started-jetson-orin-nano-devkit)
  * [Auvidea JNX30/JNX30D](https://auvidea.eu/product/70879)
  * [Auvidea JNX42 LM](https://auvidea.eu/product/70784) *(仅限 NVIDIA Jetson Nano, Xavier NX, Orin Nano, Orin NX)*
  * [Auvidea J20 (配合 Devkit Jetson AGX Xavier 或 TX2 使用)](https://auvidea.eu/j20/) *(仅限连接器 2+3)*
* 支持的板级支持包 (BSP)
  * [NVIDIA L4T 32.7.1](https://developer.nvidia.com/embedded/linux-tegra-r3271)
  * [NVIDIA L4T 32.7.2](https://developer.nvidia.com/embedded/linux-tegra-r3272)
  * [NVIDIA L4T 32.7.3](https://developer.nvidia.com/embedded/linux-tegra-r3273)
  * [NVIDIA L4T 32.7.4](https://developer.nvidia.com/embedded/linux-tegra-r3274) *(仅限 NVIDIA Jetson Nano)*
  * [NVIDIA L4T 32.7.5](https://developer.nvidia.com/embedded/linux-tegra-r3275) *(仅限 NVIDIA Jetson Nano)*
  * [NVIDIA L4T 35.1.0](https://developer.nvidia.com/embedded/jetson-linux-r351) *(仅限 NVIDIA Jetson Xavier NX 和 AGX Xavier)*
  * [NVIDIA L4T 35.2.1](https://developer.nvidia.com/embedded/jetson-linux-r3521) *(仅限 NVIDIA Jetson Xavier NX, AGX Xavier 和 Orin NX)*
  * [NVIDIA L4T 35.3.1](https://developer.nvidia.com/embedded/jetson-linux-r3531) *(仅限 NVIDIA Jetson Xavier NX, AGX Xavier, Orin NX 和 Orin Nano)*
  * [NVIDIA L4T 35.4.1](https://developer.nvidia.com/embedded/jetson-linux-r3541) *(仅限 NVIDIA Jetson Xavier NX, AGX Xavier, Orin NX 和 Orin Nano)*
  * [NVIDIA L4T 36.2.0](https://developer.nvidia.com/embedded/jetson-linux-r362) *(仅限 NVIDIA Jetson Orin Nano 和 Orin NX)*
  * [NVIDIA L4T 36.4.0](https://developer.nvidia.com/embedded/jetson-linux-r3640) *(仅限 NVIDIA Jetson Orin Nano 和 Orin NX)*
  * [NVIDIA L4T 36.4.3](https://developer.nvidia.com/embedded/jetson-linux-r3643) *(仅限 NVIDIA Jetson Orin Nano 和 Orin NX)*
* 支持的 [VC MIPI 摄像头模块](https://www.vision-components.com/fileadmin/external/documentation/hardware/VC_MIPI_Camera_Module/index.html) 
  * IMX178, IMX183, IMX226
  * IMX250, IMX252, IMX264, IMX265, IMX273, IMX392
  * IMX290, IMX327, IMX462
  * IMX296, IMX297
  * IMX335
  * IMX412
  * IMX415
  * IMX565, IMX566, IMX567, IMX568
  * IMX900
  * OV7251, OV9281
* 功能特性
  * 快速入门脚本，简化安装过程
  * 自动检测 VC MIPI 摄像头模块
  * 支持 GREY, Y10, Y12, SRGGB8, SRGGB10, SRGGB12, SGBRG8, SGBRG10, SGBRG12 格式的图像流
  * **曝光 (Exposure)** 可通过 V4L2 控制项 'exposure' 进行设置
  * **增益 (Gain)** 可通过 V4L2 控制项 'gain' 进行设置
  * **[触发模式 (Trigger mode)](doc/TRIGGER_MODE.md)** '0: 禁用', '1: 外部触发', '2: 脉冲宽度触发', '3: 自触发', '4: 单次触发', '5: 同步触发', '6: 流边缘触发', '7: 流电平触发'，可通过设备树或 V4L2 控制项 'trigger_mode' 设置
    * **软件触发** 可通过 V4L2 控制项 'single_trigger' 执行
  * **[IO 模式 (IO mode)](doc/IO_MODE.md)** '0: 禁用', '1: 闪光灯高电平有效', '2: 闪光灯低电平有效', '3: 触发低电平有效', '4: 触发低电平有效且闪光灯高电平有效', '5: 触发和闪光灯均低电平有效'，可通过设备树或 V4L2 控制项 'flash_mode' 设置
  * **[帧率 (Frame rate)](doc/FRAME_RATE.md)** 可通过 V4L2 控制项 'frame_rate' 设置 *(OV9281 除外)*
  * **[黑电平 (Black level)](doc/BLACK_LEVEL.md)** 可通过 V4L2 控制项 'black_level' 设置 *(OV7251 和 OV9281 除外)*
  * **[ROI 裁剪 (ROI cropping)](doc/ROI_CROPPING.md)** 可通过设备树属性 active_l, active_t, active_w 和 active_h 或 v4l2-ctl 设置。
  * **[分级模式 (Binning mode)](doc/BINNING_MODE.md)** 可通过 V4L2 控制项 'binning_mode' 设置 *(仅限 IMX412, IMX565, IMX566, IMX567, IMX568)*

## 交叉编译准备工作

### 主机 PC

* 推荐操作系统：Ubuntu 18.04 LTS 或 Ubuntu 20.04 LTS
* 需要使用 git 克隆此仓库
* 所有其他依赖包将由本仓库中包含的脚本自动安装

## 快速入门

1. 按照以下指南之一的说明进入恢复模式：

   [L4T 35.3.1 快速入门指南](https://docs.nvidia.com/jetson/archives/r35.3.1/DeveloperGuide/text/IN/QuickStart.html) (NVIDIA Jetson Orin Nano, Orin NX, Xavier NX 和 AGX Xavier)

   [L4T 32.7.3 快速入门指南](https://docs.nvidia.com/jetson/archives/l4t-archived/l4t-3273/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/quick_start.html) (NVIDIA Jetson Nano, TX2, Xavier NX 和 AGX Xavier)

2. 创建目录并克隆仓库。

    ```bash
    cd <working_dir>
    git clone https://github.com/VC-MIPI-modules/vc_mipi_nvidia
    ```

3. 启动快速入门安装进程并按照说明进行操作。

   ```bash
   cd vc_mipi_nvidia/bin
   ./quickstart.sh
   ```

   > 如果您更改了硬件设置，只需再次执行此脚本即可。

   > 在执行 quickstart.sh 脚本的设置过程中，将调用 NVIDIA 的 l4t_create_default_user.sh 脚本，该脚本将根据用户提供的信息创建一个默认用户。
   > * 设置完工具链后，系统会提示用户输入这些凭据。
   > * 该脚本自 L4T 32.6.1 起可用，并将进行相应应用。

4. (可选)

   在目标设备已运行的情况下，通过在主机计算机上执行以下命令来设置目标设备：

   ```bash
   cd vc_mipi_nvidia/bin
   ./setup.sh --target
   ```

   此功能将配置与运行中的目标设备的连接。它会备份用户的 known_hosts 文件并复制用户的公钥 (public rsa key)，这样每次开启到设备的 ssh 连接时，用户都无需再次输入密码。它还会将 demo.sh 和 max_speed.sh 脚本复制到设备上的 /home/username/test/ 文件夹中。

   有关上述脚本的更多信息，请在运行中的设备 shell 中运行带有 "--help" 选项的相应脚本。

   ```bash
   vc@nvidia $./test/demo.sh --help
   ```

   或者

   ```bash
   vc@nvidia $ ./test/max_speed.sh --help
   ```

   在使用 max_speed.sh 脚本时，应以 root 权限运行。<br> 例如，提高时钟频率：

   ```bash
   vc@nvidia $ sudo ./test/max_speed.sh --max
   ```

## 在设备树中更改摄像头设置

### 嵌入式元数据高度 (Embedded Metadata Height)

某些串流工具需要调整嵌入式元数据高度才能正常工作。</br>
如果元数据大小错误，v4l2-ctl 将无法工作，而其他串流应用程序虽然可能正在串流，但会缺少时间戳信息。<br>

在已配置的载板/核心板 (som) 组合的设备树包含文件中，有一个名为 **VC_MIPI_METADATA_H** 的参数。</br>
此 #define 必须根据您连接的传感器进行设置。如果您板卡上连接了不同的传感器，它们可能需要不同的元数据高度值。在这种情况下，您应该直接在所连接传感器的 modeX 节点中修改 **embedded_metadata_height** 参数的值。</br>
嵌入式元数据高度参数适用于几乎所有的核心板，但 Jetson Nano 除外。

您可以使用如下命令检查运行系统中配置的嵌入式元数据高度参数：
<pre>
cat /proc/device-tree/<b>cam_i2cmux</b>/i2c@<b>0</b>/vc_mipi@<b>1a</b>/mode0/embedded_metadata_height
</pre>
路径中加粗的部分可能会根据您配置的载板/核心板/传感器组合而有所不同。<br> 
您可以在配置的 dtsi 文件的 moduleX 节点的 drivernode0 部分中找到此路径，具体在 *proc-device-tree* 变量（Jetpack 4/5）或 *sysfs-device-tree* 变量（Jetpack 6）中。

### GStreamer 支持

如果您想将 GStreamer 与 nvarguscamerasrc 一起使用，必须调整设备树中的某些属性。为此，请按照本节中的说明进行操作。对于每个摄像头，设备树中都有一个 mode0 节点。此节点中有一个额外的注释，用于标记您需要自定义的属性。在下表中，您可以找到每个摄像头的具体数值。

属性 *pixel_t* 的值列出了支持的像素格式。您必须从下表中选择一个。

| pixel_t | RAW08 值      | RAW10 值     | RAW12 值     |
| ------- | ------------ | ------------ | ------------ |
| RGGB    | bayer_rggb8  | bayer_rggb   | bayer_rggb12 |
| GBRG    | bayer_gbrg8  | bayer_gbrg   | bayer_gbrg12 |

属性 *max_framerate* 是根据通道数 (lanes) 和像素格式给出的。例如，4L10 代表 4 通道和 RAW10 像素格式。请始终将 *def_framerate* 设置为与 *max_framerate* 相同的值。

<details>
  <summary>IMX296, IMX297, OV7251 的 GStreamer 属性（仅支持 1 通道的摄像头）</summary>

| 属性                 | IMX296     | IMX297     | OV7251     |
| -------------------- | ---------: | ---------: | ---------: |
| physical_w           |      4.968 |      4.968 |      1.920 |
| physical_h           |      3.726 |      3.726 |      1.440 |
| active_w             |       1440 |        720 |        640 |
| active_h             |       1080 |        540 |        480 |
| pixel_t              |      RG 10 |      RG 10 |    RG 8,10 |
| max_gain_val         |         48 |         48 |         18 |
| step_gain_val        |      0.100 |      0.100 |      0.018 |
| max_framerate (1L08) |          - |          - |      104.0 |
| max_framerate (1L10) |       60.3 |      120.8 |      104.0 |
| max_framerate (1L12) |          - |          - |          - |
</details>

<details>
  <summary>IMX264, IMX265, OV9281 的 GStreamer 属性（仅支持 2 通道的摄像头）</summary>

| 属性                 | IMX264     | IMX265     | OV9281     |
| -------------------- | ---------: | ---------: | ---------: |
| physical_w           |      8.390 |      7.065 |      3.840 |
| physical_h           |      7.066 |      5.299 |      2.400 |
| active_w             |       2432 |       2048 |       1280 |
| active_h             |       2048 |       1536 |        800 |
| pixel_t              | RG 8,10,12 | RG 8,10,12 |    RG 8,10 |
| max_gain_val         |         48 |         48 |         12 |
| step_gain_val        |      0.100 |      0.100 |      0.050 |
| max_framerate (2L08) |       35.5 |       55.3 |      120.6 |
| max_framerate (2L10) |       35.5 |       55.3 |      120.6 |
| max_framerate (2L12) |       35.5 |       55.3 |          - |
</details>

<details>
  <summary>IMX178, IMX183, IMX226 的 GStreamer 属性（支持 2 和 4 通道的摄像头）</summary>

| 属性                 | IMX178     | IMX183     | IMX226     |
| -------------------- | ---------: | ---------: | ---------: |
| physical_w           |      7.373 |     13.056 |      7.222 |
| physical_h           |      4.915 |      8.755 |      5.550 |
| active_w             |       3072 |       5440 |       3904 |
| active_h             |       2048 |       3648 |       3000 |
| pixel_t              | RG 8,10,12 | RG 8,10,12 | GB 8,10,12 |
| max_gain_val         |         48 |         27 |         27 |
| step_gain_val        |      0.100 |      0.026 |      0.014 |
| max_framerate (2L08) |       51.3 |       13.4 |       21.8 |
| max_framerate (2L10) |       41.6 |       13.4 |       21.8 |
| max_framerate (2L12) |       35.4 |       11.2 |       18.1 |
| max_framerate (4L08) |       58.2 |       26.8 |       43.6 |
| max_framerate (4L10) |       58.2 |       26.8 |       43.6 |
| max_framerate (4L12) |       51.3 |       22.4 |       36.3 |
</details>

<details>
  <summary>IMX250, IMX252, IMX273, IMX392 的 GStreamer 属性（支持 2 和 4 通道的摄像头）</summary>

| 属性                 | IMX250     | IMX252     | IMX273     | IMX392     |
| -------------------- | ---------: | ---------: | ---------: | ---------: |
| physical_w           |      8.390 |      7.066 |      4.968 |      6.624 |
| physical_h           |      7.066 |      5.299 |      3.726 |      4.140 |
| active_w             |       2432 |       2048 |       1440 |       1920 |
| active_h             |       2048 |       1536 |       1080 |       1200 |
| pixel_t              | RG 8,10,12 | RG 8,10,12 | RG 8,10,12 | RG 8,10,12 |
| max_gain_val         |         48 |         48 |         48 |         48 |
| step_gain_val        |      0.100 |      0.100 |      0.100 |      0.100 |
| max_framerate (2L08) |       65.7 |      102.0 |      195.6 |      132.4 |
| max_framerate (2L10) |       53.7 |       83.8 |      156.5 |      111.9 |
| max_framerate (2L12) |       45.5 |       69.8 |      136.9 |       95.0 |
| max_framerate (4L08) |      101.3 |      151.4 |      276.0 |      201.7 |
| max_framerate (4L10) |       82.5 |      123.5 |      226.5 |      167.0 |
| max_framerate (4L12) |       69.5 |      105.7 |      165.9 |      134.4 |
</details>

<details>
  <summary>IMX290, IMX327, IMX335, IMX412, IMX415 和 IMX462 的 GStreamer 属性（支持 2 和 4 通道的摄像头）</summary>

| 属性                 | IMX290/327 | IMX335     | IMX412     | IMX415     | IMX462     |
| -------------------- | ---------: | ---------: | ---------: | ---------: | ---------: |
| physical_w           |      5.568 |      5.184 |      6.250 |      5.568 |      5.568 |
| physical_h           |      3.132 |      3.888 |      4.712 |      3.132 |      3.132 |
| active_w             |       1920 |       2592 |       4032 |       3840 |       1920 |
| active_h             |       1080 |       1944 |       3040 |       2160 |       1080 |
| pixel_t              |      RG 10 |   RG 10,12 |      RG 10 |      GB 10 |      RG 10 |
| max_gain_val         |         71 |         72 |         51 |         72 |         71 |
| step_gain_val        |      0.300 |      0.300 |      0.050 |      0.300 |      0.300 |
| max_framerate (2L08) |          - |          - |          - |          - |          - |
| max_framerate (2L10) |       60.0 |       15.0 |       20.0 |       31.7 |       60.0 |
| max_framerate (2L12) |          - |       15.0 |          - |          - |          - |
| max_framerate (4L08) |          - |          - |          - |          - |          - |
| max_framerate (4L10) |       60.0 |       22.3 |       40.0 |       59.9 |      120.0 |
| max_framerate (4L12) |          - |       22.3 |          - |          - |          - |
</details>

<details>
  <summary>IMX565, IMX566, IMX567, IMX568 和 IMX900 的 GStreamer 属性（支持 2 和 4 通道的摄像头）</summary>

| 属性                 | IMX565     | IMX566     | IMX567/568 | IMX900     |
| -------------------- | ---------: | ---------: | ---------: | ---------: |
| physical_w           |     11.311 |      7.804 |      6.752 |      4.608 |
| physical_h           |      8.220 |      7.804 |      5.655 |      3.456 |
| active_w             |       4128 |       2848 |       2464 |       2048 |
| active_h             |       3000 |       2848 |       2064 |       1536 |
| pixel_t              | RG 8,10,12 | RG 8,10,12 | RG 8,10,12 | RG 8,10,12 |
| max_gain_val         |         48 |         48 |         48 |         48 |
| step_gain_val        |      0.100 |      0.100 |      0.100 |      0.100 |
| max_framerate (2L08) |       21.1 |       33.3 |       49.8 |       88.8 |
| max_framerate (2L10) |       17.0 |       26.9 |       41.3 |       72.5 |
| max_framerate (2L12) |       14.2 |       22.6 |       34.6 |       61.3 |
| max_framerate (4L08) |       40.7 |       68.2 |       96.2 |      121.0 |
| max_framerate (4L10) |       34.3 |       51.6 |       78.8 |      112.3 |
| max_framerate (4L12) |       27.8 |       43.6 |       66.7 |       67.0 |

</details>

### 示例

作为示例，代码片段中展示了具有 4 通道和 RAW10 像素格式的 IMX226 的设备树。请注意，增益 (gain) 的属性值以 mdB [:)] 为单位，帧率以 mHz 为单位。因此，您必须将表中的数值乘以 1000。

```dts
  ...
  // ----------------------------------------------------
  // 如果您想将 GStreamer 与 nvarguscamerasrc 一起使用，
  // 您必须调整这些设置
  physical_w              = "7.222";
  physical_h              = "5.550";
  // ----------------------------------------------------

  // 此节点是 Tegra 框架所需的。
  // 如果您只想使用 V4L API，则无需更改任何设置。
  mode0 {
      ...

      // ----------------------------------------------------
      // 如果您想将 GStreamer 与 nvarguscamerasrc 一起使用，
      // 您必须调整这些设置。
      active_l                 = "0";
      active_t                 = "0";
      active_w                 = "3904";
      active_h                 = "3000";
#if LINUX_VERSION < 500
      pixel_t                  = "bayer_gbrg";
#else
      mode_type                = "bayer";
      pixel_phase              = "gbrg";
      csi_pixel_bit_depth      = "10";
#endif

      min_gain_val             = "0";         //     0.0 dB
      max_gain_val             = "27000";     //    27.0 dB
      step_gain_val            = "14";        //   0.014 dB
      default_gain             = "0";         //     0.0 dB
      
      min_exp_time             = "1";         //       1 us
      max_exp_time             = "1000000";   // 1000000 us
      step_exp_time            = "1";         //       1 us
      default_exp_time         = "10000";     //   10000 us

      min_framerate            = "1000";      //       1 Hz
      max_framerate            = "43600";     //    43.6 Hz
      step_framerate           = "100";       //     0.1 Hz
      default_framerate        = "43600";     //    43.6 Hz
      // ----------------------------------------------------
      ...
```

## 设备树文件

如果您想在设备树中更改摄像头的某些设置，请按照以下步骤操作。

1. 根据您的硬件设置编辑设备树文件。 
   | 系统核心模块 (SoM) | 载板 | 设备树文件 |
   | ---------------- | ------------- | ---------------- |
   | NVIDIA Jetson Nano | NVIDIA Jetson Nano Developer Kit | src/devicetree/NV_DevKit_Nano/tegra210-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Nano | Auvidea JNX30 | src/devicetree/Auvidea_JNX30_Nano/tegra210-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Nano | Auvidea JNX42 | src/devicetree/Auvidea_JNX42_Nano/tegra210-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Xavier NX | NVIDIA Jetson Xavier NX Developer Kit | src/devicetree/NV_DevKit_XavierNX/tegra194-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Xavier NX | Auvidea JNX30 | src/devicetree/Auvidea_JNX30_XavierNX/tegra194-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Xavier NX | Auvidea JNX42 | src/devicetree/Auvidea_JNX42_XavierNX/tegra194-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson AGX Xavier | Auvidea J20 (配合 DevKit 使用) | src/devicetree/Auvidea_J20_AGXXavier/tegra194-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson TX2 | Auvidea J20 (配合 DevKit 使用) | src/devicetree/Auvidea_J20_TX2/tegra186-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson TX2 NX | Auvidea JNX30D | src/devicetree/Auvidea_JNX30D_TX2NX/tegra186-camera-vc-mipi-cam.dtsi |
   | NVIDIA Jetson Orin Nano | NVIDIA Jetson Orin Nano Developer Kit | src/devicetree/NV_DevKit_OrinNano/tegra234-camera-vc-mipi-cam.dtsi <br> Jetpack 5 (L4T 35.3.1, L4T 35.4.1)|
   | NVIDIA Jetson Orin Nano | NVIDIA Jetson Orin Nano Developer Kit | src/devicetree/NV_DevKit_OrinNano/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> Jetpack 6 (L4T 36.x.y)|
   | NVIDIA Jetson Orin Nano | Auvidea JNX42 | src/devicetree/Auvidea_JNX42_OrinNano/tegra234-camera-vc-mipi-cam.dtsi <br> Jetpack 5 (L4T 35.3.1, L4T 35.4.1) |
   | NVIDIA Jetson Orin Nano | Auvidea JNX42 | src/devicetree/NV_DevKit_OrinNano/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> Jetpack 6 (L4T 36.x.y)|
   | NVIDIA Jetson Orin NX | NVIDIA Jetson Orin Nano Developer Kit | src/devicetree/NV_DevKit_OrinNano/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> Jetpack 6 (L4T 36.x.y)|
   | NVIDIA Jetson Orin NX | Auvidea JNX42 | src/devicetree/Auvidea_JNX42_OrinNX/tegra234-camera-vc-mipi-cam.dtsi <br> Jetpack 5 (L4T 35.2.1, L4T 35.3.1, L4T 35.4.1)|
   | NVIDIA Jetson Orin NX | Auvidea JNX42 | src/devicetree/Auvidea_JNX42_OrinNX/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> Jetpack 6 (L4T 36.x.y)|

   您可以使用 setup 脚本轻松编辑正确的设备树文件。它将在 nano 编辑器中打开对应的设备树文件。

   ```bash
     ./setup.sh --camera
   ```

2. 按照以下指南之一的说明进入恢复模式：<br>
[L4T 35.3.1 快速入门指南](https://docs.nvidia.com/jetson/archives/r35.3.1/DeveloperGuide/text/IN/QuickStart.html) (NVIDIA Jetson Orin Nano, Orin NX, Xavier NX 和 AGX Xavier) <br>
[L4T 32.7.3 快速入门指南](https://docs.nvidia.com/jetson/archives/l4t-archived/l4t-3273/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/quick_start.html) (NVIDIA Jetson Nano, TX2, Xavier NX 和 AGX Xavier)

3. 编译并将设备树文件烧录到目标设备。

   ```bash
     ./build.sh --dt
     ./flash.sh --dt
   ```

   > 关于 Jetpack 5 及更高版本，请参阅“注解 (Annotations)”部分。<br>
   > **[烧录设备 (Flashing the device)](doc/DEVICE_FLASHING.md)** 

## 使用长曝光时间或具有长等待时间（> 5 秒）的外部触发模式

如果您想在具有长曝光时间或外部触发的应用中使用摄像头，且两次连续触发之间的时间可能很长（> 5 秒），则需要调整 CSI 接收器的超时时间。<br>

由于不同的 Jetson 设备使用不同的视频输入（VI2、VI4 和 VI5），因此必须在相应的位置进行调整。<br>

   | 视频输入 | 文件       | 位置                             | 调用方式                                    |
   | ----------- | ---------- | ------------------------------------ | --------------------------------------- |
   | VI2         | vi2_fops.c | 函数 tegra_channel_ec_init       | chan->timeout = msecs_to_jiffies(5000); |
   | VI4         | vi4_fops.c | 函数 vi4_channel_start_streaming | chan->timeout = msecs_to_jiffies(5000); |
   | VI5         | vi5_fops.c | 宏定义                               | #define CAPTURE_TIMEOUT_MS	5000         |

下表列出了 Jetson 设备及其对应的文件：
   | 系统核心模块 (SoM)         | 对应文件                                                          |
   | ------------------------ | ---------------------------------------------------------------- |
   | NVIDIA Jetson Nano       | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi2_fops.c |
   | NVIDIA Jetson TX2        | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi4_fops.c |
   | NVIDIA Jetson TX2 NX     | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi4_fops.c |
   | NVIDIA Jetson Xavier NX  | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi5_fops.c |
   | NVIDIA Jetson AGX Xavier | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi5_fops.c |
   | NVIDIA Jetson Orin Nano  | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi5_fops.c |
   | NVIDIA Jetson Orin NX    | /kernel/nvidia/drivers/media/platform/tegra/camera/vi/vi5_fops.c |

  > 请注意，较高的超时值可能会导致从单次或外部触发模式停止流时产生较长的等待惩罚。<br>

## 已测试的 VC MIPI 摄像头模块版本 (Revision)

* IMX178 (Rev.02), IMX183 (Rev.15), IMX226 (Rev.16), 
* IMX250 (Rev.09), IMX252 (Rev.15), IMX264 (Rev.05), IMX265 (Rev.05), IMX273 (Rev.16), IMX392 (Rev.08)
* IMX290 (Rev.02), IMX327 (Rev.02), IMX462 (Rev.01)
* IMX296 (Rev.43), IMX297 (Rev.43)
* IMX335 (Rev.02)
* IMX412 (Rev.05)
* IMX415 (Rev.02)
* IMX565 (Rev.03), IMX566 (Rev.03), IMX567 (Rev.03), IMX568 (Rev.04)
* IMX900 (Rev.02)
* OV7251 (Rev.01), OV9281 (Rev.02)

您可以在 dmesg 日志中找到摄像头模块的版本。

```bash
dmesg | grep 'i2c'
[...] i2c 6-0010: +--- VC MIPI Camera -----------------------------------+
[...] i2c 6-0010: | MANUF. | Vision Components               MID: 0x0427 |
[...] i2c 6-0010: | MODULE | ID:  0x0183                     REV:   0012 |
[...] i2c 6-0010: | SENSOR | SONY IMX183                                 |
...
```

## 将驱动程序集成到您自己的 BSP 中

如果您有自己的 BSP，则必须将驱动程序集成到其中。请按照以下步骤操作。

1. 应用 `kernel_common_32.3.1+` 文件夹中的所有补丁，以及下表中与您的硬件设置相匹配的补丁。

   | 系统核心模块 (SoM)         | 载板  | BSP             | patch/... 文件夹中的所有补丁 |
   | ------------------------ | -------------- | --------------- | --------------------- |
   | NVIDIA Jetson Nano       | NVIDIA DevKit  | 32.7.1 - 32.7.3 | kernel_Nano_32.6.1+   |
   |                          |                | 32.7.4          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.4 |
   |                          |                | 32.7.5          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.5 |
   |                          | Auvidea JNX30  | 32.7.1 - 32.7.3 | kernel_Nano_32.6.1+ <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   |                          |                | 32.7.4          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.4 <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   |                          |                | 32.7.5          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.5 <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   |                          | Auvidea JNX42  | 32.7.1 - 32.7.3 | kernel_Nano_32.6.1+ <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   |                          |                | 32.7.4          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.4 <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   |                          |                | 32.7.5          | kernel_Nano_32.6.1+ <br> kernel_Nano_32.7.5 <br> dt_Auvidea_JNX30_Nano_32.5.0+ |
   | NVIDIA Jetson Xavier NX  | NVIDIA DevKit  | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ |
   |                          |                | 35.1.0          | kernel_Xavier_35.1.0+ |
   |                          |                | 35.2.1          | kernel_Xavier_35.2.1+ |
   |                          |                | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   |                          | Auvidea JNX30  | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ <br> dt_Auvidea_JNX30_XavierNX_32.5.0+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ <br> dt_Auvidea_JNX30_XavierNX_32.5.0+ |
   |                          |                | 35.1.0          | kernel_Xavier_35.1.0+ |
   |                          |                | 35.2.1          | kernel_Xavier_35.2.1+ |
   |                          |                | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   |                          | Auvidea JNX42  | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ <br> dt_Auvidea_JNX30_XavierNX_32.5.0+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ <br> dt_Auvidea_JNX30_XavierNX_32.5.0+ |
   |                          |                | 35.1.0          | kernel_Xavier_35.1.0+ |
   |                          |                | 35.2.1          | kernel_Xavier_35.2.1+ |
   |                          |                | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   | NVIDIA Jetson AGX Xavier | DevKit + J20   | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ |
   |                          |                | 35.1.0          | kernel_Xavier_35.1.0+ |
   |                          |                | 35.2.1          | kernel_Xavier_35.2.1+ |
   |                          |                | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   | NVIDIA Jetson TX2        | DevKit + J20   | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ |
   | NVIDIA Jetson TX2 NX     | Auvidea JNX30D | 32.7.1 - 32.7.2 | kernel_Xavier_32.6.1+ |
   |                          |                | 32.7.3          | kernel_Xavier_32.7.3+ |
   | NVIDIA Jetson Orin Nano  | NVIDIA DevKit  | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   |                          |                | 36.2.0          | kernel_Xavier_36.2.0+ * |
   |                          |                | 36.4.0          | kernel_Xavier_36.4.0+ * |
   |                          |                | 36.4.3          | kernel_Xavier_36.4.3 * |
   |                          | Auvidea JNX42  | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   |                          |                | 36.2.0          | kernel_Xavier_36.2.0+ * |
   |                          |                | 36.4.0          | kernel_Xavier_36.4.0+ * |
   |                          |                | 36.4.3          | kernel_Xavier_36.4.3 * |
   | NVIDIA Jetson Orin NX    | NVIDIA DevKit  | 36.2.0          | kernel_Xavier_36.2.0+ * |
   |                          |                | 36.4.0          | kernel_Xavier_36.4.0+ * |
   |                          |                | 36.4.3          | kernel_Xavier_36.4.3 * |
   |                          | Auvidea JNX42  | 35.2.1          | kernel_Xavier_35.2.1+ |
   |                          |                | 35.3.1          | kernel_Xavier_35.3.1+ |
   |                          |                | 35.4.1          | kernel_Xavier_35.4.1+ |
   |                          |                | 36.2.0          | kernel_Xavier_36.2.0+ * |
   |                          |                | 36.4.0          | kernel_Xavier_36.4.0+ * |
   |                          |                | 36.4.3          | kernel_Xavier_36.4.3 * |
*) 对于 L4T 36.x.y 版本，必须排除 `kernel_common_32.3.1+`。

2. 将摄像头设备树复制到下表列出的文件夹中

   | 系统核心模块 (SoM)         | 载板  | 从 src/devicetree/... 复制到以下文件夹 |
   | ------------------------ | -------------- | ---------------------------------- |
   | NVIDIA Jetson Nano       | NVIDIA DevKit  | NV_DevKit_Nano/tegra210-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t210/porg/kernel-dts/porg-platforms |
   |                          | Auvidea JNX30  | Auvidea_JNX30_Nano/tegra210-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t210/porg/kernel-dts/porg-platforms |
   |                          | Auvidea JNX42  | Auvidea_JNX42_Nano/tegra210-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t210/porg/kernel-dts/porg-platforms |
   | NVIDIA Jetson Xavier NX  | NVIDIA DevKit  | NV_DevKit_XavierNX/tegra194-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t19x/jakku/kernel-dts/common |
   |                          | Auvidea JNX30  | Auvidea_JNX30_XavierNX/tegra194-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t19x/jakku/kernel-dts/common |
   |                          | Auvidea JNX42  | Auvidea_JNX42_XavierNX/tegra194-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t19x/jakku/kernel-dts/common |
   | NVIDIA Jetson AGX Xavier | DevKit + J20   | Auvidea_J20_AGXXavier/tegra194-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t19x/common/kernel-dts/t19x-common-modules |
   | NVIDIA Jetson TX2        | DevKit + J20   | Auvidea_J20_TX2/tegra186-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t18x/common/kernel-dts/t18x-common-modules |
   | NVIDIA Jetson TX2 NX     | Auvidea JNX30D | Auvidea_JNX30D_TX2NX/tegra186-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t18x/lanai/kernel-dts/common |
   | NVIDIA Jetson Orin Nano  | NVIDIA DevKit  | NV_DevKit_OrinNano/tegra234-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t23x/p3768/kernel-dts/cvb <br> (Jetpack 5 使用的设备树包含文件) |
   |                          |                | NV_DevKit_OrinNano/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> => /hardware/nvidia/t23x/nv-public/overlay <br> (Jetpack 6 的设备树叠加层文件) |
   |                          | Auvidea JNX42  | Auvidea_JNX42_OrinNano/tegra234-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t23x/p3768/kernel-dts/cvb <br> (Jetpack 5 使用的设备树包含文件) |
   |                          |                | Auvidea_JNX42_OrinNano/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> => /hardware/nvidia/t23x/nv-public/overlay <br> (Jetpack 6 的设备树叠加层文件) |
   | NVIDIA Jetson Orin NX    | Auvidea JNX42  | Auvidea_JNX42_OrinNX/tegra234-camera-vc-mipi-cam.dtsi <br> => /hardware/nvidia/platform/t23x/p3768/kernel-dts/cvb <br> (Jetpack 5 使用的设备树包含文件) |
   |                          |                | Auvidea_JNX42_OrinNX/tegra234-p3767-camera-p3768-vc_mipi-dual.dts <br> => /hardware/nvidia/t23x/nv-public/overlay <br> (Jetpack 6 的设备树叠加层文件) |

3. 将 **src/driver** 文件夹中的所有驱动程序文件复制到 <br> 
**/kernel/nvidia/drivers/media/i2c** (Jetpack 5) 或 <br>
**/nvidia-oot/drivers/media/i2c/vc_mipi** (Jetpack 6) <br>

## 测试摄像头

有关如何测试摄像头，请参阅 [测试应用程序](/doc/TEST_APPLICATIONS.md)

## 注解 (Annotations)

在目标系统上工作时，无论安装了哪个版本的 Jetpack，<br>

&#x26a0;&#xfe0f;&nbsp;&nbsp;&nbsp; 您 <ins>**绝对不能**</ins> 执行升级 (upgrade)！ &nbsp;&nbsp;&nbsp;&#x26a0;&#xfe0f; <br>

否则，为 MIPI 驱动程序打过补丁的基础架构将被替换，驱动程序在以后启动时将无法加载。<br>
允许（或推荐）执行更新 (update)：
```
sudo apt update
```

### 针对 Jetpack 5 (L4T 35.1.0, 35.2.1, 35.3.1, 35.4.1)：

* 当系统（NVIDIA Jetson Xavier NX 或 AGX Xavier）成功启动后，必须以超级用户身份运行 /target 文件夹中的 max_speed.sh 脚本。它将读取最大频率并将其设置为当前频率。这是 NVIDIA 的推荐做法。

  ```bash
  sudo ./max_speed.sh --max
  ```

### 针对 Jetpack 6 (L4T 36.2.0 DP, L4T 36.4.0, L4T 36.4.3)：

- 在 L4T 36.x.y 版本中，tegra 部分已完全从内核中分离出来
- VC MIPI 驱动程序现在作为一组内核模块运行
- 对摄像头设备树的修改通过设备树叠加层 (device tree overlays) 实现

- nvarguscamerasrc + nvvidconv 必须单独安装（gst-nvarguscamera_src.tbz2 和 gst-nvvidconv_src.tbz2 位于 Linux_for_Tegra/source 文件夹中，并将自动复制到目标设备的主目录中）
  - 也可以使用 argus_camera 示例应用程序 (nvidia-l4t-jetson-multimedia-api)
- 可以在运行中的 Jetson 上调用便捷脚本 setup_nvidia.sh 来安装一些先决条件（必须连接互联网）
  - 该脚本将安装编译必备工具、nvidia-l4t-jetson-multimedia-api、lib-cuda-dev 和 v4l-utils
  - 将生成 nvarguscamerasrc 和 nvvidconv
  - 将编译并安装 NVIDIA 示例（包括 argus_camera）

- 设备树处理
  - 可以使用 `./setup.sh --camera` 编辑 tegra234-p3767-camera-p3768-vc_mipi-dual.dts 文件
  - 使用 `./build.sh --dt` 将生成 tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo 文件 | 此步骤由 `./build.sh --all` 自动完成
  - dtbo 文件将生成到主机 PC 的 kernel/dtb 目录中
  - 第一次使用 `sudo ./flash.sh --all` 烧录时，叠加层文件将被烧录到 UEFI 中
  - 要通过 tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo 文件修改摄像头设置，必须修改/复制 /boot/extlinux/extlinux.conf 条目，并添加 <br>
  OVERLAYS /boot/tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo
这一行。这将覆盖初始的 UEFI dtbo，使修改生效
  - 在重启之前，必须将 dtbo 文件复制到指定位置
```
LABEL secondary
      MENU LABEL secondary kernel
      LINUX /boot/Image
      FDT /boot/dtb/kernel_tegra234-p3768-0000+p3767-0005-nv.dtb
      INITRD /boot/initrd
      APPEND ...

      OVERLAYS /boot/tegra234-p3767-camera-p3768-vc_mipi-dual.dtbo
```

   > * 此版本已使用两个相同的 IMX565, IMX567/8, IMX296, IMX226, IMX415 传感器进行过测试
   > * 仅使用单个传感器时可能会出现问题


### 针对 NVIDIA Jetson TX2 NX

* 目前以下摄像头模块无法在 TX2 NX 上工作
  * IMX178, IMX183
  * IMX250, IMX252, IMX264, IMX265, IMX273, IMX392

### 故障排除 (Troubleshooting)

关于烧录问题，请参阅 **[烧录设备 (Flashing the device)](doc/DEVICE_FLASHING.md)** 

