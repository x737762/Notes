# ArchLinux 之旅 系统安装

## 安装前准备

1. 制作 U 盘启动，推荐使用工具[Etcher](https://www.balena.io/etcher/)

2. 启动 u 盘系统，验证引导模式。

```bash
ls /sys/firmware/efi/efivars
```

如果命令结果显示了目录且没有报告错误，则系统以 UEFI 模式引导。 如果目录不存在，则系统可能以 BIOS 模式 (或 CSM 模式) 引导。

3. 连接网络

   - 查看是否启用网络接口。

   ```bash
   ip link
   ```

   - 检查网络连接状态

   ```bash
   ping www.baidu.com
   ```

   - 使用 NetworkManager 连接网络。

   a. 启动 NetworkManager

   ```bash
   systemctl start NetworkManager
   ```

   b. 查看可用 wifi

   ```bash
   nmcli device wifi list
   ```

   c. 连接 wifi

   ```bash
   nmcli device wifi connect [BSSID|SSID] password password
   ```

4. 同步系统时间

```bash
timedatectl
```

## 建立应该盘分区

**分区模式任选一种，目前基本都是 `UEFI`**

> 所有分区以实际分区为准
>
> 本文分区说明：
>
> - `/dev/sda1`：efi 分区。
> - `/dev/sda2`：交换分区。
> - `/dev/sda3`：根分区。

**BIOS 与 MBR**

| 挂载点 | 分区     | 分区类型   |
| ------ | -------- | ---------- |
| SWAP   | 交换分区 | Linux swap |
| /mnt   | 根分区   | Linux      |

**UEFI 与 GPT**

| 挂载点               | 分区                      | 分区类型     |
| -------------------- | ------------------------- | ------------ |
| /mnt/boot 或/mnt/efi | efi 系统分区（大于 512M） | EFI 系统分区 |
| SWAP                 | 交换分区                  | Linux swap   |
| /mnt                 | 根分区                    | Linux        |

- 格式化分区

```bash
# 格式化eif分区
mkfs.fat -F 32 /dev/sda1

# 格式化交换分区
mkswap /dev/sda2

# 挂载根分区
mkfs.ext4 /dev/sda3
```

- 挂载分区

```bash
# 挂载根分区
mount /dev/sda3 /mnt

# 挂载交换分区
swapon /dev/sda2
```

## 安装系统

- 修改镜像源

```bash
# 选择最近12小时同步的，并且位于中国的镜像，然后根据下载速度排序，最后将结果覆写到`/etc/pacman.d/mirrorlist`
reflector --country China --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

- 安装系统

```bash
pacstrap -K /mnt base base-devel linux linux-firmware vim
```

- 生成 fstab 文件

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

## 配置系统

- 切换到新系统

```bash
arch-chroot /mnt
```

- 设置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

- 生成 /etc/adjtime

```bash
hwclock --systohc
```

- 本地化

  - 编辑 `/etc/locale.gen` 文件，
  - 去掉 `en_US.UTF-8 UTF-8`、`zh_CN.UTF-8 UTF-8`、`zh_CN.GBK GBK`、`zh_CN GB2312`的注释。
  - 执行 `locale-gen`生成 `locale`信息。

- 设置系统语言

```bash
vim /etc/locale.conf
LANG=en_US.UTF-8
```

- 修改主机名

```bash
vim /etc/hostname
myhostname # 主机名
```

- 添加 `hosts`文件（可以省略）

  - 一些客户端应用仍然会依赖于 `/etc/hosts`。
  - 要阻止程序通过网络非安全的解析 localhost，请将 localhost 加入 hosts 文件:

  ```bash
    vim /etc/hosts

    #添加下面内容
    127.0.0.1	localhost
    ::1			localhost
    127.0.1.1	myhostname.localdomain myhostname # 主机名
  ```

- 安装网络管理工具

```bash
pacman -S networkmanager
systemctl enable NetworkManager # 设置开机启动
```

## 安装微码和驱动

- 安装微码

**根据自己电脑型号安装，不需要全部安装。**

```bash
# amd cpu 安装这个
pacman -S amd-ucode

# intel cpu 安装这个
pacman -S intel-ucode
```

- 安装驱动

```bash
# nvidia 显卡
pacman -S nvidia nvidia-utils

# amd 显卡或核显
pacman -S xf86-video-amdgpu

# intel 核显
pacman -S xf86-video-intel
```

## 安装引导程序

- 安装引导工具

```bash
pacman -S grub efibootmgr
```

- **如果是 BIOS 启动，请看这个**

```bash
grub-install --target=i386-pc /dev/sdX # /dev/sdX: 整块磁盘
```

- **如果是 UEFI 启动，请看这个**

  - 挂载 EFI 系统分区

  ```bash
  mkdir /boot/efi
  mount /dev/sda1 /boot/efi
  ```

  - 安装引导

  ```bash
  grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Arch
  ```

- 多系统设置

```bash
pacman -S os-prober

vim /etc/default/grub

# 取消以下代码注释，没有的话在末尾添加
GRUB_DISABLE_OS_PROBER=false
```

**注意：** 如果检测不到 Windows 的分区，可尝试安装 NTFS-3G。

- 生成配置文件

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

- 设置 root 密码

```bash
passwd
```

## 安装完成

退出 chroot 环境，重启系统。

```bash
exit
umount -R /mnt
reboot
```
