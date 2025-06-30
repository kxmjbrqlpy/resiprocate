### Prerequisites

#### Preparing OpenHarmony SDK 

OpenHarmony provides SDKs for Linux, Windows, and macOS platforms, enabling cross-compilation across these systems. This guide focuses on Linux-based cross-compilation.

1. Download the SDK for your target platform from the [official release channel](https://gitcode.com/openharmony/docs/blob/master/en/release-notes/OpenHarmony-v5.0.1-release.md#acquiring-source-code-from-mirrors).

2. Extract the SDK package:

   ```shell
   owner@ubuntu:~/workspace$ tar -zxvf ohos-sdk-windows_linux-public.tar.tar.gz
   ```

3. Navigate to the SDK's Linux directory and extract all toolchain packages: <a id="ohos_sdk"> </a>

   ```shell
   owner@ubuntu:~/workspace$ cd ohos_sdk/linux
   owner@ubuntu:~/workspace/ohos-sdk/linux$ for i in *.zip;do unzip ${i};done
   owner@ubuntu:~/workspace/ohos-sdk/linux$ ls
   total 1228400
   85988 -rw-r--r-- 1 wshi wshi  88050148 Nov 20  2024 ets-linux-x64-5.0.1.111-Release.zip          # ArkTS compiler tools
   56396 -rw-r--r-- 1 wshi wshi  57747481 Nov 20  2024 js-linux-x64-5.0.1.111-Release.zip           # JS compiler tools
   888916 -rw-r--r-- 1 wshi wshi 910243125 Nov 20  2024 native-linux-x64-5.0.1.111-Release.zip      # C/C++ cross-compilation tools
   175084 -rw-r--r-- 1 wshi wshi 179281763 Nov 20  2024 previewer-linux-x64-5.0.1.111-Release.zip   # App preview tools
   22008 -rw-r--r-- 1 wshi wshi  22533501 Nov 20  2024 toolchains-linux-x64-5.0.1.111-Release.zip   # Utilities (e.g., signing tool, device connector)
   ```
After decompressing these 5 compressed packages, the SDK will be ready. Lycium will then need to use the cross-toolchain from this SDK to compile third-party libraries.
# Lycium Cross-Compilation Framework

## Introduction

Lycium is a compilation framework tool designed to assist developers in quickly cross-compiling C/C++ third-party libraries using shell scripts and validating them on the OpenHarmony system. Developers only need to configure the compilation method and parameters for the corresponding C/C++ third-party library, and Lycium can rapidly generate binary files that can run on the OpenHarmony system.

## Directory Structure Overview

```shell
lycium  
├── doc                   # Documentation related to the Lycium tool  
├── Buildtools            # Instructions for preparing the compilation environment  
├── CItools               # Documentation for setting up the testing environment  
├── script                # Scripts dependencies for the project  
├── template              # Build/test templates for libraries in the thirdparty directory  
├── build.sh              # Top-level build script  
├── test.sh               # Top-level test script  
```

## Compiling resiprocate Using the Lycium Tool

First, obtain the Lycium tool and the Lycium package:

```shell
git clone https://gitcode.com/openharmony-sig/tpc_c_cplusplus.git
```

Then, navigate to the Lycium directory and set the [OHOS_SDK](#ohos_sdk) environment variable. This allows Lycium to locate the NDK in the OpenHarmony SDK and use its build tools and Clang toolchain to compile third-party libraries.

```shell
cd lycium  
export OHOS_SDK=~/workspace/ohos-sdk/linuxs
```

Use Lycium to compile resiprocate:
s
```shell
./build.sh resiprocate
```

Wait until the following message appears on the screen:

```
ALL JOBS DONE!!!
```

This indicates a successful compilation.

At this point, you can find two folders, `arm64-v8a` and `armeabi-v7a`, in `tpc_c_cplusplus/lycium/usr/resiprocate`, representing the compiled 64-bit and 32-bit resiprocate libraries, respectively.