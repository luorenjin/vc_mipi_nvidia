中文版基于[原英文文档](TEST_APPLICATIONS.md) v 0.19.0 翻译

# 测试应用程序

流媒体传输有两种方式。一种是使用 v4l2，它提供未经后期处理的原始图像素材。v4l2 流媒体支持不同的像素分辨率，分别为每像素 8、10、12 或 14 位。根据所使用的传感器模块，并非所有像素深度都受支持。请查看 [README GStreamer 支持](/README.md#gstreamer-support)中关于 pixel_t 的表格。<br>
在某些设置中，像素的位值可能会发生位偏移。基本上，在 8 位模式下没有位偏移。每个像素使用一个字节来存储像素值。10、12 和 14 位的像素格式将数据存储在两个字节中。在几乎所有 NVIDIA 的 SOM 上，两字节数组中的像素数据都会发生位偏移。唯一的例外是 Jetson Nano，它将像素数据在两字节数组中右对齐存储。
v4l2 流媒体不会补偿这种位偏移。这意味着，当你在 Jetson Orin 上以 10 位模式通过 v4l2 获取图像素材时，由于像素位不是右对齐的，像素值会显得过亮。此外，在某些 Jetson SOM 上，高位值也会被镜像到数组的最低有效位。<br>
> 对于 v4l2 流媒体，bypass_mode 属性必须设置为 0！<br> 
> 否则，tegra 框架会等待来自 ISP 的数据，这将导致超时。<br>

另一方面，也可以通过 ISP 进行流媒体传输。这种方式提供了一些后期处理功能，以及自动曝光/增益功能。它还会将像素值补偿为两字节数组中的右对齐。支持的像素深度为 10、12 和 14 位（取决于传感器模块）。<br>
通过 ISP 流媒体传输还可以使用所谓的 ISP 文件，这些文件提供了诸如色彩校正或自动白平衡等额外的后期处理功能。
> 当执行 ISP 流媒体调用时，bypass_mode 将自动设置为 1！<br>

## v4l2 应用程序

### v4l2-ctl
最常用的 v4l2 应用程序是 v4l2-ctl。可以通过以下方式安装：
<pre>
sudo apt update
sudo apt-get install -y v4l-utils
</pre>
或者通过调用 target 文件夹中的 demo 脚本：
<pre>
./demo.sh
</pre>
更多信息请参阅：
<pre>
v4l2-ctl --help
</pre>
或 [v4l2-ctl 示例](#v4l2-ctl-examples)部分。

### v4l2-test
还有一个名为 [v4l2-test](https://github.com/pmliquify/v4l2-test) 的自定义 v4l2 应用程序，可以在 github 上找到。<br>
通过调用：
<pre>
./demo.sh
</pre>
也会获取并构建 v4l2-test 应用程序。<br>
该应用程序既可以作为 shell 上的独立程序运行，也可以作为通过网络流式传输的客户端/服务器应用程序运行。
它正在持续开发中，并且还有一个用于补偿位偏移的选项。
更多信息请参阅：
<pre>
./v4l2-test --help
</pre>
或 [v4l2-test](https://github.com/pmliquify/v4l2-test) 的文档。

## ISP 流媒体

### gstreamer
使用 gst-launch 应用程序 (gstreamer)，你可以将 nvarguscamerasrc 二进制文件作为流媒体源。在大多数 L4T 版本上，它已经部署好了。对于 JetPack 6（L4T 36.2.0 及更高版本），必须单独安装。为此，你可以使用 target 文件夹中的以下脚本：<br>
<pre>
./setup_nvidia.sh
</pre>
有关选项的更多信息，请参阅：
<pre>
gst-inspect-1.0 nvarguscamerasrc
</pre>
或 [gstreamer 示例](#gstreamer-examples)部分。

### argus_camera
argus_camera 应用程序是 NVIDIA 的 nvidia-l4t-jetson-multimedia-api 软件包的一部分。该应用程序提供了一个 GUI，以便在流媒体传输时方便地更改参数。<br>
它将通过以下方式自动获取、构建和安装：
<pre>
./setup_nvidia.sh
</pre>
安装程序完成后，你可以直接在目标设备的 shell 上调用该程序：
<pre>
argus_camera
</pre>
有关使用和选项的更多信息，请参阅：
<pre>
argus_camera --help
</pre>
或查看 /usr/src/jetson_multimedia_api/argus/apps/camera/README.TXT

## 示例

### v4l2-ctl 示例

获取第一个传感器设备的所有属性：
<pre>
v4l2-ctl -d /dev/video0 -l
</pre>

使用第一个视频设备以及 v4l2 属性中已设置的参数进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-ctrl bypass_mode=0 --stream-mmap
</pre>

使用第一个视频设备以最大可能的帧率进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-ctrl frame_rate=0 --stream-mmap
</pre>

使用第一个视频设备，宽度为 4032，高度为 3040，格式为 10 位（IMX412 RG-Bayerpattern）进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-fmt-video=width=4032,height=3040,pixelformat=RG10 --stream-mmap
</pre>

使用第一个视频设备，宽度为 3840，高度为 2160，格式为 10 位（IMX415 GB-Bayerpattern）进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-fmt-video=width=3840,height=2160,pixelformat=GB10 --stream-mmap
</pre>

使用第一个视频设备，宽度为 2048，高度为 1536，格式为 8 位（IMX900 RG-Bayerpattern）进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-fmt-video=width=2048,height=1536,pixelformat=RGGB --stream-mmap
</pre>

使用第一个视频设备，宽度为 3904，高度为 3000，格式为 8 位（IMX226 GB-Bayerpattern）进行流媒体传输：
<pre>
v4l2-ctl -d /dev/video0 --set-fmt-video=width=3904,height=3000,pixelformat=GBRG --stream-mmap
</pre>

使用第一个视频设备，采集五帧并写入文件：
<pre>
v4l2-ctl -d /dev/video0 --stream-mmap --stream-count=5 --stream-to=file_5_frames.raw
</pre>

### gstreamer 示例

使用第一个视频设备以 20 Hz 频率，使用第一个传感器模式（设备树中的 mode0）进行流媒体传输：
<pre>
gst-launch-1.0 nvarguscamerasrc sensor-id=0 sensor-mode=0 ! 'video/x-raw(memory:NVMM),framerate=20/1,width=4032,height=3040,format=NV12' ! nvvidconv ! queue ! fpsdisplaysink video-sink=xvimagesink text-overlay=true
</pre>

使用第一个视频设备以 78 Hz 频率，使用第二个传感器模式（设备树中的 mode1，binning_mode=2）进行流媒体传输：
<pre>
gst-launch-1.0 nvarguscamerasrc sensor-id=0 sensor-mode=1 ! 'video/x-raw(memory:NVMM),framerate=78/1,width=2016,height=1520,format=NV12' ! nvvidconv ! queue ! fpsdisplaysink video-sink=xvimagesink text-overlay=true
</pre>

使用传感器 mode0 进行流媒体传输，并写入 5 个 jpeg 编码的图像文件：
<pre>
gst-launch-1.0 nvarguscamerasrc sensor-id=0 sensor-mode=0 num-buffers=5 ! 'video/x-raw(memory:NVMM), framerate=20/1, width=4032, height=3040, format=NV12' ! nvvidconv ! jpegenc ! multifilesink location=~/$(date +%s)-%d.jpg
</pre>

使用传感器 mode0 进行流媒体传输，并写入 5 个 png 编码的图像文件：
<pre>
gst-launch-1.0 nvarguscamerasrc sensor-id=0 sensor-mode=0 num-buffers=5 ! 'video/x-raw(memory:NVMM), framerate=20/1, width=4032, height=3040, format=NV12' ! nvvidconv ! pngenc ! multifilesink location=~/$(date +%s)-%d.png
</pre>

> 如果设备树中只定义了 mode0，则可以省略 sensor-mode=<value> 属性。

> nvarguscamerasrc 正在对帧率和帧大小执行额外检查。如果设备树中定义了多个模式且省略了 sensor-mode=<value> 参数，则 nvarguscamerasrc 可能会预选不同的模式！

> 命令行参数中的 width 和 height 属性仅用于输出窗口，而不是设备树中设置的帧大小。帧大小（宽/高）取自给定的设备树模式或由 nvarguscamerasrc 预选的设备树模式的参数。
