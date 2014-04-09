1. generate linker script.

导出默认的连接控制脚本文件：保存为linker.lds

通过命令gcc -Wl,--verbose可以获得默认的连接控制脚本, 即选择 "=======..."之间的文本，保存为linker.lds文件

2. Add function control to the linker.lds file

在linker.lds文件中增加本例需要控制的语句：
       将下面
       /*定义__initcall_start符号为当前位置,即.代表当前位置*/
     __initcall_start = .;
     function_ptrs   : { *(function_ptrs) }
     __initcall_end = .;
     /*上述3行代码代表function_ptrs节位于__initcall_start和__initcall_end之间*/
     code_segment    : { *(code_segment) }
    这段代码copy到linker.lds文件的
           __bss_start = .;
语句之前

3. compiler

命令：
gcc -Tlinker.lds -o doinitcall doinitcall.c
       其中：
-T选项告诉ld要用的连接控制脚本文件,做为链接程序的依据。格式如下：
              -T commandfile 或
              --script=commandfile

4. results

执行程序可以看到如下结果：
$ ./doinitcall      
       in main()
       call_p: 0x804961c
       my_init () #1
       call_p: 0x8049620
       my_init () #2