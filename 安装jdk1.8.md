## 安装jdk1.8

### 1、[官网下载地址](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

### 2、选择对应的操作系统和位数

选择windows x64 (选择64位)

### 3、选择安装目录

### 4、配置环境变量

#### 1、右键我的电脑，选择属性，选择高级系统设置

#### 2、选择系统变量，选择新建 

变量名为：`JAVA_HOME`, 变量值为：安装路径例如 `D:\Java\jdk1.8.0_191`

#### 3、再次新建

变量名为 CLASSPATH,变量值为：`.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`     //记得前面有个"."

#### 4、选择path

点击编辑，在末尾新增：`%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;`

#### 5、检测是否安装成功

选择win+r键打开dos，输入`java` 回车，有东西输出就行，输入`javac` 回车，有东西输出

