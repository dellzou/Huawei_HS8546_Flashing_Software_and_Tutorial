# 华为 HS8546V5 分区与 jffs2 备份恢复工具包

配套文章：[华为 HS8546V5 光猫：13 个分区与 jffs2 备份恢复](https://xiaozou123.cn/huawei-hs8546-partition-jffs2-backup-restore/)

HS8546V5 是移动定制的千兆网关。这个仓库只做一件事：把「动手改之前先备份」这一步讲清楚所需的工具和原文教程放齐。

## 目录

- `tools/ONT_V300R017C00SPC218.exe`：ONT 组播配置工具，V3-V5 使能用
- `tools/putty.exe`：Telnet 客户端
- `tools/tftpd64.exe`：TFTP 服务端，走 TFTP 备份路线时用
- `tools/HW_Dollar2.exe`：`hw_boardinfo` / `hw_ctree` 加解密与编辑，改界面、改 SN 时用
- `tools/华为光猫配置文件加解密工具.exe`：XML / CFG 配置加解密
- `docs/HS8546V5 分区与 jffs2 备份恢复教程.txt`：宽带技术网原帖原文，U 盘与 TFTP 两条路线
- `docs/说明.txt`：原工具包说明

## 注意：仓库不含补全包

原资料包只带了使能工具，**没有带 allShell 补全固件**。补全包要按固件版本另找：

- R19：`HS8145C5_HS8145V5_V500R019_allShell.bin`
- R20：对应版本的 R20 补全包

使能和升级是两步，缺一不可。工具提示成功也可能是假成功，以 Telnet 里 `ls /mnt/jffs2` 能否列出文件为准。

## 文件与 SHA256

| 文件 | 字节 |
|---|---|
| `tools/ONT_V300R017C00SPC218.exe` | 3,770,880 |
| `tools/putty.exe` | 574,976 |
| `tools/tftpd64.exe` | 146,656 |
| `tools/HW_Dollar2.exe` | 161,280 |
| `tools/华为光猫配置文件加解密工具.exe` | 1,794,560 |
| `docs/HS8546V5 分区与 jffs2 备份恢复教程.txt` | 6,306 |
| `docs/说明.txt` | 612 |

```
a637954b61d9c15db42fb346092837d009f404ec466bdc10319b28da7b8ed8be  tools/ONT_V300R017C00SPC218.exe
a92c1d668f6d060db06535ad346221d23dec11f897d79115673206462906f698  tools/putty.exe
988292db5e71fa50b090ce2bfe66813791d944cde64c19b7f0ecda8892392a15  tools/tftpd64.exe
d3110469d8cccc7ca479105393e047faef74c74cfb34d174deece84153ac1cc8  tools/HW_Dollar2.exe
8f005af234f73a456f5e4a127ed4624c40992445caf264d5f7566cbf3ee80808  tools/华为光猫配置文件加解密工具.exe
7348455285f51f04f8018cde6d31b7057e17bab53c9f6adbf3949d5d6d03ecac  docs/HS8546V5 分区与 jffs2 备份恢复教程.txt
bd06ca5eee0baeddc6c9f20a9c8da4715524e2f5b11492f645a756fd6ec8f717  docs/说明.txt
```

## 操作顺序

1. 拔光纤，电脑网卡设 `192.168.1.2`，只连 LAN 口。
2. ONT 工具选 V5 使能，灯全灭后停止、重启。
3. 切到「升级」，选对应版本 allShell 包，灯全亮后停止、重启。
4. Telnet `192.168.1.1`，账号 `root`，密码 `adminHW` 或 `Hw8@cMcc`，然后 `su`、`shell`。
5. `ls /mnt` 确认 U 盘挂载名，再按教程备份 13 个 mtd 分区与 jffs2。

## 原文里三处对不上的地方

1. 「单独恢复 jffs2 和 APP」下的两条 `dd` 方向写反了，恢复应为 `dd if=/mnt/usb/0_9.bin of=/dev/mtdblockubi0_9`。
2. 挂载点在文中出现 `/mnt/usb` 与 `/mnt/usb1_1` 两个名字，实际以 `ls /mnt` 为准。
3. TFTP 那节的 `rm -rf *` 没有前置 `cd`，删除范围取决于当前目录。

## 免责声明

工具与教程原文均来自公开论坛资料，仅供自有设备维修与学习使用。写分区、写引导程序有变砖风险，`mtd0` 没有编程器不要碰。仓库不含任何真实设备的配置备份。
