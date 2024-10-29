# ARCH Linux 安装指南 —— 报错整理（持续更新。。。）

### 1.  NVIDIA  驱动安装

###### 安装 NVIDIA 相关驱动

```bash
sudo pamcan -Syu

sudo pacman -S nvidia-open nvidia-settings lib32-nvidia-utils 
```

###### 禁用内核中的 nouveau 模块

方法一： 

```bash
sudo vim /etc/mkinitcpio.conf  # 在 mkinitcpio.conf 中 HOOKS 一行删除 kms 并保存

sudo mkinitcpio -P 

reboot
```

方法二： 

```bash
sudo vim /etc/default/grub  # 在 GRUB_CMDLINE_LINUX_DEFAULT="" 中加入 nomodeset nouveau.modeset=0 

sudo grub-mkconfig -o /boot/efi/EFI/GRUB/grub.cfg

reboot
```

> [!WARNING]
>
> 在重新生成 grub 时，记得提前挂载 efi 分区，否则重新生成的 grub 配置文件无效

重启电脑后，终端运行 nvidia-settings 查看显卡信息，观察 NVIDIA 驱动是否启用。

### 2.  Timeshift 与 Snapper

###### Timeshift：

对于使用 Timeshift 创建 btrfs 格式的快照，必须按照 ubuntu 格式创建子卷，否则会无法创建快照，具体步骤按照[官方教程](https://github.com/linuxmint/timeshift)所示。

###### Snapper

对于使用 Snapper 创建 btrfs 格式的快照。

```bash
snapper -c config_name create-config -f btrfs /  
# 为根卷创建snapper的配置文件 
# config_name 是配置文件的文件名，需要自己定义

snapper -c config_name create -c number --d “desc” 
# 使用 number 清理算法创建快照

snapper -c config_name --utc list 
# 以UTC方式显示日期和时间，查看创建的快照

snapper -c config_name undochange Num
# 将根卷下所有文件回滚到Num号快照状态
```

> [!WARNING]
>
> 在指定需要创建快照的卷之前，需要将其挂载

其他命令参考 [Snapper](https://wiki.archlinux.org/title/Snapper)。



### References

[archlinux 简明指南](https://arch.icekylin.online/)

[Snapper](https://wiki.archlinuxcn.org/wiki/Snapper)



 

 

 
