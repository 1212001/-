# maven安装

### 1、[maven官方下载地址](https://mirrors.sonic.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip)

### 2、解压到文件夹

### 3、打开D:\开发工具\apache-maven-3.6.3\conf\setting.xml

#### 1、在对应位置添加如下代码，绑定jdk

```xml
<profiles>
  <profile>
      <id>jdk-1.8</id>
      <activation>
	    <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
      </activation>
    <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
	<maven.compiler.target>1.8</maven.compiler.target>
	<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
</profile>
```

#### 2、修改下载镜像为阿里云仓库

```xml
<mirrors>
  <!-- 阿里云仓库 -->
        <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>	  <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
        </mirrors>
```

#### 3、设置本地仓库位置

```xml
 <localRepository>D:\开发工具\apache-maven-3.6.3\repository</localRepository>
```

