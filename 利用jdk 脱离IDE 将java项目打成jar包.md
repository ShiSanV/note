像Eclipse，IDEA这种JAVA的IDE让程序员变得越来越笨了，连怎样编译这种基础的工作都已经不会了。离开了IDE之后不会导包，不会编译，几乎成了废人一个。不仅降低了工作效率，而且对JAVA的编译连接过程也全部知道，实在是需要及时补习一下。

下面就以JDK1.5为例，为新手介绍一下如何在没有IDE的环境中编译出class文件和jar包

一、首先到oracle官网下载相应的JDK

这一步看似很简单，但其实很多工作了几年的程序员也未必知道如何到官网下载JDK，我见过很多程序员JDK都从别人那里获得或者从百度上下载。

首先到http://www.oracle.com/technetwork/java/javase/archive-139210.html 这个页面可以找到JAVA历史上全部的JDK版本，我们挑自己需要的下载即可，注意区分32位还是64位。另外下载之前可能要注册成Oracle会员，需要花个几分钟，但是不需要交钱



二、配置环境变量

这个我就不细说了，网上一大把。反正只要在cmd中执行java和javac能出来东西就行



三、编写源代码

1、首先建立一个根目录，名字随便起，我这里就叫javalearning

2、建立一个目录用于存放源代码，比如src

3、建立一个目录用于存放编译好的class二进制文件，比如classes

4、在src目录下编写相应的java源代码，不要创建子目录



此时编写一个HelloWorld.java作为主类，还有一个工具类MyTools.java,注意此时HelloWorld.java是import MyTools.java的，换句话说这是个依赖

```java
package test.ant;
import test.ant.MyTools;
import com.alibaba.fastjson.JSONObject;
public class HelloWorld{
public static void main(String[] args){
   System.out.println("Hello world!!!!");
   System.out.println(new MyTools().getTime());
}
}

package test.ant;

import java.util.Date;

public class MyTools {
	public String getTime(){
		return new Date().toString();
	}
}
```


四、把源码编译成class文件

我们先来一个一个java文件编译试试：

![img](https://img-blog.csdn.net/20170629112236515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

注意：-d参数后跟class文件的输出目录，如果没有-d参数会在当前目录下生成class文件，而且没有根据包名建立相应的目录，会很乱，所以推荐使用-d参数

我们会发现编译MyTools.java的时候没什么问题，但是HelloWorld.java怎么也编译不出来，原因就是HelloWorld.java依赖于MyTools.java，必须同时编译，所以应该这样写

```shell
javac -d .\classes .\src\*.java
```

这就是为什么我推荐把所有java文件都放到src根目录下面的原因。
编译完成之后就可以在classes目录下面看到一个文件夹，名字就是我们的包名，还有N个子文件夹，里面就是classes文件了，如下图

![img](https://img-blog.csdn.net/20170629112719375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

那么有了这两个二进制文件之后如何运行呢？



五、运行classes文件

当我们直接试图执行HelloWorld.class文件时会提示:"找不到或无法加载主类"的错误，如下

![img](https://img-blog.csdn.net/20170629113114740?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

所以需要制定-classpath参数规定class文件的搜索范围，并指明程序主入口

```shell
java -classpath .\classes test.ant.HelloWorld
```

![img](https://img-blog.csdn.net/20170629113328030?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

这样程序就可以正常运行了。

六、打包成jar包

为了程序的交付方便，我们一般会把所有的可执行文件和资源打包成jar包并发给别人，这一步在没有IDE的时候如何实现呢？

紧接上面的例子，我们把程序打包成一个jar包

首先cd到classes目录，这样可以让我们jar包打出来之后最外层文件夹是属于包名的，不然会报找不到类的错误

![img](https://img-blog.csdn.net/20170629120032970?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

这样一个jar包就做好了，用winrar打开看看

![img](https://img-blog.csdn.net/20170629120236812?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

要注意上面除了META-INF之外的目录要是属于包名的目录，如果第一个目录名字为classes那么说明包打错了。

现在尝试着执行一下这个jar包

![img](https://img-blog.csdn.net/20170629120457995?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

发现会报一个没有主清单属性的错误，说明此时程序还是找不到主函数

所以我们需要修改META-INF目录下面的MANIFEST.MF文件，添加下面一行，其中test.ant.HelloWorld就是程序主入口的类

```
Main-Class: test.ant.HelloWorld
```

![img](https://img-blog.csdn.net/20170629123131099?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

注意：冒号后面类名字前面有一个空格，如果不填这个空格会报不是java jar的错误

再次执行，发现已经可以正常运行了，我们可以把这个jar包发给别人使用了

![img](https://img-blog.csdn.net/20170629120903924?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

七、讨论有外部依赖库的情况

有些时候我们引入了很多第三方的jar包作为依赖，此时应该如何编译的，下面以阿里的fastjson第三方库为例介绍如何编译依赖jar包的文件。

首先我们把HelloWorld.java修改一下，添加一个功能，使用fastjson产生一个json字符串

```java
package test.ant;
import test.ant.MyTools;
import com.alibaba.fastjson.JSONObject;
public class HelloWorld{
public static void main(String[] args){
   System.out.println("Hello world!!!!");
   System.out.println(new MyTools().getTime());
   JSONObject obj = new JSONObject();
	obj.put("1", "one");
	obj.put("2", "two");
	System.out.print(obj.toJSONString());
}
}
```

此时我们把下载好的jar包存放在src目录下

![img](https://img-blog.csdn.net/20170629121311675?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)


然后先试试按照之前的方法进行编译行不行

![img](https://img-blog.csdn.net/20170629121647890?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

发现报错说找不到fastjson的类定义，说明直接编译时不行的，此时我们需要指定外部库的位置，同样还是通过-classpath参数来引入

```shell
javac -d .\classes -classpath .\src\fastjson-1.2.32.jar .\src\*.java
```

此时再编译就不会报错了
注：如果引入了多个第三方jar包，那么jar包的路径应以:或;分割，其中windows用;  Linux用: 如：

```shell
javac -d .\classes -classpath .\src\fastjson-1.2.32.jar;2.jar;3.jar;4.jar .\src\*.java
```

编译出来之后再classes文件夹下发现还是只有两个class文件，没有发现fastjson的jar包，那这样能运行么,先试试看


发现执行到fastjson那一句的时候报错了，所以我们在执行的时候仍然要指定第三方jar包的位置

```shell
G:\javalearning>java -classpath .\classes;.\src\fastjson-1.2.32.jar test.ant.HelloWorld
```

注：如果引入了多个第三方jar包，那么jar包的路径应以:或;分割，其中windows用;  Linux用: 如：

```shell
G:\javalearning>java -classpath .\classes;.\src\fastjson-1.2.32.jar;2.jar;3.jar;4.jar test.ant.HelloWorld
```

此时fastjson就可以发挥作用了

此时fastjson就可以发挥作用了

![img](https://img-blog.csdn.net/20170629122506460?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

然后我们再尝试把我们自己的class和第三方jar包打到同一个jar包中去
首先还是使用之前的方法打jar包

```shell
jar -cvf my.jar .\*
```

然后再像上面一样修改这个jar的配置文件

```
Manifest-Version: 1.0
Created-By: 1.5.0 (Sun Microsystems Inc.)
Main-Class: test.ant.HelloWorld
```

此时执行这个jar包会报找不到类的错误，因为我们没有指定fastjson包的位置

![img](https://img-blog.csdn.net/20170629123338673?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

那么怎么把fastjson的jar包和当前jar包关联起来呢？-classpath参数在这里已经不好用了，在网上搜了一把，是这样的

首先使用和之前一样的命令进行打包：

```shell
jar -cvf my.jar .\*
```


制作出my.jar之后，然后把fastjson的jar包放到my.jar同一级目录里面，也就是把他们俩放在一起

![img](https://img-blog.csdn.net/20170629141907706?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

此时我们再去修改jar包中的配置文件

在MANIFEST.MF中添加如下代码：

```
Main-Class: test.ant.HelloWorld
Class-Path: fastjson-1.2.32.jar
```

![img](https://img-blog.csdn.net/20170629142047501?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbHZzaGFvcm9uZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

注意这里还有一个深渊巨坑：MANIFEST.MF的最后一行一定要是空行，不然倒数第一行相当于没写！
然后再来执行一下发现没问题了



最后如果你觉得把一大堆jar包放在一起比较乱，可以把所有的第三方jar放在一个lib目录下面，然后修改MANIFEST.MF为

```
Class-Path: lib\fastjson-1.2.32.jar
```

或者如果你有很多jar包需要依赖，可以以空格分割，如

```
Class-Path: lib/some.jar lib/some2.jar
```


八、使用Ant自动化编译

看过上面的编译过程就会发现一个很死板的地方，就是所有源代码必须都放在src目录下，而且不能产生子目录。因为一旦不在一个目录下，要一起编译.java文件就很难了。而如果不能一起编译的话，就会遇到import找不到类的问题，所以Ant这个工具就非常实用了。除了可以指定编译目录以外，Ant还可以自动建立或者删除文件夹，自动打jar包等等很多实用功能，对于上面的例子，先写一个没有fastjson依赖的build.xml，放在javalearning文件夹下

如果你电脑没有ant的话需要去官网下一个，地址：http://archive.apache.org/dist/ant/binaries/  注意选择版本，目前最新版的1.10需要使用jdk1.8，由于我用的是JDK1.5所以我下载的是jakarta-ant-1.5-bin.zip   这个版本，下载之后，还需要配置环境变量

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<project name="HelloWorld" default="run" basedir=".">
<property name="src" value="src"/>
<property name="dest" value="classes"/>
<property name="hello_jar" value="my.jar"/>
<target name="init">
   <mkdir dir="${dest}"/>
</target>
<target name="compile" depends="init">
   <javac srcdir="${src}" destdir="${dest}"/>
</target>
<target name="build" depends="compile">
   <jar jarfile="${hello_jar}" basedir="${dest}"/>
</target>
<target name="run" depends="build">
   <java classname="test.ant.HelloWorld" classpath="${hello_jar}"/>
</target>
<target name="clean">
   <delete dir="${dest}" />
   <delete file="${hello_jar}" />
</target>
<target name="rerun" depends="clean,run">
   <ant target="clean" />
   <ant target="run" />
</target>
</project>
```


cd到javalearning目录，运行ant run 可以实现和上面手动javac编译一样的效果，并执行和输出。但是编译出的jar包仍需要手动修改MANIFEST.MF


如果想要把fastjson这种第三方jar包也一起编译进去，可以这样写

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<project name="HelloWorld" default="run" basedir=".">
<property name="src" value="src"/>
<property name="dest" value="classes"/>
<property name="hello_jar" value="my.jar"/>
<property name="fastjson" value="src\fastjson-1.2.32.jar"/>
<target name="init">
   <mkdir dir="${dest}"/>
</target>
<target name="compile" depends="init">
   <javac srcdir="${src}" destdir="${dest}" classpath="${fastjson}"/>
</target>
<target name="build" depends="compile">
   <jar jarfile="${hello_jar}" basedir="${dest}" />
</target>
<target name="run" depends="build">
   <java classname="test.ant.HelloWorld" classpath="${hello_jar};${fastjson}"/>
</target>
<target name="clean">
   <delete dir="${dest}" />
   <delete file="${hello_jar}" />
</target>
<target name="rerun" depends="clean,run">
   <ant target="clean" />
   <ant target="run" />
</target>
</project>
```

主要就是在compile这个target中声明了classpath指向fastjson jar包的位置
另外在run这个target中使用classpath指出了两个jar包，一个是我们自己写的程序的jar包，另一个是fastjson的jar包，和上面手动写一样，多个classpath之间用;或者:分割，看你用的是windows还是Linux了。

如果你想用java -jar来运行的话，还是需要手动修改MANIFEST.MF这个文件

————————————————
版权声明：本文为CSDN博主「lvshaorong」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/lvshaorong/article/details/73881568

------------------------------------------------------------------------------------------------------------------

补充：使用jar -uf 向jar包中导入文件 例如：

存在/data/txt/1.txt 这样的目录结构

若想把 目录下的1.txt导入到jar包根目录中 则我们需要进入到 txt目录下 使用

```shell
jar -uf xx.jar 1.txt
```

若想把txt/1.txt 都导入到jar包根目录中 则需要进入到data目录下使用

```shell
jar -uf xx.jar txt
```

成功后 在jar包根目录中会出现txt/1.txt 同样的目录结构

替换jar包中的文件也可以使用这种方式实现

-------------------------------------------------------------------------------------------------------------------------------------------------------

```shell
jar -tvf xx.jar
```

 可以展示jar包中的目录结构

------

```shell
jar -xf xxx.jar [filename]
```

可以导出jar包中的文件 若不指定filename 则将jar包中的文件全部导出

------

```
C:\Users\lan>jar
用法: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
选项:
    -c  创建新档案(create)
    -t  列出档案目录(list)
    -x  从档案中提取指定的 (或所有) 文件(extract)
    -u  更新现有档案(update)
    -i  为指定的 jar 文件生成索引信息(index)

    -v  在标准输出中生成详细输出(verbose)
    -f  指定档案文件名(filename)
    -m  包含指定清单文件中的清单信息
    -n  创建新档案后执行 Pack200 规范化
    -0  仅存储; 不使用任何 ZIP 压缩
    -P  保留文件名中的前导 '/' (绝对路径) 和 ".." (父目录) 组件
    -M  不创建条目的清单文件
    -e  为捆绑到可执行 jar 文件的独立应用程序
        指定应用程序入口点

    -C  更改为指定的目录并包含以下文件
```

