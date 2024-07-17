---
title: Linux基础复习记录
date: 2024-07-14 16:45:08
tags: 一生一芯 Linux shell
---

## 写在前面

- 作为一个多年的Linux用户，至今没有能覆盖完整所有的主题，还是很惭愧的；更令人遗憾的是，没能为开源社区做什么贡献，一直在企业应用系统中打转。
- 借此机会，系统复习一次，查漏补缺，也记录一些有价值的信息，便于后续查阅。
- **下文内容中都是个人没有掌握太好的一些知识、工具或者方法**。

## [Linux101](https://101.ustclug.org/)

- 摩尔定律的两种说法

  - “**集成电路上可容纳的晶体管数目每两年就会翻一倍**”

  - **计算机的性能每 18 个月提高一倍** ------大卫·豪斯

  - 不过，由于晶体管的密度会受到量子物理理论上的限制，以及 CPU 会受到功耗和散热的限制，事实上这个定律已经开始失效了，计算机性能的提升开始放缓。

    

- Linux进程启动顺序

  ```
  按照 PID 排序时，我们可以观察系统启动的过程。
  Linux 系统内核从引导程序接手控制权后，开始内核初始化，随后变为 init_task，初始化自己的 PID 为 0。
  随后创建出 1 号进程（init 程序，目前一般为 systemd）衍生出用户空间的所有进程，
  创建 2 号进程 kthreadd 衍生出所有内核线程。
  随后 0 号进程成为 idle 进程，1 号、2 号并非特意预留，而是产生进程的自然顺序使然。
  
  由于 kthreadd 运行于内核空间，htop命令下，故需按大写 K（Shift + k）键显示内核进程后才能看到。然而无论如何也不可能在 htop 中看到 0 号进程本体，只能发现 1 号和 2 号进程的 PPID 是 0。
  ```

  

- ```
  flock -n [锁文件的路径] [你需要执行的命令]
  ```

  如果其他命令正在执行，那么这个文件锁会被其占用；crontab 尝试再执行命令时，flock 会发现对应的文件已经锁上，因此会立刻退出（`-n` 参数）。

- 正则表达式，是我一直以来都没有很好掌握的章节[Shell 高级文本处理与正则表达式](https://101.ustclug.org/Ch09/) ， 我的基本使用原则是，不理解的表达式不用。



## [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line) 命令行的艺术

- 快速通过；记录一些冷门但有用的命令

  ```
  expr：计算表达式或正则匹配
  
  m4：简单的宏处理器
  
  yes：多次打印字符串
  
  cal：漂亮的日历
  
  env：执行一个命令（脚本文件中很有用）
  
  printenv：打印环境变量（调试时或在写脚本文件时很有用）
  
  look：查找以特定字符串开头的单词或行
  
  cut，paste 和 join：数据修改
  
  fmt：格式化文本段落
  
  pr：将文本格式化成页／列形式
  
  fold：包裹文本中的几行
  
  column：将文本格式化成多个对齐、定宽的列或表格
  
  expand 和 unexpand：制表符与空格之间转换
  
  nl：添加行号
  
  seq：打印数字
  
  bc：计算器
  
  factor：分解因数
  
  gpg：加密并签名文件
  
  toe：terminfo 入口列表
  
  nc：网络调试及数据传输
  
  socat：套接字代理，与 netcat 类似
  
  slurm：网络流量可视化
  
  dd：文件或设备间传输数据
  
  file：确定文件类型
  
  tree：以树的形式显示路径和文件，类似于递归的 ls
  
  stat：文件信息
  
  time：执行命令，并计算执行时间
  
  timeout：在指定时长范围内执行命令，并在规定时间结束后停止进程
  
  lockfile：使文件只能通过 rm -f 移除
  
  logrotate： 切换、压缩以及发送日志文件
  
  watch：重复运行同一个命令，展示结果并／或高亮有更改的部分
  
  when-changed：当检测到文件更改时执行指定命令。参阅 inotifywait 和 entr。
  
  tac：反向输出文件
  
  shuf：文件中随机选取几行
  
  comm：一行一行的比较排序过的文件
  
  strings：从二进制文件中抽取文本
  
  tr：转换字母
  
  iconv 或 uconv：文本编码转换
  
  split 和 csplit：分割文件
  
  sponge：在写入前读取所有输入，在读取文件后再向同一文件写入时比较有用，例如 grep -v something some-file | sponge some-file
  
  units：将一种计量单位转换为另一种等效的计量单位（参阅 /usr/share/units/definitions.units）
  
  apg：随机生成密码
  
  xz：高比例的文件压缩
  
  ldd：动态库信息
  
  nm：提取 obj 文件中的符号
  
  ab 或 wrk：web 服务器性能分析
  
  strace：调试系统调用
  
  mtr：更好的网络调试跟踪工具
  
  cssh：可视化的并发 shell
  
  rsync：通过 ssh 或本地文件系统同步文件和文件夹
  
  wireshark 和 tshark：抓包和网络调试工具
  
  ngrep：网络层的 grep
  
  host 和 dig：DNS 查找
  
  lsof：列出当前系统打开文件的工具以及查看端口信息
  
  dstat：系统状态查看
  
  glances：高层次的多子系统总览
  
  iostat：硬盘使用状态
  
  mpstat： CPU 使用状态
  
  vmstat： 内存使用状态
  
  htop：top 的加强版
  
  last：登入记录
  
  w：查看处于登录状态的用户
  
  id：用户/组 ID 信息
  
  sar：系统历史数据
  
  iftop 或 nethogs：套接字及进程的网络利用情况
  
  ss：套接字数据
  
  dmesg：引导及系统错误信息
  
  sysctl： 在内核运行时动态地查看和修改内核的运行参数
  
  hdparm：SATA/ATA 磁盘更改及性能分析
  
  lsblk：列出块设备信息：以树形展示你的磁盘以及磁盘分区信息
  
  lshw，lscpu，lspci，lsusb 和 dmidecode：查看硬件信息，包括 CPU、BIOS、RAID、显卡、USB设备等
  
  lsmod 和 modinfo：列出内核模块，并显示其细节
  
  fortune，ddate 和 sl：额，这主要取决于你是否认为蒸汽火车和莫名其妙的名人名言是否“有用”
  ```

  

##  [The Missing Semester of Your CS Education](https://missing-semester-cn.github.io/) 计算机教育中缺失的一课

- 计算机设计的初衷就是任务自动化，然而学生们却常常陷在大量的重复任务中，或者无法完全发挥出诸如 版本控制、文本编辑器等工具的强大作用。

- 我认为比较有意义的工具

  ```
   sshfs 可以将远端服务器上的一个文件夹挂载到本地，然后您就可以使用本地的编辑器了
   fd 性能比find快很多
   
  ```

  

## 必做题



## 高价值信息

- [MIT 学术诚信](https://integrity.mit.edu/)
- [*The TTY demystified* 中文翻译](https://www.cnblogs.com/liqiuhao/p/9031803.html)
- [The TTY demystified](http://www.linusakesson.net/programming/tty/)
- [正则表达式101站点， 包括一个正则表达式解析工具](https://regex101.com/)
- 