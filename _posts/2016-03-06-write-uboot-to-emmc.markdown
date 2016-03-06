如何将u-boot.bin写入到emmc当中
===
## 一.首先将u-boot写入SD卡中从SD卡启动<br>

### 进入三星提供的u-boot源码中的/sd_fuse/tiny4412执行sd_fusing.sh<br>
> cd .../sd_fuse/tiny4412/ && sudo ./sd_fusing.sh /dev/mmcblk0 <br> (...表示u-boot源码根目录，mmcblk0名称在不同系统中的名字有时会不一样)，<br>然后把SD卡插入，并让开发板从SD卡启动。<br>

## 二.将uboot写入emmc，并通过EMMC启动<br>

### mmcinfo 0 : 查看mmc卡信息。0 表示SD卡; 1表示emmc卡<br>
> TINY4412 # fdisk -p 1<br>
partion #    size(MB)     block start #    block count    partition_Id <br>
   1           695          6070812         1424478          0x0C<br>
   2           320           134244          656304          0x83<br>
   3          2057           790548         4213770          0x83<br>
   4           520          5004318         1066494          0x83<br>

### 格式化emmc卡 （一般分四个区）<br>
> TINY4412 # fdisk -c 1 320 809 524<br>
Count: 10000<br>
Count: 9999<br>
Count: 9998<br>
Count: 9997<br>
fdisk is completed<br>
<br>
partion #    size(MB)     block start #    block count    partition_Id <br>
   1          1937          3527634         3967656          0x0C<br>
   2           320           134244          656304          0x83<br>
   3           812           790548         1663134          0x83<br>
   4           524          2453682         1073952          0x83<br>
<br>
 >TINY4412 # fatformat mmc 1:1<br>
Start format MMC&d partition&d ...<br>
Partition1: Start Address(0x35d3d2), Size(0x3c8aa8)<br>
size checking ...<br>
Under 8G<br>
write FAT info: 32<br>
Fat size : 0xf22<br>
Erase FAT region.................................................<br>
Partition1 format complete.<br>
TINY4412 # ext3format mmc 1:2<br>
Start format MMC1 partition2 ....<br>
\*\* Partition2 is not ext2 file-system 1 \*\* <br>
Partition2: Start Address(0x20c64), Size(0xa03b0)<br>
Start ext2format...<br>
Wirte 0/3block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x20c64<br>
Erase inode table(0) - 0x20d24..............<br>
d_indirect_point:0x24874<br>
Wirte 1/3block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x60c64<br>
Erase inode table(1) - 0x60d24..............<br>
Wirte 2/3block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0xa0c64<br>
Erase inode table(2) - 0xa0c74..............<br>
TINY4412 # ext3format mmc 1:3<br>
Start format MMC1 partition3 ....<br>
\*\* Partition3 is not ext2 file-system 1 \*\* <br>
Partition3: Start Address(0xc1014), Size(0x19609e)<br>
Start ext2format...<br>
Wirte 0/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0xc1014<br>
Erase inode table(0) - 0xc11c4...............<br>
d_indirect_point:0xc4f64<br>
Wirte 1/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x101014<br>
Erase inode table(1) - 0x1011c4...............<br>
Wirte 2/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x141014<br>
Erase inode table(2) - 0x141024...............<br>
Wirte 3/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x181014<br>
Erase inode table(3) - 0x1811c4...............<br>
Wirte 4/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x1c1014<br>
Erase inode table(4) - 0x1c1024...............<br>
Wirte 5/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x201014<br>
Erase inode table(5) - 0x2011c4...............<br>
Wirte 6/7block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x241014<br>
Erase inode table(6) - 0x241024...............<br>
TINY4412 # ext3format mmc 1:4<br>
Start format MMC1 partition4 ....<br>
\*\* Partition4 is not ext2 file-system 1 \*\*<br>
Partition4: Start Address(0x2570b2), Size(0x106320)<br>
Start ext2format...<br>
Wirte 0/5block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x2570b2<br>
Erase inode table(0) - 0x2571d2..............<br>
d_indirect_point:0x25acaa<br>
Wirte 1/5block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x2970b2<br>
Erase inode table(1) - 0x2971d2..............<br>
Wirte 2/5block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x2d70b2<br>
Erase inode table(2) - 0x2d70c2..............<br>
Wirte 3/5block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x3170b2<br>
Erase inode table(3) - 0x3171d2..............<br>
Wirte 4/5block-group<br>
Reserved blocks for jounaling : 4102<br>
Start write addr : 0x3570b2<br>
Erase inode table(4) - 0x3570c2..............<br>

### 将bl1.bin, bl2.bin, u-boot.bin, tzsw.bin烧录到eMMC中<br>
> TINY4412 # emmc open 1<br>
eMMC OPEN Success.!!<br>
                        !!!Notice!!!<br>
!You must close eMMC boot Partition after all image writing!<br>
!eMMC boot partition has continuity at image writing time.!<br>
!So, Do not close boot partition, Before, all images is<br> written.!<br>
TINY4412 # dnw 50000000<br>
OTG cable Connected!<br>
Now, Waiting for DNW to transmit data<br>
Download Done!! Download Address: 0x50000000, Download<br> Filesize:0x2000<br>
Checksum is being calculated.<br>
Checksum O.K.<br>
TINY4412 # mmc write 1 0x50000000 0 0x10<br>
<br>
MMC write: dev # 1, block # 0, count 16 ... 16 blocks written:<br> OK<br>
TINY4412 # dnw 50000000<br>
OTG cable Connected!<br>
Now, Waiting for DNW to transmit data<br>
Download Done!! Download Address: 0x50000000, Download<br> Filesize:0x3800<br>
Checksum is being calculated.<br>
Checksum O.K.<br>
TINY4412 # mmc write 1 0x50000000 0x10 0x1c<br>
<br>
MMC write: dev # 1, block # 16, count 28 ... 28 blocks written:<br> OK<br>
TINY4412 # dnw 0x50000000<br>
OTG cable Connected!<br>
Now, Waiting for DNW to transmit data<br>
Download Done!! Download Address: 0x50000000, Download<br> Filesize:0x3ea64<br>
Checksum is being calculated.<br>
Checksum O.K.<br>
TINY4412 # mmc write 1 0x50000000 0x30 0x21d<br>
<br>
MMC write: dev # 1, block # 48, count 541 ... 541 blocks<br> written: OK<br>
TINY4412 # dnw 0x50000000<br>
OTG cable Connected!<br>
Now, Waiting for DNW to transmit data<br>
Download Done!! Download Address: 0x50000000, Download<br> Filesize:0x17000<br>
Checksum is being calculated.<br>
Checksum O.K.<br>
TINY4412 # mmc write 0x50000000 1 0x2c0 0xb8<br>
MMC Device 1342177280: N/A<br>
TINY4412 # mmc write 0x50000000 1 0x2c0 0xb8<br>
MMC Device 1342177280: N/A<br>
TINY4412 # emmc close<br>
Usage:<br>
Open/Close eMMC boot Partition<br>
TINY4412 # emmc close 1<br>
eMMC CLOSE Success.!!<br>
`在这里说下mmcwrite后面的四个参数，第一个参数：1（是指第一分区），`<br>  `第二个参数：0x50000000是刚才用dnw下载到开发板RAM当中的文件地址，`<br>
`第三个参数：是文件在emmc第一分区中的起始地址（以block为单位，即512B）`<br>
`第四个参数：是文件所要占用的emmc空间（以block为单位，即512B）`<br>
`emmc open 1 和emmc close 1是对emmc进行操作和停止操作的命令`<br>
