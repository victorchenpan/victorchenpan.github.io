## 一.概述：<br>

 众所周知，如果要跑linux kernel(此时先不讨论挂载文件系统这些工作)，我们得先有一个bootloader,那下面就说下其中的一个embedded bootloader-----uboot;一般的开发流程就是：
 a.先下载uboot源码，在主机上通过交叉编译器得到uboot.bin等映像文件，b.然后下载到flash当中，（如果之前开发板中就已经有了bootloader的话，可以通过tftp把主机上的映像先下载到ram中，再写入到flash或emmc中去），c.修改完环境变量再写入到flash中，然后重启就完成了整个bootloader的移植工作。因此最主要的第一步就是编译出合适的uboot映像文件出来。

---

## 二.源码的获取：<br>

- a.从硬件开发商官网中下载符合自己板卡的uboot代码库。
- b.从git://git.denx.de/u-boot.git下载board中含有自己开发板的代码库，虽然后面新版本的大部分都包含以前的旧库，但还是不建议下载最新的。

---

## 三.源码的编译：<br>
 - a.从硬件开发商官网下载的代码是经开发商修改好了的，我们可以节省很多时间，因此我们只要查看.../README就可以快速的修改成更适合自己的代码库。
 - b.从git://git.denx.de/u-boot.git下载的代码库，我们要修改的东西就多了点，先在makefile中指定好CORSS_COMPILE,通过make menuconfig 选择好cpu的架构，开发板类型名，uboot命令等选项，保存生成我们的.config文件（如果.../configs下有符合的配置文件的话，可以直接make xxx_deconfig）,然后make all。不过make all通常不会很顺利，会有各种报错产生，一些常规的报错好解决，有时会出现一些我们从没遇到过的错误，通过google也很难找到好方法。比如今天我遇到了一个错误：arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12195
arm-linux-ld: BFD (GNU Binutils) 2.20.1.20100303 assertion fail /work/toolchain/build/src/binutils-2.20.1/bfd/elf32-arm.c:12429
Segmentation fault
Makefile:1171: recipe for target 'u-boot' failed
make: *** [u-boot] Error 139
刚开始还以为是编译器的原因，因为用老的编译器去编译最新的代码库很多时候是不行的，得更新自己的编译器才行，我使用的系统为debian，通过apt-cache search arm gcc找到了gcc-arm-none-eabi，把它下载下来了之后发现还是不行，后来才发现原来是自己失误，没有在Makefile中指定好ARCH，从而导致的。
指定好了ARCH＝arm之后，再make下，根目录下为：
api               .config    dts         Kbuild       MAKEALL   snapshot.commit  u-boot.bin       u-boot.lds             .u-boot.srec.cmd
arch              config.mk  examples    Kconfig      Makefile  System.map       .u-boot.bin.cmd  .u-boot.lds.cmd        u-boot.sym
board             configs    fs          lib          net       test             u-boot.cfg       u-boot.map             .u-boot.sym.cmd
.checkpatch.conf  disk       .git        Licenses     post      tools            .u-boot.cfg.cmd  u-boot-nodtb.bin
cmd               doc        .gitignore  .mailmap     README    .travis.yml      .u-boot.cfg.d    .u-boot-nodtb.bin.cmd
common            drivers    include     MAINTAINERS  scripts   u-boot           .u-boot.cmd      u-boot.srec
下面就得知道如何从已有的board下，把和自己板卡硬件类似的板卡库改成符合自己板卡的库了，这部分放在下一篇blog中来记录！

---

## 四.小经验总结
> 1.在进行uboot移植工作时，不能因为看上去很easy就图快，因为无意间就会忘记一些细节从而浪费更多的时间来弥补。

>2.如果想减小移植工作，可以从cpu开发商官网上去下载他们已经修改过的uboot库（或者称为板级支持包bsp）,而不直接从git://git.denx.de/u-boot中下载。

>3.在此推荐一个比较好用的翻墙工具：lantern
