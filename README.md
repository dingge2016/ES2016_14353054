# ReadMe

## Description
The distributed operation layer (DOL) is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping. www.tik.ee.ethz.ch/~shapes/dol.html

![1](http://p1.bqimg.com/4851/6bcdf76515e41dc8.jpg)
 
*  Make工具简介

    *  在Linux和Ubuntu环境中，make工具主要被用来进行工程编译和程序链接

      * Makefile文件：告诉make以何种方式编译源代码和链接程序

      * make通过比较对应文件（规则的目标和依赖）的最后修改时间，来决定哪些文件需要更新、那些文件不需要更新。 http://blog.chinaunix.net/uid-9314244-id-2004686.html


* Ant工具简介
 	*  Ant是一种基于Java的build工具。

 	* Ant用Java的类来扩展。
	* Ant本身就是这样一个流程脚本引擎，用于自动化调用程序完成项目的编译，打包，测试等。
http://blog.163.com/qiangyongbin2000@126/blog/static/77517819201151653423687/

	* Ant的优点
		* 跨平台性。Ant是纯java语言编写的，所示具有很好的跨平台性。
		* 操作简单。Ant是由一个内置任务和可选任务组成的。Ant运行时需要一个XML文件(构建文件)。
		* 容易维护和书写，结构清晰。
		* Ant可以集成到开发环境中。

* Java与javac简介
  *  用途：编译或执行java代码 
  *  javac命令用来编译java文件
  *  java命令可以执行生成的class文件


## How to install

0.  本次实验环境在linux下进行，建议使用虚拟机安装Ubuntu（

	* VMWARE教程：
http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html
	* VIRTUALBOX教程：
http://jingyan.baidu.com/article/cdddd41c5eea3153ca00e160.html
	* Ubuntu下载：
http://www.ubuntu.com/download/desktop

* 安装一些必要的环境(ubuntu为例)：
```sh
$	sudo apt-get update
$	sudo apt-get install ant
$ 	sudo apt-get install openjdk-7-jdk
$	sudo apt-get install unzip
```
1. 下载文件(使用Vmware虚拟机，也可以从主机拷贝到虚拟机中去 http://jingyan.baidu.com/article/c33e3f48a5c153ea15cbb5b2.html
)：
```sh
$   sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz 
$   sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
```
2. 解压文件
    * 新建dol的文件夹
    * 将dolethz.zip解压到 dol文件夹中
    * 解压systemc 
   
```sh
$	sudo mkdir dol  
$	sudo unzip dol_ethz.zip -d dol
$	sudo tar -zxvf systemc-2.3.1.tgz
```

3.  编译systemc

* 解压后进入systemc-2.3.1的目录下
```sh
$	cd systemc-2.3.1 
```

* 新建一个临时文件夹objdir
```sh
$   sudo mkdir objdir
```
* 进入该文件夹objdir
```sh
$	cd objdir  
```

* 运行configure(能根据系统的环境设置一下参数，用于编译)


    $	sudo ../configure CXX=g++ --disable-async-updates


* 图为运行configure之后的截图
* 
![1](http://p1.bqimg.com/4851/008971529e9cb06c.jpg)

* 编译完后文件目录如下


    $   sudo make install        
    $   ls
![2](http://p1.bqimg.com/4851/8f47efa8400b61d9.jpg)

* 记录当前的工作路径(会输出当前所在路径，记下来，待会有用)


    $	sudo pwd
![3](http://p1.bqimg.com/4851/304c6dca995af623.jpg)

* 这里表示我当前的工作路径为 /root/systemc-2.3.1


3. 编译dol

* 进入刚刚dol的文件夹


	$	cd ../dol

* 修改build_zip.xml文件
* 找到下面这段话，就是说上面编译的systemc位置在哪里，


    <property name="systemc.inc" value="YYY/include"/>
    <property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/> 
* 把YYY改成上页pwd的结果。（注意，对于
64位系统的机器，lib-linux要改成lib-linux64）

* 然后是编译


    $ sudo ant -f build_zip.xml all 约成功会显示build successful

* 接着可以试试运行第一个例子
* 进入build/bin/mian路径下

    
    $	cd build/bin/main 然后运行第一个例子
    $	sudo ant -f runexample.xml -Dnumber=1

* 成功结果如图

![4](http://p1.bqimg.com/4851/605dca43aed4130e.jpg)

## Experimental experience

本次实验主要是配置dol，了解git和makedown的使用。
	git作为一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理 。
    Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
    在本次实验中，我简单的熟悉了git和makedown的使用，并且了解了其结构。