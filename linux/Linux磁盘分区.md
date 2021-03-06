## 分区

>磁盘分区分为三步：磁盘分区、格式化磁盘、挂载磁盘

### 介绍

为什么要进行分区？

- 操作系统在启动的时候会通过磁盘的引导来获得操作系统文件所在的分区，所以必须有一个可引导分区。这也就是为什么新买的硬盘不能直接用的原因。
- 分区是可以提高管理效率，所有东西放在一个分区，OS管理起来效率比较低，因为每次要检索的东西太多了。

现在的主流分区有 MBR 和 GPT

#### MBR 主引导记录
主引导记录（Master Boot Record，缩写：MBR），又叫做主引导扇区，
是计算机开机后访问硬盘时所必须要读取的首个扇区。

硬盘分区表占据主引导扇区的64个字节，
可以对四个分区的信息进行描述，其中每个分区的信息占据16个字节。

如果某一分区在硬盘分区表的信息如下：

80 01 01 00 0B FE BF FC 3F 00 00 00 7E 86 BB 00

MBR 只可以有4个分区，在实际使用中很多时候4个分区是不够使用的，所以更多的分区就要由扩展分区来实现，扩展分区可以有很多个


#### GPT 分区

全局唯一标识分区表（GUID Partition Table，缩写：GPT）
是一个实体硬盘的分区表的结构布局的标准。它是可扩展固件接口（EFI）标准
（被Intel用于替代个人计算机的BIOS）的一部分
所以如果使用 GPT 分区如果要作为启动盘，主板必须支持 UEFI 才可以

>在MBR硬盘中，分区信息直接存储于主引导记录（MBR）中（主引导记录中还存储着系统的引导程序）。
但在GPT硬盘中，分区表的位置信息储存在GPT头中。但出于兼容性考虑，
硬盘的第一个扇区仍然用作MBR，之后才是GPT头

##### 查看磁盘状态`df`
- `df`查询磁盘空间（默认）
- `df -h`查询磁盘空间（自动换算空间）

##### 查询指定目录的磁盘使用空间 `du`
- `du -sh .`查询当前目录的所占磁盘空间
- `du -sh 指定目录`查询指定目录所占磁盘空间
- `du -h 指定目录`查询指定目录下所有目录所占磁盘空间
- `du -ah 指定目录`查询指定目录下的所有目录及文件所占磁盘空间


### 磁盘分区工具 `fdisk`

`fdisk -l`查询磁盘物理信息

磁盘分区
`fdisk 指定磁盘`使用 fdisk 命令打开磁盘。如`fdisk /dev/sdb`。进入磁盘分区页面。
- 输入 m 可以获取帮助信息。
- 输入 p 可以获取当前磁盘分区信息。
- 输入 n 创建新的分区。
- 分配完分区可以通过 p 来查看结果。
- w 或 q 保存/退出
刚刚分配完的分区只是保存在内存中，并没有实际应用到磁盘，如果此时点击 q，分区工具会直接退出，刚刚的更改并不会生效。如果想要叫分区生效，输入 w，保存退出，就会直接将修改生效到磁盘，此步骤要万分小心，因为一担保存后就立即生效，如果原本分区中有数据就会丢失。

## 格式化磁盘

磁盘分完区还要先磁盘格式化才可以使用，如果要使用 lvm 逻辑卷就不要格式化（lvm 部分再说明）。

#### Linux 下的主流格式

##### ext2、ext3、ext4

ext 家族在 Linux 使用的时间最长，现在很多发行版本默认也会使用这个格式。
随着数量来的增大，现在 ext4 就有一些明显的限制，最大文件大小是 16 TB（tebibytes），
对于普通磁盘来说已经非常大了，但是一个大数据的处理或者使用了磁盘阵列后，
这个就会不够用了。并且因为数据验证算法的原因，当磁盘容量大的时候，
格式化磁盘的时间会非常的长。

##### xfs

XFS 是一个全64-bit的文件系统，它可以支持上百万T字节的存储空间。对特大文件及小尺寸文件的支持都表现出众，支持特大数量的目录。最大可支持的文件大 小为263 = 9 x 1018 = 9 exabytes，最大文件系统尺寸为18 exabytes。
总体来说，XFS的各方面现在都已经超过了 ext4，所以现在很多新版本的 Linux 发行版本中，
都已经替换成了 xfs。

##### mkfs 格式化磁盘

`mkfs.ext4 指定磁盘`格式化成 ext 4`mkfs.ext4 /dev/sdb1`
`mkfs.xfs 指定磁盘`格式化成 xfs`mkfs.xfs /dev/sdb2`

## 挂载磁盘 `mount`

磁盘格式化完成后要将磁盘挂载才可以使用，在Windows下，其实也有这个步骤，
不过一般都是和格式化磁盘同时进行了，Windows下叫驱动器号，就是平常所说的C盘、D盘，
Linux下叫做挂在点，在前面已经介绍过，Linux是由 / 开始的目录结果，
所以刚刚格式化好的磁盘理论上可以挂载到任意一个目录下，但是一般需要挂载点有意义一些，
并且保证原本的目录是空的，否则会将原目录的数据覆盖，不能再使用。

`mount 指定的磁盘分区 指定目录` 将指定的磁盘分区挂载到指定目录