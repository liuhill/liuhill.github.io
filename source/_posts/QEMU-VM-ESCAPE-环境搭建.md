---
title: QEMU VM ESCAPE 环境搭建
date: 2018-04-28 12:17:26
tags:
    - 虚拟机
    - qemu
    - vm
categories:
  - 技术
---
# QEMU VM ESCAPE 环境搭建
## 0.参考
- https://bbs.pediy.com/thread-225211.htm
- https://dangokyo.me/2018/03/02/qemu-escape-part-1-environment-set-up/
- https://dangokyo.me/2018/03/08/qemu-escape-part-2-debugging-environment-set-up/

## 1.安装操作系统 `ubuntu 17.10`
```
http://mirrors.163.com/ubuntu-releases/17.10/ubuntu-17.10.1-desktop-amd64.iso
```

## 2. 安装指定版本的qemu
不要使用make install. 后续使用绝对路径执行qemu
```
$ git clone git://git.qemu-project.org/qemu.git
$ cd qemu
$ git checkout bd80b59
$ mkdir -p bin/debug/native
$ cd bin/debug/native
$ ../../../configure --target-list=x86_64-softmmu --enable-debug \
$                    --disable-werror
$ make -j 4
```

## 3. 创建ubuntu镜像,用户qemu运行虚拟机
### 3.1) 下载linux源代码
```
https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.15.18.tar.xz
```
### 3.2) 编译内核
可能需要安装一下依赖包，发现错误百度一下即可。
```
make defconfig
make kvmconfig
make -j 4
```
### 3.3) 创建Debian-Stretch image
- 3.3.1） 安装debootstrap工具
```
sudo apt-get install debootstrap
```
- 3.3.2）安装Debian镜像需要的依赖包
```
mkdir qemu
sudo debootstrap --include=openssh-server,curl,tar,gcc,\
libc6-dev,time,strace,sudo,less,psmisc,\
selinux-utils,policycoreutils,checkpolicy,selinux-policy-default \
stretch qemu
```

- 3.3.4） 创建镜像
这个镜像配置后，可以通过ssh使用root用户免密钥登录。
**脚本中 enp0s3为host机网卡的名称，通过ifconfig先查看后，修改脚本保持一至*

```
set -eux

# Set some defaults and enable promtless ssh to the machine for root.
sudo sed -i '/^root/ { s/:x:/::/ }' qemu/etc/passwd
echo 'T0:23:respawn:/sbin/getty -L ttyS0 115200 vt100' | sudo tee -a qemu/etc/inittab
printf '\nauto enp0s3\niface enp0s3 inet dhcp\n' | sudo tee -a qemu/etc/network/interfaces
echo 'debugfs /sys/kernel/debug debugfs defaults 0 0' | sudo tee -a qemu/etc/fstab
echo "kernel.printk = 7 4 1 3" | sudo tee -a qemu/etc/sysctl.conf
echo 'debug.exception-trace = 0' | sudo tee -a qemu/etc/sysctl.conf
echo "net.core.bpf_jit_enable = 1" | sudo tee -a qemu/etc/sysctl.conf
echo "net.core.bpf_jit_harden = 2" | sudo tee -a qemu/etc/sysctl.conf
echo "net.ipv4.ping_group_range = 0 65535" | sudo tee -a qemu/etc/sysctl.conf
echo -en "127.0.0.1\tlocalhost\n" | sudo tee qemu/etc/hosts
echo "nameserver 8.8.8.8" | sudo tee -a qemu/etc/resolve.conf
echo "ubuntu" | sudo tee qemu/etc/hostname
sudo mkdir -p qemu/root/.ssh/
rm -rf ssh
mkdir -p ssh
ssh-keygen -f ssh/id_rsa -t rsa -N ''
cat ssh/id_rsa.pub | sudo tee qemu/root/.ssh/authorized_keys

# Build a disk image
dd if=/dev/zero of=qemu.img bs=1M seek=2047 count=1
sudo mkfs.ext4 -F qemu.img
sudo mkdir -p /mnt/qemu
sudo mount -o loop qemu.img /mnt/qemu
sudo cp -a qemu/. /mnt/qemu/.
sudo umount /mnt/qemu
```

## 4.运行qemu
```
../Security/qemu/bin/debug/build/x86_64-softmmu/qemu-system-x86_64   \
-kernel /home/dango/Kernel/linux-4.15.7/arch/x86/boot/bzImage  \
-append "console=ttyS0 root=/dev/sda rw"  \
-hda /home/dango/Kernel/Image/image03/qemu.img  \
-enable-kvm -m 2G -nographic \
-netdev user,id=t0, -device rtl8139,netdev=t0,id=nic0 \
-netdev user,id=t1, -device pcnet,netdev=t1,id=nic1
```
- `../Security/qemu/bin/debug/build/x86_64-softmmu/qemu-system-x86_64`为第2步生成qemu程序路径
- `/home/dango/Kernel/linux-4.15.7/arch/x86/boot/bzImage` 为第3.2中生成的路径
- `/home/dango/Kernel/Image/image03/qemu.img` 为第3.3.4中生成文件

虚拟机运行起来后，提示输入用户名root,即登录guest虚拟机。


## 5.测试qemu。
在第4步中运行起的客户机,在guest 虚拟机执行如下操作

### 5.1 用vim创建c源代码文件mmu.c
```
//mmu.c
#include <stdio.h>
#include <string.h>
#include <stdint.h>
#include <stdlib.h>
#include <fcntl.h>
#include <assert.h>
#include <inttypes.h>

#define PAGE_SHIFT  12
#define PAGE_SIZE   (1 << PAGE_SHIFT)
#define PFN_PRESENT (1ull << 63)
#define PFN_PFN     ((1ull << 55) - 1)

int fd;

uint32_t page_offset(uint32_t addr)
{
    return addr & ((1 << PAGE_SHIFT) - 1);
}

uint64_t gva_to_gfn(void *addr)
{
    uint64_t pme, gfn;
    size_t offset;
    offset = ((uintptr_t)addr >> 9) & ~7;
    lseek(fd, offset, SEEK_SET);
    read(fd, &pme, 8);
    if (!(pme & PFN_PRESENT))
        return -1;
    gfn = pme & PFN_PFN;
    return gfn;
}

uint64_t gva_to_gpa(void *addr)
{
    uint64_t gfn = gva_to_gfn(addr);
    assert(gfn != -1);
    return (gfn << PAGE_SHIFT) | page_offset((uint64_t)addr);
}

int main()
{
    uint8_t *ptr;
    uint64_t ptr_mem;

    fd = open("/proc/self/pagemap", O_RDONLY);
    if (fd < 0) {
        perror("open");
        exit(1);
    }

    ptr = malloc(256);
    strcpy(ptr, "Where am I?");
    printf("%s\n", ptr);
    ptr_mem = gva_to_gpa(ptr);
    printf("Your physical address is at 0x%"PRIx64"\n", ptr_mem);

    return 0;
}
```
### 5.2 用gcc编译如下代码
如果没有先安装gcc,编译可能会有一些warning，不用在意。
```
gcc -c  mmu.c
```

### 5.3 执行程序
```
./mmu.o
```
