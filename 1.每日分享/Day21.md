## Day21_Wednesday_Week2_3.18

1. **标题**-- gzh

   | 函数声明(返回值-函数名-参数)                                 | 头文件                                                       | 作用                                                     |
   | :----------------------------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------------- |
   | int chmod(const char *pathname, mode_t mode);                | #include <sys/stat.h>                                        | 改变文件的权限                                           |
   | char *getcwd(char *buf, size_t size);                        | #include <unistd.h>                                          | 获取当前工作目录                                         |
   | int chdir(const char *path);                                 | #include <unistd.h>                                          | 改变当前工作目录                                         |
   | int mkdir(const char *pathname, mode_t mode);                | #include <sys/stat.h> #include <sys/types.h>                 | 创建目录                                                 |
   | int rmdir(const char *pathname);                             | #include <unistd.h>                                          | 删除目录                                                 |
   | DIR ***opendir**(const char *name); int closedir(DIR *dirp); | #include <dirent.h> #include <sys/types.h>                   | 打开目录流 关闭目录流                                    |
   | struct dirent *readdir(DIR *dirp);                           | #include <dirent.h>                                          | 通过目录流读取下一个目录项                               |
   | long telldir(DIR *dirp); void seekdir(DIR *dirp, long loc);  | #include <dirent.h>                                          | 记录目录流指针的位置 返回记录的位置                      |
   | void rewinddir(DIR *dirp);                                   | #include <dirent.h>                                          | 重置目录流的位置到开始处                                 |
   | int stat(const char *path, struct stat *buf);                | #include <sys/stat.h>                                        | 用于获取指定文件的相关详细信息 主要包括各种元数据信息    |
   | **POSIX标准库函数** struct passwd *getpwuid(uid_t uid); struct passwd *getpwuid(uid_t uid); | #include<sys/types.h> #include <pwd.h> #include <grp.h>      | 已经有用户UID以及组ID，则 将uid和gid成员转换成对应字符串 |
   | struct tm *localtime(const time_t *timer);                   | #include <time.h>                                            | 处理最后修改时间戳                                       |
   | int open(const char *pathname, int flags); int open(const char *pathname, int flags, mode_t mode); | #include <fcntl.h>  #include <sys/types.h> #include <sys/stat.h> | 打开一个已存在的文件或创建一个新文件                     |
   | ssize_t read(int fd, void *buf, size_t count);               | #include <unistd.h>                                          | 用户进程从内核区域中直接将数据读取出来                   |
   | ssize_t write(int fd, const void *buf, size_t count);        | #include <unistd.h>                                          | 用户进程直接将内存数据写入到内核区域中                   |
   | int ftruncate(int fd, off_t length);                         | #include <unistd.h>                                          | 改变已打开文件的长度，这个函数允许你增大或减小文件的大小 |
   | off_t lseek(int fd, off_t offset, int whence);               | #include <unistd.h>                                          | 修改当前文件描述符（fd）对应的读写位置                   |

2. **标题**-- cjs

   open函数中mode参数：

   | 标志         | 描述/含义                                                    |
   | ------------ | ------------------------------------------------------------ |
   | **O_RDONLY** | 以只读的方式打开                                             |
   | **O_WRONLY** | 以只写的方式打开                                             |
   | **O_RDWR**   | 以可读可写的方式打开                                         |
   | **O_CREAT**  | 如果文件不存在，则创建文件。**如果不添加此标志，那么文件不存在时，将打开失败** |
   | **O_EXCL**   | **仅与O_CREAT连用**，单独使用无意义。**如果文件已存在，则open失败** |
   | **O_TRUNC**  | 如果文件已存在且成功以写入模式打开，则将其长度截断为 0，即删除文件内容。 |
   | **O_APPEND** | 以追加模式打开文件，不能和**O_RDONLY**或者**O_TRUNC**连用。  |

   ```c
   1.O_RDONLY ：read only，只读模式。
   2.O_WRONLY ：write only，只写模式。
   3.O_RDWR：read write，读写模式。
   这样设计使得这三个标志是互斥的，在打开文件选择模式时，必须进行三选一
   4.O_WRONLY | O_CREAT | O_EXCL：以只写模式打开文件，如果文件不存在就创建新文件，如果文件存在则报错。这个标志组合可以避免覆盖已有文件。
     O_EXCL仅能和O_CREAT一起使用，单独使用或者和其余选项连用，都没有意义。
   5.O_RDONLY | O_CREAT：以只读模式打开文件，但如果文件不存在就创建一个新文件。
   6.O_APPEND表示写入操作将始终从文件末尾开始，即使文件指针移动到文件的其他位置也不例外。注意它和O_RDONLY或者O_TRUNC连用是没有意义的
   关于open函数的最后一个参数mode_t mode:
   这个参数只在使用了 O_CREAT 标志时才需要带上，因为这意味着文件不存在时会创建新文件，创建新文件则需要指定新文件的权限.
   
   ```

   

3. **标题**-- xkz

   

4. **标题**-- zjl

   

5. **标题**-- glh

   

6. **标题**-- zzc

   
