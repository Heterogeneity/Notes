### 问题：

使用`df -h`命令查看虚拟机磁盘空间，发现根目录空间需要扩容

![image](https://github.com/Heterogeneity/Notes/assets/102458836/62fcf7f9-296c-442b-8cdd-0572bb99116b)


### 解决方法：

关闭虚拟机，编辑虚拟机设置，点击硬盘，选择扩展，输入期望的扩展后的空间大小。

开启虚拟机，输入`lsblk`，查看硬盘是否已经扩展

![image](https://github.com/Heterogeneity/Notes/assets/102458836/f9a5824d-6198-48f7-959a-7f68ee34e313)

图中看出，sda磁盘共有三个分区，分别挂载到启动目录（用于存放启动相关文件）、交换分区（用于虚拟内存）和根目录，我们要扩展根目录，即扩展sda3分区的空间。

我的磁盘分区类型是"part"，即标准分区。相对应的lvm是逻辑卷，即是linux内核提供的一种逻辑卷管理功能，由内核驱动和应用层工具组成，它是在硬盘的分区基础上，创建了一个逻辑层。

网上大部分的扩容教程都是针对lvm的，要扩容part类型，需要安装growpart

`yum install cloud-utils-growpart -y`

安装后执行如下两条命令：

`growpart /dev/sda 3`  用于将sda磁盘的3号分区进行物理扩容

注意事项：若出现"unexpected output in sfdisk --version"报错，则键入命令`LANG=en_US.UTF-8`，解决编码问题，仍报错重启虚拟机即可。

`xfs_growfs /dev/sda5` 用于扩容文件系统

注意事项：若出现"resize2fs: Bad magic number in super-block while trying to open ···"报错，则使用`parted -l`查看根分区的文件系统是什么类型，若是ext2、ext3、ext4文件系统，将命令中`xfs_growfs`改为`resize2fs`即可。

最后执行`df -h`查看是否扩容成功。

