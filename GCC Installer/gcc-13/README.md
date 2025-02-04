# Linux 编译安装 GCC 13

**注意: 使用 `./installer.sh` 前请先将文件Copy到外层，确保路径上没有特殊字符和空格。**
> 某些第三方组件的构建脚本适配不是很好，特殊字符和空格会导致编译不过。

GCC 13发布啦，本脚本在之前GCC 11的基础上做了稍许更新 。

由于GCC从5开始[对版本号进行重新规范](https://gcc.gnu.org/develop.html#num_scheme)，所以这里的编译脚本以后以主版本号为准。

GCC 13的大致(C/C++)内容如下：

1. AddressSanitizer在Linux上默认开启 `detect_stack_use_after_return=1` 。
2. 新的调试符号压缩选项 `-gz=zstd` ，废弃老旧的压缩格式 `-gz=zlib-gnu` 。
3. 可以使用 [SARIF](https://sarifweb.azurewebsites.net/) 规范输出诊断信息。
4. 两个非标准 `std::pair` 开启 deprecated 标记 。
5. 支持 STABS 调式信息格式。
6. `-Ofast, -ffast-math and -funsafe-math-optimizations` 优化。
7. 新的Warning选项和属性。
8. 一些新的C23/C++23特性支持。
9. 针对C语言常见的动态长度数组的设计模式的支持。
10. 增加 `-nostdlibc++` 用于告知 g++ 不要默认链接libstdc++库。

详见: https://gcc.gnu.org/gcc-13/changes.html

## 编译安装 GCC 13.X.X

### 准备环境及依赖项

1. 支持 ISO C++ 11 的编译器（GCC 4.8及以上）
2. C标准库及头文件
3. 用于创建Ada编译器的GNAT
4. 支持POSIX的shell或GNU bash
5. POSIX或SVR4的 awk工具
6. GNU binutils
7. gzip 版本1.2.4及以上     （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
8. bzip2 版本 1.0.2及以上    （此处可下载 http://www.bzip.org/）
9. GNU make 工具 版本3.80及以上 （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
10. GNU tar工具 版本1.14及以上   （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
11. perl 版本5.6.1-5.6.24      （此处可下载 http://www.perl.org/）
12. tar或zip和unzip工具 （此处可下载 http://www.info-zip.org)
13. gmp库 版本4.3.2及以上 （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
14. mpfr库 版本3.1.0及以上 （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
15. mpc库 版本1.0.1及以上 （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 下载 ）
16. isl 版本 0.15及以上 （可由GNU镜像列表 http://www.gnu.org/prep/ftp.html 或自动选择最佳镜像 http://ftpmirror.gnu.org 中gcc目录中的infrastructure目录下载 ）
17. zstd
18. awk
19. m4
20. automake
21. autoconf
22. gettext
23. gperf
24. cmake

## 我编译的环境

### 系统

CentOS 7 & CentOS 8

### 编译的依赖库和工具

+ m4 latest
+ autoconf latest
+ automake 1.16.5
+ libtool 2.4.7
+ pkgconfig 0.29.2
+ gmp 6.3.0
+ mpfr 4.2.1
+ mpc 1.3.1
+ isl 0.24
+ libatomic_ops 7.8.0
+ bdw-gc 8.2.4
+ zstd 1.5.5
+ openssl 3.1.2
+ libexpat 2.5.0
+ libxcrypt 4.4.36
+ gdbm latest
+ readline 8.2

### 编译目标

+ gcc 13.2.0
+ bison 3.8.2
+ binutils 2.41
+ python 3.11.6 *[按需]*
+ gdb 13.2
+ global 6.6.10
+ lz4 1.9.4 *[非必须]*
+ zlib 1.3 *[非必须]*
+ libffi 3.4.4 *[非必须]*
+ ncurses 6.4 *[非必须]*
+ xz 5.4.4 *[非必须]*

### 注

+ (所有的库都会被安装在**$PREFEX_DIR**里)

### 额外建议

给特定用户安装 gdb的pretty-printer 用以友好打印stdc++的stl容器

1. 在执行 install.sh 脚本前安装 ncurses-devel 和 python-devel， 用于编译gdb和开启python功能
2. gdb载入后可使用 ```so [安装目录]/load-libstdc++-gdb-printers.py``` 手动加载gdb的pretty printers

### History

+ 2023-04-28    Created
+ 2023-07-15    Update
  + python -> 3.11.4
  + bdw-gc -> 8.2.4
  + openssl -> 3.0.9
  + libxcrypt -> 4.4.36
  + gdb -> 13.2
  + xz -> 5.4.3
+ 2023-10-07    Update
  + python -> 3.11.6
  + gmp -> 6.3.0
  + mpfr -> 4.2.1
  + gcc -> 13.2.0
  + binutils -> 2.41
  + openssl -> 3.1.2
  + zlib -> 1.3
  + global -> 6.6.10
  + xz -> 5.4.4
