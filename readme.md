## ubuntu 环境

1. 使用apt 安装gcc和openmpi
2. 使用gcc/g++/mpicc/mpicxx 编译运行一个最简单的hello world 程序（网上寻找样例），了解这几个命令的区别。
3. 使用`g++ test.cpp -L ${libmfem} -lmfem -o test` 编译运行test程序，注意使用   `mpiexec -n 4 ./test` 执行, 其中 ${libmfem}应当替换为 libmfem.so 或者libmfem.a 所在路径（不含文件名）。 理解 “-L” "-lmfem" "-o" 这几个参数的意思。
4. 若仅存在libmfem.a（静态库），则上述命令无法链接所有需要的符号，这个时候应当怎么办（大致思路）。

## msys2

1. msys2 安装 [MSYS2](https://www.msys2.org/)
2. msys2 与ide 和 terminal 集成 [Terminals - MSYS2](https://www.msys2.org/docs/terminals/)   在windows terminal/vscode intergated terminal  打开msys2 bash shell 
3. 在 https://packages.msys2.org/ 搜索 wget, mfem 和 msmpi 软件包，安装 ucrt64 的版本
4. 在 https://github.com/qbisi/mfem-windows/releases 下载预编译的软件包`wget https://github.com/qbisi/mfem-windows/releases/download/0.1/mingw-w64-ucrt-x86_64-mfem-4.7-1-any.pkg.tar.zst` `wget https://github.com/qbisi/mfem-windows/releases/download/0.1/mingw-w64-ucrt-x86_64-mumps-5.7.3-3-any.pkg.tar.zst`
5. `pacman -U *.tar.zst` 安装软件包

## msmpi

1. windows dll: "C:\Windows\System32\msmpi.dll"
2. windows mpiexec : https://github.com/microsoft/Microsoft-MPI/releases/download/v10.1.1/msmpisetup.exe "C:\Program Files\Microsoft MPI\Bin\mpiexec.exe"
3. msys2 lib: "/ucrt64/lib/libmsmpi.dll.a" msmpi.dll动态库的符号表


## compile 

```
mpicxx test.cpp -o test -Wl,--allow-multiple-definition -static -lmfem -lHYPRE \
$(pkgconf --static --libs --cflags netcdf libcurl hdf5 snappy liblz4 liblzma  klu UMFPACK mumps-dmo scotch mpfr superlu superlu_dist)
```

## run

```bash
#add "C:\Program Files\Microsoft MPI\Bin\" to PATH
export PATH=/c/Program\ Files/Microsoft\ MPI/Bin:$PATH

mpiexec -np 4 ./test.exe
```