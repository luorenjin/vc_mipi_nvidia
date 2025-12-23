中文版基于[原英文文档](FRAME_RATE.md) v 0.19.0 翻译

# 帧率

帧率始终以 mHz 为单位，例如：20000 mHz 对应 20 Hz。<br>
基本上可以通过以下 shell 命令进行调节：
```
v4l2-ctl -d /dev/video0 -c frame_rate=<value>
```
或者
```
v4l2-ctl -d /dev/video0 --set-ctrl frame_rate=<value>
```
帧率值为 0 表示传感器在给定模式下所能达到的最大帧率。
无论通过 v4l2 还是 ISP 进行流传输，都可以在流传输过程中修改帧率。

在使用 ISP 应用程序（nvarguscamerasrc 或 argus_camera）进行流传输时，必须先在设备树中设置一些与帧率相关的数值。
因此有以下数值：
<pre>
      min_framerate            = "1000";      //       1 Hz
      max_framerate            = "43600";     //    43.6 Hz
      step_framerate           = "100";       //     0.1 Hz
      default_framerate        = "43600";     //    43.6 Hz
      ...
      framerate_factor         = "1000";
</pre> 
`min_framerate` 参数决定了流传输的最低可能帧率。此值不能太小，否则 Tegra 框架会因为帧周期过长（直到获取下一帧的时间太长）而导致运行超时。<br>
`max_framerate` 参数定义了流传输模式的最大可能帧率。它不能超过给定传感器在特定模式下的值。例如，当传感器没有进行 ROI 裁剪或未设置 binning 选项时，应使用 [Readme GStreamer Support](/README.md#gstreamer-support) 中的最大值。如果您想获得更高的最大帧率，可以尝试减小帧高度（[ROI 裁剪](/doc/ROI_CROPPING.md)）。“frame rate increase”列将显示您的传感器是否可以在减小帧高度的情况下运行得更快。<br>
或者，如果您的传感器支持，您可以尝试使用 binning 模式。另请参阅 [Binning 模式](/doc/BINNING_MODE.md)。<br>
`step_framerate` 参数用于 ISP 以特定幅度增加或减少帧率。<br>
ISP 的自动曝光功能将使用这些值来计算帧周期时间，从而在此范围内设置曝光值。<br>
`default_framerate` 属于 v4l 控制结构，也必须进行设置。它应该小于或等于 `max_framerate` 参数。在驱动程序内部，它将被设置为 0，这意味着最大帧率。<br>
> 请注意，`nvarguscamerasrc` 应用程序不会使用 `default_framerate` 的值。如果您在 gstreamer 调用中省略了帧率规格，`nvarguscamerasrc` 将使用其自身的默认值 30 Hz，无论设备树中设置了什么。<br>

`framerate_factor` 用于浮点和定点数据类型之间的转换。例如：
<pre>
      max_framerate            = "43600";
      framerate_factor         = "1000";
</pre>
或者
<pre>
      max_framerate            = "43600000";
      framerate_factor         = "1000000";
</pre>
这两种表示方式最终都会得到 43.6 Hz 的最大帧率。

通过 v4l2 应用程序进行的流传输不会使用上述设备树条目中的 min、max、step 和 default 帧率参数。它使用 v4l2 `frame_rate` 属性的值，该值也可以通过流传输调用进行设置：

<pre>
v4l2-ctl --stream-mmap --set-ctrl=<b>frame_rate=20000</b>
</pre>

<pre>
./v4l2-test stream -d /dev/video0 <b>-r 20000</b> -e 5000 -g 0 -p 1 -n 10
</pre>
