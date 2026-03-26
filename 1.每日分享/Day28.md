## Day28_Thursday_Week5_3.26

1. **五个重要的信号** -- gzh

   |   Signal    |   value    | Action |                    Comment                    |
   | :---------: | :--------: | :----: | :-------------------------------------------: |
   | **SIGINT**  |     2      |  Term  |   键盘中断(Ctrl+C触发) (默认行为:终止进程)    |
   | **SIGQUIT** |     3      |  Core  |   键盘退出(Ctrl+\触发) (默认行为:终止进程)    |
   | **SIGKILL** |     9      |  Term  |                   终止进程                    |
   | **SIGCONT** | 19, 18, 25 |  Cont  |                暂停后恢复运行                 |
   | **SIGSTOP** | 17, 19, 23 |  Stop  | 暂停进程(可通过Ctrl+Z触发)(SIGCONT或者fg恢复) |

2. **标题** -- cjs

   

3. **标题** -- xkz

   

4. **标题** -- zjl

   

5. **标题**-- glh

   

6. **标题**-- zzc

   

7. **标题** -- ywj

   

8. **sleep函数如何保证睡眠** -- lw

   > 首先看sleep睡眠的本质：
   >
   > 1. 主动放弃 CPU
   > 2. 交给内核定时器
   > 3. 时间到自动唤醒
   >
   > 只有信号能提前打断，否则一定睡够。

方法一：可以用循环结构保证睡眠：while (sleep(seconds) != 0);
方法二：使用sigaction函数，设置sa_flag,修改信号处理方式