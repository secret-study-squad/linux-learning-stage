## Day29_Friday_Week5_3.27

1. **线程库** -- gzh

   ```c
   man 7 pthreads
   (Linux implementations of POSIX threads
   LinuxThreads: 并不完全兼容POSIX标准
   NPTL:   Native POSIX Thread Library)
   ```

2. **标题** -- cjs

   ```c
   #include <2026Linux.h>
   void func(int num){
       printf("num = %d\n",num);
       sigset_t block_set;
       sigemptyset(&block_set);
       sleep(10);
       sigpending(&block_set);
       printf("sleep begin\n");
       if(sigismember(&block_set,2)){
           printf("2号还有在里面的\n");
       }
       if(sigismember(&block_set,3)){
           printf("3号在里面\n");
       }
       printf("sleep over\n");
   }
   // 此时值得注意的一个点:
   // 当我按下2号信号时，此时进入func中，然后我再次按下3号和2号信号(3号先于2号信号)
   // 此时当第一个2号执行完时，先执行的是2号信号，而不是3号信号
   //
   // 原因：在act中我们设置了阻塞3号信号，此时3号信号此时已经在mask集合之中，这是一个前提
   // 同时当第一个2号信号来的时候，此时2号信号也在mask集合之中了，
   // 所以当再来2号和3号信号的时候，此时都会放入pending集合之中，
   // 所以在pending集合之中，会按照信号顺序来取了报送
   
   int main(int argc,char *argv[])
   {
       struct sigaction act,oldact;
       memset(&act,0,sizeof(act));
   
       act.sa_handler = func;
       
       sigset_t set;
       sigemptyset(&set);
       sigaddset(&set,3); // 将3号信号加入阻塞，当2号信号在执行的时候
   
       act.sa_mask = set;
   
       sigaction(2,&act,&oldact);
   
       printf("begin while\n");
       while(1);
   
       return 0;
   }
   
   ```

   

3. **标题** -- xkz

   每个线程开辟了一个独属于自己的栈, 而堆/数据段/代码段/..是共享的。 同时也共享一个进程的stdout(标准输出)。

4. **标题** -- zjl

   

5. **标题**-- glh

   

6. **标题**-- zzc

   

7. **标题** -- ywj

   

![pthread_cleanup_push and pop](..\pics\pthread_cleanup_push and pop.png)

![push and pop 示例](..\pics\push and pop 示例.png)

1. **标题** -- lw

| 对比项       | 普通函数           | 子线程                     | 子进程                        |
| ------------ | ------------------ | -------------------------- | ----------------------------- |
| 通俗理解     | 正常调用的一段代码 | **并行执行的特殊函数**     | **克隆出来的新程序**          |
| 是否独立进程 | 同进程内           | 同进程内                   | **全新独立进程**              |
| 内存 / 变量  | 完全共享           | **完全共享**               | **完全独立，不共享**          |
| 执行方式     | 阻塞，必须等它跑完 | 并发，后台自己跑           | 并发，独立运行                |
| 崩溃影响     | 一般不崩整个程序   | 线程崩，**整个进程一起崩** | 子进程崩，**父进程没事**      |
| 创建开销     | 极小               | 小                         | 较大                          |
| 通信方式     | 直接传参           | 直接读写变量               | 需要 IPC（管道 / 消息队列等） |
| 对应系统调用 | 函数调用           | `pthread`                  | `fork`                        |

**极简记忆**

- **函数**：顺序跑，等它结束
- **线程**：同家兄弟，一起干活，一人出事全完蛋
- **进程**：分家单过，互不影响，沟通要传话

