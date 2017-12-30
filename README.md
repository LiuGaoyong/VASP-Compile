# VASP-5.4.1 的安装（基于Ubuntu 16.04编译环境）
---



##  一、Intel_parallel_studio_xe
### 1. 需要的安装包 （推荐`IPS2015`）

软件版本| 百度云链接 | 密码
---     |---| ---
2011    | http://pan.baidu.com/s/1nv4PYOx  | 59tu
2013    | http://pan.baidu.com/s/1o6sPd8m  | 无
2015    | https://pan.baidu.com/s/1o8U6yvw | 无


### 2. 安装步骤：
- 进入解压后的目录，修改权限

```bash
cd /home/当前用户/安装包/
chmod -R +rwx /home/当前用户/安装包/
```

- 运行安装脚本 `./install.sh`
- 一直敲回车 
- 到 view license，一路空格，最后==输入accept==然后回车
- 到 Alternative activation, 选择 use a license file, provide the full path, 输入：目录/lic文件名 （具体内容可能有出入）
- 可选择Typical Install全部安装，或只安装inter fortran composer, 安装包具体内容可参考说明文件。
- 目录（`默认/opt/intel`）已存在，因为里面放了刚才的lic文件，所以无所谓，overwrite yes。后面省略，安装完成。
- 加入环境变量

```bash
vim ~/.bashrc

# add the following
source /opt/intel/bin/ifortvars.sh intel64
source /opt/intel/mkl/bin/mklvars.sh intel64
```

##  二、openMPI

### **1. 需要的安装包（[官网](http://www.open-mpi.org/)）**

### **2. 安装步骤：**
- 解压openMPI安装包 `tar zxcf 文件名`
- 编译，终端输入
```bash
cd 
mkdir openmpi_compiled
sudo mv openmpi_compiled /opt
./configure --prefix=/opt/openmpi_compiled F77=ifort FC=ifort \
        #CC=icc CXX=icpc F77=ifort FC=ifort 若不加，则用gcc编译
make -j8 #采用八核心加快编译
make install
```

- 添加以下内容到 `.bashrc`文件
```bash
# openMPI setup
export MPI_HOME=/hoopt/openmpi_compiled
export PATH=$PATH:$MPI_HOME/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$MPI_HOME/lib
```


### **3. 测试mpi效果：**

- 测试环境变量
```bash
echo $PATH
echo $LD_LIBRARY_PATH

#结果中显示有刚才的bin和lib路径则为配置成功
```
- 测试`mpirun`
```bash
which mpirun
mpif90 -v #检查版本

#结果中显示 /opt/openmpi_compiled/bin/mpirun 则为配置成功
```


- 测试`mpi`代码 （使用普通账户）
```bash
cd /openmpi源代码目录/examples
make
mpirun -np 2 hello_c  #2为双核

#结果中显示以下内容则为配置成功
#   Hello, world, I am 0 of 2
#   Hello, world, I am 1 of 2
```

##  三、编译VASP
### 1. 需要的安装包（[我的GitHub](https://github.com/LiuGaoyong/VASP-src)）

### 2. 安装步骤：
- 测试环境变量



---
> ##### by LiuGaoYong（其他网名： Young、Smitee）
> ##### 欢迎邮件交流 liugaoyong_88@163.com or liugaoyong88@gmail.com

---

 #### 参考文章网址
- [linux ubuntu12.04 下的 vasp 5.2 的安装方法](http://blog.csdn.net/txcokokok/article/details/42219099)
- 
