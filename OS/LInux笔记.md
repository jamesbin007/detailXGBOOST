<!-- TOC -->

- [一、Linux文件与目录](#一linux文件与目录)
  - [1.1 Linux文件权限概念](#11-linux文件权限概念)
  - [1.2 Linux目录配置](#12-linux目录配置)
  - [1.3 文件与目录管理](#13-文件与目录管理)
- [二、pip](#二pip)
- [三、conda](#三conda)
- [四、Vim](#四vim)
- [五、其他](#五其他)

<!-- /TOC -->
# 一、Linux文件与目录

## 1.1 Linux文件权限概念

  - 切换至root身份:`su -`
  - 离开root身份:`exit`
  - 文件名第一个字符为.的文件为隐藏文件

## 1.2 Linux目录配置
- FHS针对目录树架构仅定义出三层目录下面应该放置什么数据而已，分别是下面这三个目录的定义：
  - `/`（root, 根目录）:与开机系统有关；
  - `/usr` （unix software resource）:与软件安装/执行有关；
  - `/var`（variable）:与系统运行过程有关。
- FHS要求根目录中必须存在的目录:
  - `/bin`:系统有很多放置可执行文件的目录，但`/bin`比较特殊。因为`/bin`放置的是在 单人维护模式下还能够被操作的指令。 在`/bin`下面的指令可以被root与一般帐号所使用，主要有：cat, chmod, chown, date, mv, mkdir, cp, bash等等常用的指令。
  - `/boot`:主要在放置开机会使用到的文件，包括 Linux 核心文件以及开机菜单与开机所需配置文件等等Linux kernel 常用的文件名为：vmlinuz，如果使用的是 grub2 这个开机管理程序， 则还会存在`/boot/grub2/`这个目录。
  - `/dev`:存放硬件相关的文件。在 Linux 系统上，任何设备与周边设备都是以文件的型态存在于这个目录当中的。你只要通过存取这个目录下面的某个文件，就等于存取某个设备。比要重要的文件有`/dev/null`, `/dev/zero`, `/dev/tty`, `/dev/loop`, `/dev/sd`等等。
  - `/etc`:存放配置文件的目录。系统主要的配置文件几乎都放置在这个目录内，例如人员的帐号密码档、各种服务的启始档等等。一般来说，这个目录下的各文件属性是可以让一般使用者查阅的，但是只有 root 有权力修改。FHS建议不要放置可执行文件 （binary）在这个目录中喔。比较重要的文件有： `/etc/modprobe.d/`, `/etc/passwd`, `/etc/fstab`, `/etc/issue`等等。另外 FHS 还规范几个重要的目录最好要存在 `/etc/`目录下：
    - `/etc/opt`（必要）：这个目录在放置第三方协力软件 `/opt` 的相关配置文件。
    - `/etc/X11/`（建议）：与 X Window 有关的各种配置文件都在这里，尤其是 xorg.conf 这个 X Server 的配置文件。
    - `/etc/sgml/`（建议）：与 SGML 格式有关的各项配置文件。
    - `/etc/xml/`（建议）：与 XML 格式有关的各项配置文件。
  - `/lib`:系统的函数库非常的多，而`/lib`放置的则是在开机时会用到的函数库， 以及 在`/bin`或`/sbin`下面的指令会调用的函数库而已。 什么是函数库呢？你可以将 他想成是“外挂”，某些指令必须要有这些“外挂”才能够顺利完成程序的执行 之意。 另外 FSH 还要求下面的目录必须要存在：`/lib/modules/`：这个目录 主要放置可抽换式的核心相关模块（驱动程序）
  - `/media`:media是“媒体”的英文，顾名思义，这个`/media`下面放置的就是可移除的设备！ 包括软盘、光盘、DVD等等设备都暂时挂载于此。常见的文件名 有：`/media/floppy`, `/media/cdrom`等等。
  - `/mnt`:如果你想要暂时挂载某些额外的设备，一般建议你可以放置到这个目录中。 在古早时候，这个目录的用途与`/media`相同啦！只是有了`/media`之后，这个 目录就用来暂时挂载用了。
  - `/opt`:这个是给第三方协力软件放置的目录。什么是第三方协力软件啊？ 举例来说，KDE 这个桌面管理系统是一个独立的计划，不过他可以安装到 Linux 系统中，因此KDE的软件就建议放置到此目录下了。 另外，如果你想要自行安装额外的软件（非原本的 distribution 提供的），那么也能够将你的软件安装到这里来。 不过，以前的 Linux 系统中，我们还是习惯放置在`/usr/local`目录下呢！
  - `/run`:早期的 FHS 规定系统开机后所产生的各项信息应该要放置到 `/var/run` 目录下，新版的 FHS 则规范到 `/run` 下面。 由于 `/run` 可以使用内存来仿真，因此性能上会好很多！
  - `/sbin`:存放管理员root可以执行的命令。Linux 有非常多指令是用来设置系统环境的，这些指令只有 root 才能够利用 来“设置”系统，其他使用者最多只能用来“查询”而已。放在`/sbin`下面的为开机过程中所需要的，里面包括了开机、修复、还原系统所需要的指令。至于某些服务器软件程序，一般则放置到`/usr/sbin/`当中。至于本机自行安装的软件所产生的系统可执行文件（system binary）， 则放置到`/usr/local/sbin/` 当中了。常见的指令包括：fdisk, fsck, ifconfig, mkfs等等。
  - `/srv`:srv 可以视为“service”的缩写，是一些网络服务启动之后，这些服务所需要取用的数据目录。 常见的服务例如 WWW, FTP 等等。举例来说，WWW 服务器需要的网页数据就可以放置在`/srv/www/`里面。 不过，系统的服务数据 如果尚未要提供给网际网络任何人浏览的话，默认还是建议放置到 `/var/lib`下面即可。
  - `tem`:这是让一般使用者或者是正在执行的程序暂时放置文件的地方。 这个目录是任何人都能够存取的，所以你需要定期的清理一下。当然，重要数据不可放置在此目录啊！ 因为FHS甚至建议在开机时，应该要将`/tmp`下的数据都删除唷!
  - `/var`:第二层 FHS 设置，主要为放置变动性的数据，后续介绍。

  - `/usr`:第二层 FHS 设置，后续介绍。

- FHS 建议根目录中可以存在的目录：
  
  - `/home`：这是系统默认的使用者主文件夹（home directory）。在你新增一个一般使 用者帐号时，默认的使用者主文件夹都会规范到这里来。
  - `/lib<equal>`:用来存放与 `/lib` 不同的格式的二进制函数库，例如支持 64 位的 `/lib64` 函数库等。
  - `/root`:系统管理员（root）的主文件夹。之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时， 该目录就能够拥有 root 的主文件夹，所以我们会 希望 root 的主文件夹与根目录放置在同一个分区中。
  - `/lost+found`:这个目录是使用标准的 ext2/ext3/ext4 文件系统格式才会产生的一个目录， 目的在于当文件系统发生错误时， 将一些遗失的片段放置到这个目录下。 不过如果使用的是 xfs 文件系统的话，就不会存在这个目录了！
  - `/proc`:这个目录本身是一个“虚拟文件系统（virtual filesystem）”喔！他放置的数据都是在内存当中， 例如系统核心、行程信息（process）、周边设备的状态及网络状态等等。因为这个目录下的数据都是在内存当中， 所以本身不占任何硬盘空间啊！比较重要的文件例如：`/proc/cpuinfo`, `/proc/dma`, `/proc/interrupts`, `/proc/ioports`, `/proc/net/*` 等等。
  - `/sys`:这个目录其实跟/proc非常类似，也是一个虚拟的文件系统，主要也是记录 核心与系统硬件信息较相关的信息。 包括目前已载入的核心模块与核心侦测到的硬件设备信息等等。这个目录同样不占硬盘容量喔！
- 因为是所有系统默认的软件（distribution发布者提供的软件）都会放置到`/usr`下面，因此这个目录有点类似 Windows 系统的“C:\Windows\ （当中的一部份） + C:\Program files\”这两个目 录的综合体，系统刚安装完毕时，这个目录会占用最多的硬盘容量。一般来说，`/usr`的次目录 建议有下面这些：
- FHS 要求必须要存在的目录:
  - `/usr/bin/`:所有一般用户能够使用的指令都放在这里！`/usr/bin` 与 `/bin` 是一模一样了！另外，FHS 要求在此目录下不应该有子目录！
  - `/usr/lib/`:基本上，与 `/lib` 功能相同，所以 `/lib` 就是链接到此目录中的！
  - `/usr/local/`:系统管理员在本机自行安装自己下载的软件（非 distribution 默认提供者），建议安装到此目录， 这样会比较便于管理。举例来说，你的 distribution 提供的软件较旧，你想安装较新的软件但又不想移除旧版，此时你可以将新版软件安装于`/usr/local/`目录下，可与原先的旧版软件有分别啦！ 你可以自行到`/usr/local`去看看，该目录下也是具有bin, etc, include, lib...的次目录喔！
  - `/usr/sbin/`:非系统正常运行所需要的系统指令。最常见的就是某些网络服务器软件 的服务指令（daemon）啰！不过基本功能与 `/sbin` 也差不多， 因此目前 `/sbin` 就是链接到此目录中的。
  - `/usr/share/`:主要放置只读架构的数据文件，当然也包括共享文件。在这个目录下放置的数据几乎是不分硬件架构均可读取的数据，因为几乎都是文字文件嘛！在此目录下常见的还有这些次目录：
    - `/usr/share/man`：线上说明文档。 
    - `/usr/share/doc`：软件杂项的文件说明。 
    - `/usr/share/zoneinfo`：与时区有关的时区文件。
- FHS 建议可以存在的目录：
  - `/usr/games/`:与游戏比较相关的数据放置处。
  - `/usr/include/`:c/c++等程序语言的文件开始（header）与包含档（include）放置处， 当我们以tarball方式 （*.tar.gz 的方式安装软件）安装某些数据时，会使用到里头的许多包含档喔！
  - `/usr/libexec/`:某些不被一般使用者惯用的可执行文件或脚本（script）等等，都会放 置在此目录中。例如大部分的 X 窗口下面的操作指令， 很多都是放在此目录下的。
  - `/usr/lib<qual>/`:与 `/lib<qual>/`功能相同，因此目前 `/lib<qual>` 就是链接到此目录中。
  - `/usr/src/`:一般源代码建议放置到这里，src有source的意思。至于核心源代码则建议放置到`/usr/src/linux/`目录下。
- 如果`/usr`是安装时会占用较大硬盘容量的目录，那么`/var`就是在系统运行后才会渐渐占用硬盘容量的目录。 因为`/var`目录主要针对常态性变动的文件，包括高速缓存（cache）、登录文件 （log file）以及某些软件运行所产生的文件， 包括程序文件（lock file, run file），或者例如 MySQL数据库的文件等等。常见的次目录有：
- FHS 要求必须要存在的目录:
  - `/var/cache/`:应用程序本身运行过程中会产生的一些暂存盘；
  - `/var/lib/`:程序本身执行的过程中，需要使用到的数据文件放置的目录。在此目录下各自的软件应该要有各自的目录。举例来说，MySQL的数据库放置 到`/var/lib/mysql/`而rpm的数据库则放到`/var/lib/rpm`去！
  - `/var/lock/`:某些设备或者是文件资源一次只能被一个应用程序所使用，如果同时有两个程序使用该设备时，就可能产生一些错误的状况，因此就得要将该设备上锁（lock），以确保该设备只会给单一软件所使用。目前此目录也已经挪到 `/run/lock`中！
  - `/var/log/`:重要到不行！这是登录文件放置的目录！里面比较重要的文件 如`/var/log/messages`, `/var/log/wtmp`（记录登陆者的信息）等。
  - `/var/mail/`:放置个人电子邮件信箱的目录，不过这个目录也被放置到`/var/spool/mail/` 目录中！ 通常这两个目录是互为链接文件啦！
  - `/var/run/`:某些程序或者是服务启动后，会将他们的PID放置在这个目录下喔！至于 PID的意义我们会在后续章节提到的。与 `/run` 相同，这个目录链接到 `/run` 去了！
  - `/var/spool/`:这个目录通常放置一些伫列数据，所谓的“伫列”就是排队等待其他程序使用的数据啦！ 这些数据被使用后通常都会被删除。举例来说，系统收到新信会放置到`/var/spool/mail/`中， 但使用者收下该信件后该封信原则上就会 被删除。信件如果暂时寄不出去会被放到`/var/spool/mqueue/`中， 等到被送出后就被删除。如果是工作调度数据（crontab），就会被放置到`/var/spool/cron/`目录中！

## 1.3 文件与目录管理
  
   - 网络文件常常提到类似“./run.sh”之类的数据，这个指令的意义为何？答：由于指令的执行需要变量（bash章节才会提到）的支持，若你的可执行文件放置在本目录，并且本目录并非正规的可执行文件目录（/bin, /usr/bin等为正规），此时要执行指令就得要严格指定该可执行文件。“./”代表“本目录”的意思，所以“./run.sh”代表“执行本目录下，名为run.sh的文件”。
   - cd （change directory, 变换目录）

     -  `cd ~dmtsai`: 代表去到 dmtsai 这个使用者的主文件夹，亦即 `/home/dmtsai`
     -  `cd ~`:表示回到自己的主文件夹，亦即是 `/root` 这个目录
     -  `cd`:没有加上任何路径，也还是代表回到自己主文件夹的意思
     -  `cd ..`:表示去到目前的上层目录
     -  ` cd -`:表示回到刚刚的那个目录
     -  ` cd ../postfix`:由`/var/spool/mail` 去到`/var/spool/postfix`
  -  一登陆Linux系统后，每个帐号都会在自己帐号的主文件夹中(`/home`)。
  -  pwd （显示目前所在的目录）
     - `pwd [-P]`:-P ：显示出确实的路径，而非使用链接 （link） 路径。加上 `pwd -P` 的选项后，会不以链接文件的数据显示，而是显示正确的完整路径。
     - `pwd`:单纯显示出目前的工作目录。
  - mkdir （创建新目录）
    - `mkdir [-mp] 目录名称`: 
       - `-m` ：设置文件的权限喔！直接设置，不需要看默认权限 （umask） 的脸色～
       - `-p` ：帮助你直接将所需要的目录（包含上层目录）递回创建起来！
       - 示例:
         - `mkdir test1/test2/test3/test4`:错误。mkdir: cannot create directory ‘test1/test2/test3/test4’: No such file or directory 
         - ` mkdir -p test1/test2/test3/test4`:正确。
    - rmdir （删除“空”的目录）
      - `rmdir [-p] 目录名称`:
         - `-p` ：连同“上层”“空的”目录也一起删除。
      - 示例:
        - `rmdir test1`:rmdir: failed to remove ‘test1’: Directory not empty
        - `rmdir -p test1/test2/test3/test4`:利用 `-p` 这个选项，立刻就可以将 `test1/test2/test3/test4` 一次删除,包括test1
        - 如果要将非空目录下的东西都杀掉呢？！ 这个时候就必须使用`rm -r 目录名称`
  - 文件与目录的检视： ls
    - `ls [-aAdfFhilnrRSt] 文件名或目录名称`
      - `-a` ：全部的文件，连同隐藏文件（ 开头为 . 的文件） 一起列出来（常用）
      - `-d` ：仅列出目录本身，而不是列出目录内的文件数据（常用）
      - `-l` ：长数据串行出，包含文件的属性与权限等等数据（常用）
      - 示例：
        - `ls -al ~`: 将主文件夹下的所有文件列出来（含属性与隐藏文件）
  - cp （复制文件或目录）
    - `cp [-adfilprsu] 来源文件（source） 目标文件（destination）`
    - `cp [options] source1 source2 source3 .... directory`
      - `-a` ：相当于 -dr --preserve=all 的意思，至于 dr 请参考下列说明；（常用） 
      - `-i` ：若目标文件（destination）已经存在时，在覆盖时会先询问动作的进行（常用） 
      - `-r` ：递回持续复制，用于目录的复制行为；（常用）
      - `-p` ：连同文件的属性（权限、用户、时间）一起复制过去，而非使用默认属性（备份常用）；
      - `--preserve=all` ：除了 -p 的权限相关参数外，还加入 SELinux 的属性, links, xattr 等也复制了。
      - 如果来源文件有两个以上，则最后一个目的文件一定要是“目录”才行！
      - 如果目标目录或者目标文件不存在，则相当于重命名。
      - 示例
        - ` cp ~/.bashrc /tmp/bashrc`:将主文件夹下的 .bashrc 复制到 `/tmp` 下，并更名为 bashrc
        - `cp -i ~/.bashrc /tmp/bashrc`:cp: overwrite `/tmp/bashrc'? n不覆盖，y为覆盖，重复作两次动作，由于 `/tmp` 下面已经存在 bashrc 了，加上 `-i` 选项后，则在覆盖前会询问使用者是否确定！可以按下 n 或者 y 来二次确认
        - `cp /var/log/wtmp .`:复制到当前目录
        - `cp /etc/ /tmp`:复制 `/etc/` 这个目录下的所有内容到 `/tmp` 下面，cp: omitting directory `/etc`，如果是目录则不能直接复制，要加上 `-r` 的选项
  - rm （移除文件或目录）
    - ` rm [-fir] 文件或目录`：
      - `-f` ：就是 force 的意思，忽略不存在的文件，不会出现警告讯息；
      - `-r` ：递回删除啊！最常用在目录的删除了！这是非常危险的选项！！！ 
      - `-rf`: 如子目录里面还有子目录时，那就要使用 -r 这个选项
      - `-rf/`: 会将系统文件全部删除，非常危险  ~~删库跑路~~
      - 示例：
        - `rm bashrc*`:通过万用字符*的帮忙，将`/tmp`下面开头为bashrc的文件名通通删除 
  - mv （移动文件与目录，或更名）
    - `mv [-fiu] source destination`
    - `mv [options] source1 source2 source3 .... directory`
      - `-f` ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；  
      - `-i` ：若目标文件 （destination） 已经存在时，就会询问是否覆盖！
      - `-u` ：若目标文件已经存在，且 source 比较新，才会更新 （update）
      - 示例:
        - 复制一文件，创建一目录，将文件移动到目录中： 
          - `cd /tmp`
          - `cp ~/.bashrc bashrc`
          - `mkdir mvtest`
          - `mv bashrc mvtest`
        - 文件改名：
          - `mv mvtest mvtest2`
        - 再创建两个文件，再全部移动到 `/tmp/mvtest2` 当中:
          - `cp ~/.bashrc bashrc1`
          - `cp ~/.bashrc bashrc2`
          - `mv bashrc1 bashrc2 mvtest2`
# 二、pip
  - `pip install --upgrade pip`或`pip install -U pip`:升级pip自身
  - `pip list`：查看已经通过pip安装的包
  - `pip install <包名> --upgrade`或`pip install -U <包名>`:升级包
  - `pip install <包名> -i https://mirrors.aliyun.com/pypi/simple`:指定单次安装源
  - `pip install SomePackage==1.0.4`:指定版本
  - `pip-review --auto/pip-review --local --interactive`:自动更新所有包
# 三、conda
  - `conda --version`:查看conda版本，验证是否安装
  - `conda update conda`:更新至最新版本，也会更新其它相关包
  - `conda update --all`:更新所有包 
  - `conda update package_name`:更新指定的包
  - `conda create -n env_name package_name`:创建名为 env_name 的新环境，并在该环境下安装名为 package_name 的包，可以指定新环境的版本号，例如：`conda create -n python2 python=python2.7 numpy pandas`，创建了python2环境，python版本为2.7，同时还安装了numpy pandas包
  - `conda activate env_name`:切换至env_name环境
  - `conda deactivate`:退出环境
  - `conda info -e`:显示所有已经创建的环
  - `conda create -n new_env_name --clone old_env_name`:复制old_env_name为new_env_name
  - `conda remove -n env_name –-all`:删除环境
  - `conda list`:查看所有已经安装的包
  - `conda install -n env_name package_name`:在指定环境中安装包
  - `conda remove -n env_name package`:删除指定环境中的包
  - `rm -rf 虚拟环境所在路径`:删除空环境
# 四、Vim
  - `shift+g`:移至文件末尾
# 五、其他
  - `sudo nautilus /home`:打开图形化文件夹
  - `top`或`htop`：查看当前进程
  - `ifconfig`：查看IP地址
  - `kill 进程号`:终止进程
  - `kill -9 进程号`:强制终止进程
  - `tar -zxvf xxx.tar.gz`:解压缩
  - `chmod u+x file`:对文件 file 增加文件主可执行权限（u表示文件主）
  - `sudo apt-get install package`:安装包
  - `sudo apt-get install package --reinstall`:重新安装包
  - `sudo apt-get -f install`:修复安装
  - `sudo apt-get remove package`:删除包
  - `sudo apt-get remove package --purge`:删除包，包括删除配置文件等
  - `sudo apt-get update`:更新源
  - `sudo apt-get upgrade`:更新已安装的包
  - `sudo apt-get dist-upgrade`:`升级系统
  - `sudo apt-get autoclean`:清理旧版本软件缓存
  - `sudo dpkg -i package.deb`:所有deb文件的安装
  - `sudo dpkg -r package`：所有deb文件的卸载
  - `sudo dpkg -P package`:彻底的卸载，包括软件的配置文件
   