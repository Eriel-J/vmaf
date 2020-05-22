VMAF - Video Multi-Method Assessment Fusion
===================

[![Build Status](https://travis-ci.org/Netflix/vmaf.svg?branch=master)](https://travis-ci.org/Netflix/vmaf) [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/68i57b8ssasttngg?svg=true)](https://ci.appveyor.com/project/li-zhi/vmaf) [![libvmaf](https://github.com/Netflix/vmaf/workflows/libvmaf/badge.svg)](https://github.com/Netflix/vmaf/actions?query=workflow%3Alibvmaf)

VMAF is a perceptual video quality assessment algorithm developed by Netflix. VMAF Development Kit (VDK) is a software package that contains the VMAF algorithm implementation, as well as a set of tools that allows a user to train and test a custom VMAF model. Read [this](https://medium.com/netflix-techblog/toward-a-practical-perceptual-video-quality-metric-653f208b9652) techblog post for an overview, or [this](https://medium.com/netflix-techblog/vmaf-the-journey-continues-44b51ee9ed12) post for the latest updates and tips for best practices.

![vmaf logo](resource/images/vmaf_logo.jpg)

## News

- (2/27/20) We have changed VMAF's license from Apache 2.0 to [BSD+Patent](https://opensource.org/licenses/BSDplusPatent), a more permissive license compared to Apache that also includes an express patent grant. VMAF has been increasingly used in a number of open-source multimedia projects. However, it posed a problem to more deeply integrate VMAF into these projects to perform advanced tasks. Many of the open-source projects are licensed under GPLv2/LGPLv2, and the old Apache 2.0 license was incompatible with GPLv2/LGPLv2. Due to this incompatibility, when integrating VMAF source code into these projects, the license automatically got bumped to GPLv3/LGPLv3, which could prove challenging for downstream projects who might want to integrate VMAF. Changing VMAF’s license to BSD+Patent have resolved this issue, because it is compatible with GPLv2/LGPLv2.
- (2/27/20) We made a few changes in a recent refactoring effort: 1) migrated the build system from makefile to meson, 2) restructured the code, and 3) introduced a new release candidate API with the associated library `libvmaf_rc` and executable `vmaf_rc`, co-existing with the current `libvmaf` and `vmafossexec`, all under `libvmaf/build`. The new release candidate API is designed for better interoperrability with encoding optimization. We will deprecate the old API on a future date.
- (9/8/19) Added a [link to report VMAF bad cases](https://docs.google.com/forms/d/e/1FAIpQLSdJntNoBuucMSiYoK3SDWoY1QN0yiFAi5LyEXuOyXEWJbQBtQ/viewform?usp=sf_link). Over time, we have received feedbacks on when VMAF's prediction does not reflect the expected perceptual quality of videos, either they are corner cases where VMAF fails to cover, or new application scenarios which VMAF was not initially intended for. In response to that, we have created the Google form to allow users to upload their video samples and describe the scenarios. The bad cases are valuable for improving future versions of VMAF. Users can opt in or out for sharing their sample videos publicly.

## Frequently Asked Questions

Refer to the [FAQ](FAQ.md) page.

## Usages

The VDK package offers a number of ways for a user to interact with the VMAF algorithm implementations. The core feature extraction library is written in C. The rest scripting code including the classes for machine learning regression, training and testing VMAF models and etc., is written in Python. Besides, there is C++ an implementation partially replicating the logic in the regression classes, such that the VMAF prediction (excluding training) is fully implemented.
VDK软件包为用户提供了多种与VMAF算法实现进行交互的方式。核心特征提取库用C编写。其余脚本代码（包括用于机器学习回归，训练和测试VMAF模型等的类）用Python编写。此外，还有一个C ++实现，该实现部分复制了回归类中的逻辑，从而完全实现了VMAF预测（不包括训练）。

There are a number of ways one can use the package:

  - [VMAF Python library](resource/doc/VMAF_Python_library.md) offers full functionalities including running basic VMAF command line, running VMAF on a batch of video files, training and testing a VMAF model on video datasets, and visualization tools, etc.
  - VMAF Python库提供了全部功能，包括运行基本的VMAF命令行，在一批视频文件上运行VMAF，在视频数据集上训练和测试VMAF模型以及可视化工具等。
  - [`vmafossexec` - a C++ executable](resource/doc/vmafossexec.md) offers running the prediction part of the algorithm in full, such that one can easily deploy VMAF in a production environment without needing to configure the Python dependencies. Additionally, `vmafossexec` offers a number of exclusive features, such as 1) speed optimization using multi-threading and skipping frames, 2) optionally computing PSNR, SSIM and MS-SSIM metrics in the output.
  - vmafossexec-一种C ++可执行文件，提供了完整运行算法的预测部分的功能，因此无需配置Python依赖项就可以轻松地在生产环境中部署VMAF。此外，vmafossexec提供了许多独有功能，例如1）使用多线程和跳过帧进行速度优化，2）可选地计算输出中的PSNR，SSIM和MS-SSIM指标。
  - [`libvmaf` - a C library](libvmaf/README.md) offers an interface to incorporate VMAF into your C/C++ code.
  - libvmaf-C库提供了将VMAF合并到您的C / C ++代码中的接口。
  - VMAF is now included as a filter in [FFmpeg](http://ffmpeg.org/) and can be configured using: `./configure --enable-libvmaf --enable-version3`. See the [FFmpeg documentation](https://ffmpeg.org/ffmpeg-filters.html#libvmaf) for usage.
  - VMAF现在作为FFmpeg的筛选器包括在内，可以使用以下命令进行配置：./configure --enable-libvmaf --enable-version3。有关用法，请参见FFmpeg文档。
  - [VMAF Dockerfile](Dockerfile) generates a VMAF docker image from the [VMAF Python library](resource/doc/VMAF_Python_library.md). Refer to [this](resource/doc/docker.md) document for detailed usages.
  - VMAF Dockerfile从VMAF Python库生成VMAF docker映像。有关详细用法，请参阅此文档。
  - Build VMAF on Windows: follow instructions on [this](resource/doc/BuildForWindows.md) page.
  - 在Windows上构建VMAF：按照此页面上的说明进行操作。

## Datasets

We also provide [two sample datasets](resource/doc/datasets.md) including the video files and the properly formatted dataset files in Python. They can be used as sample datasets to train and test custom VMAF models.
我们还提供了两个样本数据集，包括视频文件和Python格式正确的数据集文件。它们可用作样本数据集，以训练和测试自定义VMAF模型。

## Models

Besides the default VMAF model which predicts the quality of videos displayed on a 1080p HDTV in a living-room-like environment, VDK also includes a number of additional models, covering phone and 4KTV viewing conditions. Refer to the [models](resource/doc/models.md) page for more details.
除了可预测类似客厅环境的1080p HDTV上的视频质量的默认VMAF模型之外，VDK还包括许多其他模型，涵盖电话和4KTV的观看条件。有关更多详细信息，请参阅型号页面。

## Confidence Interval

Since VDK v1.3.7 (June 2018), we have introduced a way to quantify the level of confidence that a VMAF prediction entails. Each VMAF prediction score now can come with a 95% confidence interval (CI), which quantifies the level of confidence that the prediction lies within the interval. Refer to the [VMAF confidence interval](resource/doc/conf_interval.md) page for more details.
自VDK v1.3.7（2018年6月）以来，我们引入了一种量化VMAF预测所需要的置信度的方法。现在，每个VMAF预测得分都可以带有95％的置信区间（CI），该区间可量化预测位于该区间内的置信度。有关更多详细信息，请参考VMAF置信区间页面。

## Matlab Functionality

Besides the Python/C/C++ part of the repository, we also introduced a number of algorithms that are implemented in Matlab. For example, users can calculate ST-RRED, ST-MAD, SpEED-QA, and BRISQUE. For more details, see the [Matlab Usage](resource/doc/matlab_usage.md) page for more details.
除了存储库的Python / C / C ++部分，我们还介绍了许多在Matlab中实现的算法。例如，用户可以计算ST-RRED，ST-MAD，SpEED-QA和BRISQUE。有关更多详细信息，请参见Matlab使用情况页面以获取更多详细信息。

## References

Refer to the [references](resource/doc/references.md) page.
