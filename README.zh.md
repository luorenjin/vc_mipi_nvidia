# Vision Components MIPI CSI-2 驱动（适用于 NVIDIA Jetson 系列）

这是 Vision Components 为 NVIDIA Jetson 平台（包括 Jetson Nano、Xavier NX、AGX Xavier、TX2、TX2 NX、Orin Nano、Orin NX）提供的 MIPI CSI-2 摄像头驱动与配套工具集合。该驱动支持 VC MIPI 摄像头模块，并包含便捷的构建与部署脚本。

主要特性：

- 支持系统模块（SOM）：NVIDIA Jetson Nano、Jetson Xavier NX、AGX Xavier、Jetson TX2、TX2 NX、Orin Nano、Orin NX。
- 支持载板：NVIDIA 官方开发板与部分 Auvidea 板（JNX30、JNX42、J20 等）。
- 支持的 VC MIPI 摄像头模块（示例）：
  - SONY: IMX178, IMX183, IMX226
  - SONY: IMX250, IMX252, IMX264, IMX265, IMX273, IMX392
  - SONY: IMX290, IMX327, IMX462
  - SONY: IMX296, IMX297
  - SONY: IMX335, IMX412, IMX415
  - SONY: IMX565, IMX566, IMX567, IMX568
  - SONY: IMX900
  - OmniVision: OV7251, OV9281

- 功能亮点：
  - 支持多种像素格式（GREY, Y10, Y12, SRGGB8/10/12, SGBRG8/10/12 等）
  - 通过 V4L2 控制项支持设置曝光（exposure）与增益（gain）
  - 支持多种触发模式（外部触发、脉冲宽度触发、软件触发、单帧触发、同步触发等）
  - 支持 IO 模式（闪灯/触发 控制）和 ROI 裁剪
  - 支持 binning（部分传感器）与帧率设置

快速开始（简要）：

1. 在主机上准备环境（推荐 Ubuntu 18.04/20.04），安装 git。
2. 克隆仓库：

   git clone https://github.com/VC-MIPI-modules/vc_mipi_nvidia

3. 运行快速安装脚本并按照提示操作：

   cd vc_mipi_nvidia/bin
   ./quickstart.sh

4. （可选）在目标设备上运行 setup：

   cd vc_mipi_nvidia/bin
   ./setup.sh --target

- 如果需要修改摄像头参数，请编辑对应的设备树文件（位于 src/devicetree 下），然后运行：

  ./build.sh --dt
  ./flash.sh --dt

- 对于 Jetpack 5/6 的差异与 Overlay 使用，请参阅仓库中的 README 英文版和 doc 目录中的说明。

设备树与整合说明（简要）：

- 本驱动通过设备树（或 overlay）配置各个摄像头模块的模式、像素格式、物理尺寸、默认帧率等。
- 若集成到自有 BSP，请将 src/devicetree 中对应板卡的 dtsi 复制到 BSP 指定位置，并将 src/driver 中的驱动文件复制到目标内核源码对应目录（参考 README 中的详细步骤）。

许可证与反馈：

请在遇到问题时检查 dmesg 日志和 README 中的调试/排错章节。欢迎在仓库中提交 issues 或 PR 提供改进建议。
