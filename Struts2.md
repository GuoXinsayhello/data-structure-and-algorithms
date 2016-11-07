1
--
JRE:Java  Runtime  Enviromental(java运行时环境)。也就是我们说的JAVA平台，所有的Java程序都要在JRE下才能运行。包括JVM和JAVA核心类库和支持文件。
与JDK相比，它不包含开发工具——编译器、调试器和其它工具。<br>
遇到404错误有可能是jsp文件的位置不对，应该在 WebContent文件夹下面创建jsp文件。点击指定的类，按下F1可以获得该类的帮助信息。

10
--
在struts.xml里面，package标签用于区分action可能会出现重名的情况，表明action来自哪一个包，namespace可以不写，表示接收一切action，囊括了其他namespace处理不了的action，如果要写要以/开头，可以是/XXX,当然在浏览器访问的时候就要添加上/XXX。对于action标签，如果没有写name，那么name默认值是"success"。遇到一个问题，如果从前一个项目拷贝一个新项目的时候，马士兵说要修改web project settings的web context root改为和新的工程名一样的名字，但是我改之后还是没有用。weird。
