中文版基于[原英文文档](BINNING_MODE.md) v 0.19.0 翻译

# Binning 模式

可以使用以下 v4l2 控制项设置传感器的 binning 模式：
```
v4l2-ctl -c binning_mode=<index>
```
该值决定了 binning 模式，并取决于传感器的能力。水平和垂直 binning 因子有一组预定义的数值对，通过索引进行寻址。<br>
例如 IMX412：
```
v4l2-ctl -c binning_mode=1
```
表示水平 binning 为 1，垂直 binning 为 2 (1x2)。
预定义的数值对可以在传感器的 `vc_init_ctrl_xxx` 函数中查看。例如 `vc_init_ctrl_imx412(...)`<br>
设置 binning 模式时，必须相应地调整 ROI。


下表概述了传感器各 binning 模式下的最大分辨率：

| 传感器模块 | binning_mode <br> 索引 | binning 因子 <br>(水平 x 垂直) | 最大分辨率 <br> (宽 x 高) | 备注 |
| ------------- | ------------------ | --------------- | ----------- | ----------------- |
| IMX412        | 0                  |      0 x 0      | 4032 x 3040 | 无 binning |
|               | 1                  |      1 x 2      | 4032 x 1520 | 垂直方向 2 像素 |
|               | 2                  |      2 x 2      | 2016 x 1520 | 水平方向 2 像素 <br> 垂直方向 2 像素 |
|               | 3                  |      4 x 2      |  992 x 1520 | 水平方向 4 像素 <br> 垂直方向 2 像素 |
|               | 4                  |      8 x 2      |  480 x 1520 | 水平方向 8 像素 <br> 垂直方向 2 像素 |
|               | 5                  |     16 x 2      |  224 x 1520 | 水平方向 16 像素 <br> 垂直方向 2 像素 <br> 扩展 binning |
| IMX565        | 0                  |      0 x 0      | 4128 x 3000 | 无 binning |
|               | 1                  |      2 x 2      | 2048 x 1504 | 水平方向 2 像素 <br> 垂直方向 2 像素 |
| IMX566        | 0                  |      0 x 0      | 2848 x 2848 | 无 binning |
|               | 1                  |      2 x 2      | 1408 x 1408 | 水平方向 2 像素 <br> 垂直方向 2 像素 |
| IMX567/568    | 0                  |      0 x 0      | 2464 x 2064 | 无 binning |
|               | 1                  |      2 x 2      | 1216 x 1032 | 水平方向 2 像素 <br> 垂直方向 2 像素 |

也可以将 ROI 与 binning 模式结合使用。宽度必须是 32 像素的倍数，高度必须是 8 像素的倍数。这两个值都必须小于上表给出的最大值。另请参阅 **[ROI 裁剪](ROI_CROPPING.md)**。

当 Pregius S (IMX56x) 开启 binning 时，传感器会变为黑白模式。

## 示例
### V4L
```
$ v4l2-ctl --set-fmt-video=width=4032,height=3040 -c binning_mode=0
```
> IMX412 无 binning。

<br>

```
$ v4l2-ctl --set-fmt-video=width=2016,height=1520 -c binning_mode=2
```
> IMX412 开启 2x2 binning。

<br>

```
v4l2-ctl --set-selection=left=0,top=0,width=4128,height=3000 -c binning_mode=0
```
> IMX565 无 binning。

<br>

```
v4l2-ctl --set-selection=left=0,top=0,width=2048,height=1504 -c binning_mode=1
```
> IMX565 开启 2x2 binning。

<br>

```
v4l2-ctl --set-selection=left=128,top=0,width=1920,height=1504 -c binning_mode=1
```
> IMX565 开启 2x2 binning，偏移 128 像素，并相应设置宽度。这 128 像素的偏移量将从该 binning 模式下的最大宽度中扣除。

<br>

### 使用 nvarguscamerasrc 或 argus_camera 应用程序的 GStreamer

您可以在设备树中定义 `binning_mode` 属性，并将其用于多个模式。因此，`use_sensor_mode_id` 也必须设置为 `true`。

```
        {
        ...
        use_sensor_mode_id      = "true";
        ...
                mode0 {
                        ...
                        binning_mode            = "0";
                        active_l                = "0";       // 左 (left)
                        active_t                = "0";       // 上 (top)
                        active_w                = "4032";    // 宽 (width)
                        active_h                = "3040";    // 高 (height)
                        ...
                };

                mode1 {
                        ...
                        binning_mode            = "2";
                        active_l                = "0";       // 左 (left)
                        active_t                = "0";       // 上 (top)
                        active_w                = "2016";    // 宽 (width)
                        active_h                = "1520";    // 高 (height)
                        ...
                };
        }
```
> 请注意，如果在设备树中为一个或多个模式设置了 `binning_mode` 属性，则 v4l 的 `binning_mode` 控制项将不再对这些模式生效。

> argus 管道的最小尺寸为 256x256 像素。这意味着以下配置会导致 argus 守护进程崩溃：
```
        mode0 {
                ...
                binning_mode            = "5";
                active_l                = "0";       // 左 (left)
                active_t                = "0";       // 上 (top)
                active_w                = "224";     // 宽 (width)
                active_h                = "1520";    // 高 (height)
                ...
        };
```

还可以使用 `sensor_mode` 属性。当设备树中定义了多个模式时，可以在流媒体播放时设置 `sensor_mode=<index>`。
<br>
<pre>
gst-launch-1.0 nvarguscamerasrc sensor-id=0 <b>sensor-mode=2</b> ! 'video/x-raw(memory:NVMM), format=(string)NV12, framerate=(fraction)40/1' ! nvvidconv ! queue ! xvimagesink
</pre>
此调用演示了 nvargus gstreamer 管道的使用。在这种情况下可以省略 width 和 height 属性，因为这些值是从设备树中读取的。</br>

### 帧尺寸校验

如果设置了 binning 模式，MIPI 驱动程序将在流启动前检查帧尺寸。如果检测到给定的 binning 模式超出了传感器的宽或高能力，则不会执行流，并在 dmesg 中发布相应的消息。</br>
当通过 gstreamer 使用 `nvarguscamerasrc` 或 `argus_camera` 且设备树中误配置了 width、height 或 binning 参数之一时，nvargus-daemon 可能会崩溃。
在这种情况下，必须取消或杀死流应用程序，并重新启动 nvargus-daemon：
<pre>
sudo killall nvargus-daemon
sudo nvargus-daemon &
</pre>
