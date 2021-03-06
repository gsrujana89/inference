[![compatibility](https://github.com/ctuning/ck-guide-images/blob/master/ck-compatible.svg)](https://github.com/ctuning/ck)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

# MLPerf Inference - Object Classification - MobileNet
- [MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications](https://arxiv.org/abs/1704.04861) (Howard et al., 2017)
- [MobileNetV2: Inverted Residuals and Linear Bottlenecks ](https://arxiv.org/abs/1801.04381) (Sandler et al., 2018)

**NB:** MLPerf Inference v0.5 uses MobileNets-v1-1.0-224 (called MobileNet in what follows).

# Table of contents

1. [Installation](#installation)
    - [Install prerequisites](#installation-debian) (Debian-specific)
    - [Install CK workflows](#installation-workflows) (universal)
1. [Benchmarking](#benchmarking)
    - [via TensorFlow Lite](tflite/README.md)
    - [via TensorFlow (C++)](tf-cpp/README.md)
    - [via TensorFlow (Python)](tf-py/README.md)

<a name="installation"></a>
# Installation

<a name="installation-debian"></a>
## Debian (tested with Ubuntu v18.04 and v16.04)

- Common tools and libraries.
- [Python](https://www.python.org/), [pip](https://pypi.org/project/pip/), [SciPy](https://www.scipy.org/), [Collective Knowledge](https://cknowledge.org) (CK).
- (Optional) [Android SDK](https://developer.android.com/studio/), [Android NDK](https://developer.android.com/ndk/).

### Install common tools and libraries
```bash
$ sudo apt install autoconf autogen libtool zlib1g-dev
$ sudo apt install gcc g++ git wget
$ sudo apt install libblas-dev liblapack-dev
```

### Install Python, pip, SciPy and CK
```bash
$ sudo apt install python3 python3-pip
$ sudo python3 -m pip install scipy
$ sudo python3 -m pip install ck
```
**NB:** CK also supports Python 2.

### [Optional] Install Android SDK and NDK

You can optionally target Android API 23 (v6.0 "Marshmallow") devices using the
`--target_os=android23-arm64` flag 
(or [similar](https://source.android.com/setup/start/build-numbers)), when using
the TensorFlow Lite benchmark (recommended) and TensorFlow (C++) benchmark (not recommended).

On Debian Linux, you can install the [Android SDK](https://developer.android.com/studio/) and the [Android NDK](https://developer.android.com/ndk/) as follows:
```
$ sudo apt install android-sdk
$ adb version
Android Debug Bridge version 1.0.36
Revision 1:7.0.0+r33-2
$ sudo apt install google-android-ndk-installer
```
**NB:** On Ubuntu 18.04, NDK r13b gets installed. On Ubuntu 16.04, download [NDK r18b](https://dl.google.com/android/repository/android-ndk-r18b-linux-x86_64.zip) and extract it into e.g. `/usr/local`. NDK r18c only supports LLVM, which currently requires a CK quirk to work properly (removing a dependency on `soft:compiler.gcc.android.ndk` from `soft:compiler.llvm.android.ndk`).

<a name="installation-workflows"></a>
## Install CK workflows for MLPerf

### Pull CK repositories
```bash
$ ck pull repo:ck-mlperf
```
**NB:** Transitive dependencies include [repo:ck-tensorflow](https://github.com/ctuning/ck-tensorflow).

### Install a small dataset (500 images)
```bash
$ ck install package:imagenet-2012-val-min 
```
**NB:** ImageNet dataset descriptions are in [repo:ck-env](https://github.com/ctuning/ck-env).

### Install the full dataset (50,000 images)
```bash
$ ck install package:imagenet-2012-val
```

**NB:** If you already have the ImageNet validation dataset downloaded in a directory e.g. `$HOME/ilsvrc2012-val/`, you can simply detect it as follows:
```bash
$ ck detect soft:dataset.imagenet.val --full_path=$HOME/ilsvrc2012-val/ILSVRC2012_val_00000001.JPEG
```

<a name="benchmarking"></a>
## Benchmarking

You can benchmark MobileNet using one of the available options:
- [via TensorFlow Lite](tflite/README.md)
- [via TensorFlow (C++)](tf-cpp/README.md)
- [via TensorFlow (Python)](tf-py/README.md)
