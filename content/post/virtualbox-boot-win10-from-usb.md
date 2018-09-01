+++
categories = []
date = "2018-09-01T09:09:31+08:00"
description = "將windows 10安裝在USB外接硬碟中並使用Virtualbox開機"
keywords = ["win10", "virtualbox"]
title = "使用Virtualbox對外接USB硬碟中的Windows10開機"

+++


準備事項：

- Mac一台
- usb外接硬碟（建議SSD）
- 安裝好Virtualbox
- 基本的termainal操作

#### 建立USB外接硬碟的vmdk檔案

開啟`磁碟工具程式`將外接硬碟的所有磁區卸除(unmount)
![](/image/vb-win10-unmount.png)
卸除後磁碟會以灰色字顯示
![](/image/vb-win10-after-unmount.png)


開啟terminal並輸入`diskutil list`找到自己的外接硬碟代號
```
/dev/disk4 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *250.1 GB   disk4
   1:               Windows_NTFS 系統保留                575.7 MB   disk4s1
   2:               Windows_NTFS BOOTCAMP                249.5 GB   disk4s2
```
根據自己的硬碟代號建立vmdk檔案使用terminal輸入指令
```bash
sudo VBoxManage internalcommands createrawvmdk -filename ~/VirtualBox\ VMs/disk4.vmdk -rawdisk /dev/disk4
```
其中`/dev/disk4`請替換成自己的硬碟代號，`~/VirtualBox\ VMs/disk4.vmdk`替換成vmdk檔案要放置的路徑

將磁碟與vmdk檔案權限改為777
```bash
sudo chmod 777 ~/VirtualBox\ VMs/disk4.vmdk
sudo chmod 777 /dev/disk4
```

#### 於Virtualbox建立虛擬機

![](/image/vb-create-vm.png)

- 輸入名稱
- 選擇作業系統類型與版本
- 調整記憶體大小
- 硬碟指定為`使用現有虛擬硬碟檔案`將檔案選為剛剛產生的vmdk檔案

接著就可以對虛擬機開機，就可以直接對外接硬碟開機並使用
![](/image/vm-win10-booting.png)

若外接硬碟為空的，尚未安裝Windows可以直接透過Virtualbox安裝（插入windows 10安裝ISO檔）

平時使用windows都是使用外接硬碟開機，需要將電腦重開，且無法同時使用MacOS，需要同時使用Winsows與MacOS時可以使用此方法！且資料與獨立開機時一致