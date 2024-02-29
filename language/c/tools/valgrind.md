# valgrind

## 基本选项
1. Memcheck。这是valgrind应用最广泛的工具，一个重量级的内存检查器，能够发现开发中绝大多数内存错误使用情况，
   比如：使用未初始化的内存，使用已经释放了的内存，内存访问越界等。这也是本文将重点介绍的部分。
2. Callgrind。它主要用来检查程序中函数调用过程中出现的问题。
3. Cachegrind。它主要用来检查程序中缓存使用出现的问题。
4. Helgrind。它主要用来检查多线程程序中出现的竞争问题。
5. Massif。它主要用来检查程序中堆栈使用中出现的问题。
6. Extension。可以利用core提供的功能，自己编写特定的内存调试工具

## 常用选项
1. 适用于所有Valgrind工具
    –tool=< name > 最常用的选项。运行 valgrind中名为toolname的工具。默认memcheck。
    -h --help 显示帮助信息。
    –version 显示valgrind内核的版本，每个工具都有各自的版本。
    -q --quiet 安静地运行，只打印错误信息。
    -v --verbose 更详细的信息, 增加错误数统计。
    –trace-children=no|yes 跟踪子线程? [no]
    –track-fds=no|yes 跟踪打开的文件描述？[no]
    –time-stamp=no|yes 增加时间戳到LOG信息? [no]
    –log-fd=< number > 输出LOG到描述符文件 [2=stderr]
    –log-file=< file > 将输出的信息写入到filename.PID的文件里，PID是运行程序的进行ID
    –log-file-exactly=< file > 输出LOG信息到 file
    –log-file-qualifier=< VAR > 取得环境变量的值来做为输出信息的文件名。 [none]
    –log-socket=ipaddr:port 输出LOG到socket ，ipaddr:port

2. LOG信息输出
    –xml=yes 将信息以xml格式输出，只有memcheck可用
    –num-callers=< number > show < numbe r> callers in stack traces [12]
    –error-limit=no|yes 如果太多错误，则停止显示新错误? [yes]
    –error-exitcode=< number > 如果发现错误则返回错误代码 [0=disable]
    –db-attach=no|yes 当出现错误，valgrind会自动启动调试器gdb。[no]
    –db-command=< command > 启动调试器的命令行选项[gdb -nw %f %p]

3. 适用于Memcheck工具的相关选项：
    –leak-check=no|summary|full 要求对leak给出详细信息? [summary]
    –leak-resolution=low|med|high how much bt merging in leak check [low]
    –show-reachable=no|yes show reachable blocks in leak check? [no]
    更详细的使用信息详见帮助文件、man手册或官网：http://valgrind.org/docs/manual/manual-core.html

4. 注意：
    valgrind不会自动的检查程序的每一行代码，只会检查运行到的代码分支，所以单元测试或功能测试用例很重要；
    可以把valgrind看成是一个sandbox，通过valgrind运行的程序实际上是运行在valgrind的sandbox中的，
    所以，不要测试性能，会让你失望的，建议只做功能测试
    编译代码时，建议增加-g -o0选项，不要使用-o1、-o2选项

示例:
```shell
valgrind --tool=memcheck --log-file=log.txt --leak-check=yes  ./test
```

### memcheck
Memcheck是valgrind应用最广泛的工具，能够发现开发中绝大多数内存错误使用情况。
此工具主要可检查以下错误
1. 使用未初始化的内存(Use of uninitialised memory)
2. 使用已经释放了的内存(Reading/writing memory after it has been free’d)
3. 使用超过malloc分配的内存空间(Reading/writing off the end of malloc’d blocks)
4. 对堆栈的非法访问(Reading/writing inappropriate areas on the stack)
5. 申请的空间是否有释放(Memory leaks – where pointers to malloc’d blocks are lost forever)
6. malloc/free/new/delete申请和释放内存的匹配(Mismatched use of malloc/new/new [] vs free/delete/delete [])
7. src和dst的重叠(Overlapping src and dst pointers in memcpy() and related functions)
```c
#include<iostream>
int main()
{
	int *pInt;
	std::cout<<"使用未初始化的内存";
	int a=*pInt;    //使用未初始化的内存
}


#include<iostream>
int main()
{
	int *pArray=(int *)malloc(sizeof(int) *5);
	std::cout<<"使用已经释放了的内存";
	free(pArray);
	pArray[0]=0;    //使用已经释放了的内存
}


#include<iostream>
int main()
{
	int *pArray=(int *)malloc(sizeof(int) *5);
	std::cout<<"使用超过malloc分配的内存空间";
	pArray[5]=5;    //使用超过malloc分配的内存空间
	free(pArray);
}


#include<iostream>
int main()
{
	int *pArray=(int *)malloc(sizeof(int) *5);
	std::cout<<"malloc缺少free";
}


#include<iostream>
int main()
{
	char a[10];
	for (char c=0; c < sizeof(a); c++)
	{
		a[c]=c;
	}
	std::cout<<"拷贝的src和dst存在重叠";
	memcpy(&a[4],&a[0],6);
}


g++ test1.cpp -g -o test1_g   //-g：让 memcheck 工具可以取到出错的具体行号
valgrind --leak-check=yes --log-file=1_g ./test1_g

https://blog.csdn.net/weixin_45518728/article/details/119865117
```
