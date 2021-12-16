# Scons-Tutorials
# 什么是[Scons](https://www.scons.org/)？

​	SCons 是一个开源软件构建工具。将 SCons 视为经典 Make 实用程序的改进的跨平台替代品，具有类似于 autoconf/automake 和编译器缓存（如 ccache）的集成功能。简而言之，SCons 是一种更简单、更可靠、更快速的软件构建方式。



# 什么使Scons更好？

- 配置文件是 Python 脚本——使用真正的编程语言的力量来解决构建问题。
-  为 C、C++ 和 Fortran 内置了可靠、自动的依赖项分析——不再需要“make Depend”或“make clean”来获取所有依赖项。通过用户定义的其他语言或文件类型的依赖项扫描器，可以轻松扩展依赖项分析。
- 内置支持 C、C++、D、Java、Fortran、Yacc、Lex、Qt 和 SWIG，以及构建 TeX 和 LaTeX 文档。可通过用户定义的其他语言或文件类型的构建器轻松扩展。
- 从源代码和/或预构建目标的中央存储库构建。
- 对 Microsoft Visual Studio 的内置支持，包括生成 .dsp、.dsw、.sln 和 .vcproj 文件。
- 使用 MD5 签名可靠检测构建更改；对传统时间戳的可选、可配置支持。
- 支持并行构建 - 类似于 make -j 但无论目录层次结构如何，都可以同时运行 N 个作业。
- 用于查找 #include 文件、库、函数和 typedef 的集成 Autoconf 式支持。
- 所有依赖项的全局视图——不再需要多次构建传递或重新排序目标来构建所有内容。
- 能够在缓存中共享构建的文件以加速多个构建——类似于 ccache，但适用于任何类型的目标文件，而不仅仅是 C/C++ 编译。
- 专为跨平台构建而设计，可用于 Linux、其他 POSIX 系统（包括 AIX、BSD 系统、HP/UX、IRIX 和 Solaris）、Windows 7/8/10、MacOS 和 OS/2 .



# Scons从哪里来？

​	SCons 最初是作为 ScCons 构建工具设计的，它在 2000 年 8 月赢得了 Software Carpentry SC Build 竞赛。该设计反过来又基于 Cons 软件构建实用程序。该项目已重命名为 SCons 以反映它不再与 Software Carpentry 直接相关。



# github源码

https://github.com/SCons/scons

运行 SCons 需要 Python 3.6 或更高版本。运行标准 SCons 不应该有其他依赖项或要求。支持 Python 3.5 的最后一个版本是 4.2.0。



# 安装

windows安装示例：

```
# to do a system-level install:
$ python -m pip install --user scons

# Windows variant, assuming Python Laucher:
C:\Users\me> py -m pip install --user scons

# inside a virtualenv it's safe to use bare pip:
(myvenv) $ pip install scons

# install in a virtualenv from a wheel file:
(myvenv) $ pip install SCons-4.3.0-py3-none-any.whl

# install in a virtualenv from source directory:
(myvenv) $ pip install --editable .
```

	ubuntu 安装Scons:

```
sudo apt-get install scons
```



# Scons使用

​	Scons是一个开放源码、以Python语言编码的自动化构建工具，可用来替代make编写复杂的makefile。并且scons是跨平台的，只要scons脚本写的好，可以在Linux和Windows下随意编译。


​	

## 单个文件

​	一个最简单的项目， hello.c, 源码在  **[这里](https://github.com/lvguidong/Scons-Tutorials/tree/main/demo1)**：

```c
#include <stdio.h>

int main()
{
    printf("lvguidong scons first demo\n");
    return 0;
}
```

编写SConstruct文件：

```shell
Program("hello.c")
```

对的， 就一行，就可构建程序了，再使用scons命令执行：

```shell
 D:\Scons-Tutorials\demo1> scons
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Building targets ...
cl /Fohello.obj /c hello.c /nologo
hello.c
link /nologo /OUT:hello.exe hello.obj
scons: done building targets.
```

这个比传统的makefile简单很多，SConstruct用python脚本语法编写，可以像编写python脚本一样来写，如要清理生成的文件，使用 `scons -c`

```
D:\Scons-Tutorials\demo1> scons -c
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Cleaning targets ...  
Removed hello.obj
Removed hello.exe
scons: done cleaning targets.
```





## 相关命令：

​	scons 不仅可以编写可执行的二进制文件，也可以编译其他类型，执行的类型有：

- `Program`:  编译成可执行程序, 这是常用的类型
- `object`: 只编译成目标文件，这种使用的较少，windows上产生 `.obj` 文件，linux系统中产生`.o`文件
- `Library`: 编译成库文件，默认是指静态链接库
- `StaticLibrary`:  显示的编译成静态链接库，与上面的 Library 效果一样。
- `SharedLibrary`:  windows产生 `.dll` 文件,linux产生 `.so`文件。



## 编译静态文件

​	编译静态文件使用类型`Libray`,源码在 **[这里](https://github.com/lvguidong/Scons-Tutorials/tree/main/demo2)**

​	同样Scontruct一行：

```shell
Library("add.c")
```



## 编译动态文件

​	编译静态文件使用类型 `SharedLibray`,源码在 **[这里](https://github.com/lvguidong/Scons-Tutorials/tree/main/demo3)**

```
SharedLibrary("add.c")
```



## 多个文件

​	如果你的项目由多个源文件组成，而且你想指定一些编译的宏定义，以及显式的指定使用某些库，这些对于 SCons 来说，都是非常简单的事情，

​	使用Glob() 包含所需要的文件，并且生成自定义名称文件，源码在 [**这里**](https://github.com/lvguidong/Scons-Tutorials/tree/main/demo4)

```
SOURDES = Glob('*.c')
SharedLibrary('myscons', SOURDES)
```



## 配置参数

​	makefile可以配置编译的参数，scons也可以，如下示例：

```
Program('test', ['test1.c', 'file1.c', 'file2.c'], 
       LIBS = 'm', 
       LIBPATH = ['/usr/lib', '/usr/local/lib'], 
       CCFLAGS = '-DHELLOSCONS')

```

​	配置文件中 LIBS,LIBAPTH 和 CCFLAGS 是 SCons 内置的关键字，它们的作用如下：

- `LIBS`： 显示的指明要在链接过程中使用的库，如果有多个库，应该把它们放在一个列表里面。这个例子里，我们使用一个称为 m 的库。
- `LIBPATH`： 链接库的搜索路径，多个搜索路径放在一个列表中。这个例子里，库的搜索路径是 /usr/lib 和 /usr/local/lib。
- `CCFLAGS`： 编译选项，可以指定需要的任意编译选项，如果有多个选项，应该放在一个列表中。这个例子里，编译选项是通过 -D 这个 gcc 的选项定义了一个宏 HELLOSCONS。
- `CPPPATH`：指定头文件的路径



## 一个典型的示例

​	参考了zxing的 **[Scontruct](https://github.com/zxing/zxing/blob/00f634024ceeee591f54e6984ea7dd666fab22ae/cpp/SConstruct)** 和 **[SConscript ](https://github.com/zxing/zxing/blob/00f634024ceeee591f54e6984ea7dd666fab22ae/cpp/SConscript)**文件

​	Scontruct:

```
SConscript('SConscript', variant_dir='build')

Alias('all', ['lib', 'tests', 'zxing'])
Default('all')

# Remove build folder on "scons -c all"
Clean('all', 'build')
```

​	Sconscript:

```python
# -*- python -*-

#
# SConscript file to specify the build process, see:
# http://scons.org/doc/production/HTML/scons-man.html
#
Decider('MD5')
import platform
import fnmatch
import os

vars = Variables()
vars.Add(BoolVariable('DEBUG', 'Set to disable optimizations', True))
vars.Add(BoolVariable('PIC', 'Set to 1 for to always generate PIC code', False))
env = Environment(variables = vars)
#env.Replace(CXX = 'clang++')

# 编译选项
compile_options = {}
if platform.system() is 'Windows':
  compile_options['CXXFLAGS'] = '-D_CRT_SECURE_NO_WARNINGS /fp:fast /EHsc'
else:
  # Force ANSI (C++98) to ensure compatibility with MSVC.
  cxxflags = ['-ansi -pedantic']
  if env['DEBUG']:
    #compile_options['CPPDEFINES'] = '-DDEBUG'
    cxxflags.append('-O0 -g3 -ggdb')
    cxxflags.append('-Wall -Wextra -Werror')
    # -Werror
  else:
    cxxflags.append('-Os -g3 -ggdb -Wall -Wextra')
  if env['PIC']:
    cxxflags.append('-fPIC')
  compile_options['CXXFLAGS'] = ' '.join(cxxflags)
  compile_options['LINKFLAGS'] = '-ldl -L/usr/lib -L/opt/local/lib -L/usr/local/lib'

# 编译所有的源文件
def all_files(dir, ext='.cpp', level=6):
  files = []
  for i in range(1, level):
    files += Glob(dir + ('/*' * i) + ext)
  return files

# 查找所有lib文件
def all_libs(name, dir):
  matches = []
  for root, dirnames, filenames in os.walk(dir):
    for filename in fnmatch.filter(filenames, name):
      matches.append(os.path.join(root, filename))
  return matches

# Setup libiconv, if possible
libiconv_include = []
libiconv_libs = []
if all_libs('libiconv.*', '/opt/local/lib'):
  libiconv_include.append('/opt/local/include/')
  libiconv_libs.append('iconv')
else:
  if all_libs('libiconv.*', '/usr/lib'):
    libiconv_libs.append('iconv')

# 编译成静态库文件
# Add libzxing library.
libzxing_files = all_files('core/src')+all_files('core/src', '.cc')
libzxing_include = ['core/src']
if platform.system() is 'Windows':
  libzxing_files += all_files('core/src/win32')
  libzxing_include += ['core/src/win32']
libzxing = env.Library('zxing', source=libzxing_files,
  CPPPATH=libzxing_include + libiconv_libs, **compile_options)

# 编译二进制文件
# Add cli.
zxing_files = all_files('cli/src')
zxing = env.Program('zxing', zxing_files,
  CPPPATH=libzxing_include,
  LIBS=libzxing + libiconv_libs, **compile_options)

# Setup CPPUnit.
cppunit_include = ['/opt/local/include/']
cppunit_libs = ['cppunit']

# Add testrunner program.
test_files = all_files('core/tests/src')
test = env.Program('testrunner', test_files,
  CPPPATH=libzxing_include + cppunit_include,
  LIBS=libzxing + cppunit_libs, **compile_options)

# Setup some aliases.
Alias('lib', libzxing)
Alias('zxing', zxing)
Alias('tests', test)
```

