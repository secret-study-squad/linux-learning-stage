## Day_day_Week_.

1. **标题** -- gzh

   

2. **标题** -- cjs

   

3. **常见信号** -- xkz

   1.**SIGINT**   键盘中断  终止进程

   ctrl c ;  kill -2 [pid]

   2.**SIGQUIT**键盘退出，终止进程  产生转储文件/core/dumped   也可设置ulimit中的core-file-size为0，不生成

   ctrl \;kill -3 [pid]

   3.**SIGKILL**  强制终止进程

   kill -9 [pid]  不可捕获，不可忽略，不可阻塞

   4.**SIGSTOP**  暂停进程

   ctrl z;kill -19[pid]

   5.**SIGCONT** 继续暂停的进程

   kill -18 [pid]  

4. **标题** -- zjl

   

5. **标题**-- glh

   

6. **标题**-- zzc

   

7. **标题** -- ywj

   

8. **为什么同一个信号量可能有多个值** -- lw

   同一个信号名出现多个值，是因为它在不同 CPU 架构上的编号不同，这是 Linux/Unix 的历史与跨平台兼容设计导致的。
   在 kill -l 或 man 7 signal 里，常见格式是：
   SIGUSR1: 10, 30, 16
   这三个数字分别对应：
   值（10）：x86、x86_64、ARM、PPC 等主流架构（你现在用的电脑 / 服务器基本都是这个）
   值（30）：Alpha、SPARC 等老架
   值（16）：MIPS 架构
   
   不同架构信号值不一样，是因为：
   硬件中断号不同 + 早期各 UNIX 厂商自定义 + Linux 为了兼容老系统保留了多套编号
   所以敲代码不要直接写数字值
