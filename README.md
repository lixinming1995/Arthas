# Arthas

官方API文档：https://arthas.aliyun.com/doc/quick-start.html

官方下载地址：https://arthas.aliyun.com/download/latest_version?mirror=aliyun
Arthas主要是阿里开发的一款方便快速定位问题的工具。主要用来排查没有打印日志的部分数据值

1.运行arthas-boot.jar包，java -jar arthas-boot.jar
2.可能会出现端口占用，可以使用命令指定端口 
java -jar arthas-boot.jar --telnet-port 9888 --http-port -1
Arthas定位war包问题：ps aux | grep xxx-tomcat定位tocmat具体进程号
定位到war包所在tomcat的进程号，其他命令与jar包一致，但是war包不能热部署替换.java文件

# Arthas反编译代码

Arthas文件反编译并热部署到线上，无需换包即可手动修改线上代码不需要进行重启。
1. 通过jad命令将class文件编译到本地linuxjad --source-only com.qax.ngsoc.service.impl.TestServiceImpl > /tmp/TestServiceImpl.java
2. 修改源代码vim /tmp/TestServiceImpl.java
3.  查询类的编辑器hash码sc -d *TestServiceImpl | grep classLoaderHash
4.  编译修改好的java文件mc -c 5d099f62 /tmp/TestServiceImpl.java -d /tmp
5.  重新加载修改好的Class文件redefine /tmp/com/qax/ngsoc/service/impl/TestServiceImpl.class

# watch比较常见的类
watch xxx.DutyManagerController getManagerList returnObj ; returnObj返回值

watch xxx.DutyManagerController getManagerList params ; params传参/params[0]第一个传参值

watch xxx.DutyManagerController getManagerList returnObj -e -x 2 ; returnObj 返回值/-e 异常时打印/-x 2 打印两次调用

# trace比较常见的用法

trace xxx.service.impl.DutyManagerServiceImpl getManagerList  不打印JDK源码
trace xxx.service.impl.DutyManagerServiceImpl getManagerList --skipJDKMethod false 打印源码


# 堆空间大小排序

Arthas的堆栈内存空间问题排查
通过命令定位内存占用最大的包：
ps  aux  --sort -rss | grep /opt/qax/ngsoc/repository/microservices/ |head -n 10
然后通过Arthas定位到包文件
dashboard：通过命令查看堆空间大小

heapdump
将文件下载出来使用任意工具分析，定位到具体代码行数，这里使用的是Jprofiler

# Arthas的堆栈CPU占用排查
查询调用链路：ps aux | grep 进程号
通过Arthas的profiler命令排查

Arthas的堆栈CPU占用排查
使用Jprofiler导入test.jfr
更多profiler命令：
https://arthas.aliyun.com/doc/arthas-tutorials.html?language=cn&id=command-profiler

参考：https://github.com/alibaba/arthas/issues/1416






