# linker

编译静态库：
  g++ my_math.cpp -c //  -c表示生成.o文件，不调用linker
  ar -r libmy_math_static.a my_math.o  //  ar只是将my_math.o打包，可以同时打包多个

链接静态库：
  g++ main.cpp -lmy_math_static -L./ -o a.out_static  //  将libmy_math_static.a静态链接
  或者g++ main.cpp -static -lmy_math_static -L./ -o a.out_static  //  -static会把其他库也静态链接进来，执行ldd a.out_static可以观察到

编译动态库：
  g++ my_math.cpp  -shared -fPIC  -o libmy_math_shared.so  //  -fPIC生成位置无关代码
  
链接动态库：
  g++ main.cpp -lmy_math_shared -L./ -o a.out_shared  //  直接执行会报错./a.out_shared: error while loading shared libraries: libmy_math_shared.so: cannot open shared object file: No such file or directory，看下一步

动态库搜索路径（按照优先级）：
  1、rpath：g++ main.cpp -lmy_math_shared -L. -Wl,-rpath=/search/lab/cpp/lib  -o a.out_shared  //  -Wl表示传参数给linker，-rpath就是需要传递的路径
  2、export LD_LIBRARY_PATH=/search/lab/cpp/lib
  3、在/etc/ld.so.conf中新增路径/search/lab/cpp/lib
  4、/lib /lib64  
  5、/usr/lib /usr/lib64
  
  
main.cpp
*****************
#include <iostream>
#include "my_math.h"

int main()
{
        int r = sum(6, 7);
        std::cout<< "r = " << r << '\n';
}
*****************
  
  
my_math.h
*****************
#ifndef _MY_MATH_H_
#define _MY_MATH_H_
int sum(int a, int b);
#endif
*****************
  
my_math.cpp
*****************
#include "my_math.h"

int sum(int a, int b)
{
        return a + b;
}
*****************
