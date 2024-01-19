### 继承

- 在继承的头文件中包含了，主程序又重复包含了基类头文件，导致编译器报错：redefinition

![image-20231026113640820](C:/Users/21064/AppData/Roaming/Typora/typora-user-images/image-20231026113640820.png)

用make重复编译程序前，需要（make clean）删除之前编译产生的.o文件，否则make会直接使用上一次的.o文件，不会通过.c/.cpp生成.o文件。

### 多态

~~~bash
内联函数不能是虚函数
构造函数不能是虚函数
析构函数一般都声明为虚函数
重载：函数的参数不同，不能为虚函数
覆盖：函数、返回值相同，可设为虚函数
	**返回值例外：函数参数相同，但是返回值是当前对象的指针或引用，也可设为虚函数
~~~

### 动态库编译

~~~bash
/* 生成动态库使用.cpp直接生成 */
all: main.o libHuman.so  
	g++ -o test $< -L. -lHuman

main.o : main.cpp
	g++ -fPIC -c -o $@ $<
	
libHuman.so : English.cpp Chinese.cpp Human.cpp
	g++ -shared -o $@ $^ -fPIC

clean:
	rm -f *.o *.so test
	
/* 编译多个.o文件生成动态库报错 */
all: main.o libHuman.so  
	g++ -o test $< -L. -lHuman

main.o : main.cpp
	g++ -fPIC -c -o $@ $<
	
libHuman.so : English.o Chinese.o Human.o	//编译不通过？？？
	g++ -shared -o $@ $^ -fPIC

clean:
	rm -f *.o *.so test
~~~

~~~bash

/* .cpp生成动态库，编译时会生成‘.S文件’，然后连接成动态库‘.so文件’ */
GNU C++14 (Ubuntu 7.5.0-3ubuntu1~18.04) version 7.5.0 (x86_64-linux-gnu)
	compiled by GNU C version 7.5.0, GMP version 6.1.2, MPFR version 4.0.1, MPC version 1.1.0, isl version isl-0.19-GMP

GGC heuristics: --param ggc-min-expand=100 --param ggc-min-heapsize=131072
Compiler executable checksum: 3eb3dc290cd5714c3e1c3ae751116f07
COLLECT_GCC_OPTIONS='-shared' '-o' 'libHuman.so' '-fPIC' '-v' '-shared-libgcc' '-mtune=generic' '-march=x86-64'
 as -v --64 -o /tmp/ccLBS2T1.o /tmp/ccHNHxCN.s
GNU assembler version 2.30 (x86_64-linux-gnu) using BFD version (GNU Binutils for Ubuntu) 2.30
COLLECT_GCC_OPTIONS='-shared' '-o' 'libHuman.so' '-fPIC' '-v' '-shared-libgcc' '-mtune=generic' '-march=x86-64'
 /usr/lib/gcc/x86_64-linux-gnu/7/cc1plus -quiet -v -imultiarch x86_64-linux-gnu -D_GNU_SOURCE Human.cpp -quiet -dumpbase Human.cpp -mtune=generic -march=x86-64 -auxbase Human -version -fPIC -fstack-protector-strong -Wformat -Wformat-security -o /tmp/ccHNHxCN.s
GNU C++14 (Ubuntu 7.5.0-3ubuntu1~18.04) version 7.5.0 (x86_64-linux-gnu)
	compiled by GNU C version 7.5.0, GMP version 6.1.2, MPFR version 4.0.1, MPC version 1.1.0, isl version isl-0.19-GMP

GGC heuristics: --param ggc-min-expand=100 --param ggc-min-heapsize=131072
ignoring duplicate directory "/usr/include/x86_64-linux-gnu/c++/7"
ignoring nonexistent directory "/usr/local/include/x86_64-linux-gnu"
ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/7/../../../../x86_64-linux-gnu/include"
#include "..." search starts here:
#include <...> search starts here:
 /usr/include/c++/7
 /usr/include/x86_64-linux-gnu/c++/7
 /usr/include/c++/7/backward
 /usr/lib/gcc/x86_64-linux-gnu/7/include
 /usr/local/include
 /usr/lib/gcc/x86_64-linux-gnu/7/include-fixed
 /usr/include/x86_64-linux-gnu
 /usr/include
End of search list.
~~~

