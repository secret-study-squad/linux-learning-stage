## Day19_Monday_Week4_3.16

1. **标题**-- gzh

   

2. **标题**-- cjs

   ```bash
   vim 中操作多窗口
   :new test.c -->上下分屏   :sp test.c 也是上下分屏 
   :vnew test.c --->左右分屏 :vsp test.c 也是左右分屏
   多窗口的切换
   ctrl + w,W (连续按两下w)
   
   vim中操作多标签
   :tabnew test.c
   切换多标签
   gt / gT
   ```

   

3. **标题**-- xkz

   

4. **标题**-- zjl

   

5. **标题**-- glh

   

6. **标题**-- zzc

   生成一个可执行程序main
   
   ~~~makefile
   OUT := main
   SRCS := $(wildcard *.c)
   OBJS := $(patsubst %.c, %.o, $(SRCS))
   
   $(OUT) : $(OBJS)
   	$(CC) $^ -o $@
   %.o : %.c
   	$(CC) -c $^ -o $@
   .PHONY : clean rebuild
   clean : 
   	$(RM) $(OBJS) $(OUT)
   rebuild : clean main
   ~~~
   
   
