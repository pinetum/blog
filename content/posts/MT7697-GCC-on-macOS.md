---
categories : []
date : "2017-10-04T19:11:31+08:00"
description : "因緣際會獲得了Linkit 7697 HDK 開發版，但官方僅針對windows與Ubuntu提供SDK，如果想在macOS上開發需要對官方的SDK加一點料。"
keywords : []
title : "於macOS上建立MT7697開發環境（使用GCC）"

---

因緣際會獲得了Linkit 7697 HDK 開發版，但官方僅針對windows與Ubuntu提供SDK，如果想在macOS上開發需要對官方的SDK加一點料。

#### 本文大綱
- 下載官方SDK
- 下載GNU Arm Embedded Toolchain for macOS
- 將SDK中的Toolchain替換成macOS版本
- 下載Flash tool
- 修改Makefile支援燒錄
- 開發流程


#### 下載官方SDK

最新版本的SDK可於下方連結下載，目前最新版本為4.6.0
[https://docs.labs.mediatek.com/resource/mt7687-mt7697/zh_tw/downloads](https://docs.labs.mediatek.com/resource/mt7687-mt7697/zh_tw/downloads)

#### 下載GNU Arm Embedded Toolchain for macOS

我使用的版本為 `4.8.3-2014q1`

[https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)

#### 替換Toolchain
於官方下載的SDK中所附的Toolchain為給Ubuntu使用，需要使用macOS版本的Toolchain覆蓋

將官方SDK與toolchain解壓縮

toolchain解壓縮後會有以下資料夾，將`LinkIt_SDK_V4.6.0_public/tools/gcc/gcc-arm-none-eabi`資料夾中內容清空，並使用新下載的內容替換
```bash
arm-none-eabi
bin
lib
share
```

#### 下載Flash tool

```bash
cd path/to/LinkIt_SDK_V4.6.0_public/tools
git clone https://github.com/MediaTek-Labs/mt76x7-uploader flash_tool
```

#### 修改Makefile支援燒錄

於專案下的GCC資料夾中的Makefile增加flash，往後僅需要make flash即可燒錄並開啟serial port

```
flash:
    $(SOURCE_DIR)/tools/flash/upload.py -c /dev/cu.SLAB_USBtoUART -n $(SOURCE_DIR)/tools/flash//da97.bin -f $(OUTPATH)/$(PROJ_NAME).bin
    screen /dev/tty.SLAB_USBtoUART 115200
```

#### 開發流程

- 新建專案
```bash
cd path/to/LinkIt_SDK_V4.6.0_public/project/linkit7697_hdk/templates/
cp -r freertos_initialize_main_features ../apps/project_name
```

- 編譯
```bash
cd path/to/LinkIt_SDK_V4.6.0_public/project/linkit7697_hdk/apps/project_name/GCC
make -j8
```

- 燒錄/開啟serial port
```bash
make flash
```
