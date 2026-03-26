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

   mmap 系统调用的底层原理

   参考流程

   > [!NOTE]
   >
   > 1.准备阶段：open() 打开文件，获取 fd；用 fstat() 获取文件长度，**确定映射规模**。
   >
   > 2.建立映射：
   > 调用 mmap()。此时内核在进程的虚拟地址空间中（位于堆和栈之间的映射区）寻找一段足够长的空闲地址，并初始化一个 `vm_area_struct（VMA）` 结构体插入到进程的虚拟内存链表中。
   > **此时，并没有任何数据拷贝进物理内存**，但通过把文件对象（struct file）的地址赋值给 vma->vm_file，内核会调用 get_file(vma->vm_file)，将该文件对象的**引用计数**加 1。此时，vma->vm_file 指向的struct file文件对象里有一个 f_mapping 指针指向 struct address_space（inode的成员变量）。**(这里绕过了fd，所以close(fd)后，只要没有munmap依然能访问mmap的文件)**
   >
   > ```c
   > struct inode {
   > ...
   > struct address_space i_mapping;
   > ...
   > };
   > ```
   >
   > struct address_space 是内核管理该文件所有缓存页的核心。内核通过 inode->i_mapping 获取 address_space，将**虚拟地址**和**页缓存page cache**（address_space 管理）建立映射关系。注意，**此时只建立了“映射表”，没有分配物理页**。
   >
   > 3.读写阶段：当进程第一次访问映射区的某个地址时，由于没有对应的物理内存，CPU 触发**缺页中断 (Page Fault)**。内核捕捉到中断，检查发现该地址合法且有映射文件。**内核将磁盘数据读入物理内存（Page Cache），并在 MMU 中建立虚拟地址到物理地址的映射**。中断结束，进程恢复执行，如同数据早已在内存中。
   >
   > 4.同步阶段（可选）：修改重要数据后，调用 msync(..., MS_SYNC) 确保数据已经穿过内核缓冲区，安全到达磁盘驱动器。
   >
   > 5.销毁阶段：调用 munmap() 释放虚拟空间，最后调用 close(fd) 关闭文件描述符。

    `vm_area_struct` 的结构简析

   在 Linux 内核源码（通常在 `include/linux/mm_types.h`）中，`vm_area_struct` 定义了进程的一块连续虚拟内存区域。以下是其核心成员：

   ```c
   struct vm_area_struct {
       /* 1. 映射的范围 */
       unsigned long vm_start;     /* 区域的起始虚拟地址 */
       unsigned long vm_end;       /* 区域的结束虚拟地址 */
   
       /* 2. 链表和红黑树指针，用于快速查找 VMA */
       struct vm_area_struct *vm_next, *vm_prev;
       struct rb_node vm_rb;
   
       /* 3. 权限和标志 */
       pgprot_t vm_page_prot;      /* 访问权限：只读、读写等 */
       unsigned long vm_flags;     /* 标志：MAP_SHARED, MAP_PRIVATE, VM_MAYSHARE 等 */
   
       /* 4. 文件映射的核心 */
       struct file * vm_file;      /* 指向被映射的文件对象 (关键!!!!！) */
       unsigned long vm_pgoff;     /* 文件内的偏移量（单位是页） */
       
       /* 5. 虚存操作函数指针 */
       const struct vm_operations_struct *vm_ops; /* 包含 fault() 函数，处理缺页异常 */
   
       void * vm_private_data;     /* 驱动程序私有数据 */
   };
   ```

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