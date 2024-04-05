

# 新版本maven依赖上传



#### 准备工作

|     工具      |     说明     |
| :-----------: | :----------: |
|     IDEA      |   开发工具   |
|   JAVA JDK    | java开发环境 |
|      Git      | 拉取代码工具 |
|      GPG      | 签名验证工具 |
|     Maven     |  maven仓库   |
| 一个gihub账号 |              |



### 接下来按着操作来执行：

#### 1.登录你的github账号 >>> [https://github.com/](https://github.com/)

![image-20240405192053716](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192053716.png)



### 2.登录完成之后 [前去新版本maven中央仓库上传地址](https://central.sonatype.com/)

![image-20240405192408797](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192408797.png)

### 3.选择你的github账号进行登录

![image-20240405192438881](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192438881.png)





#### 4. 获取token

![image-20240405192737823](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192737823.png)



![image-20240405192803732](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192803732.png)



### 5.复制token

![image-20240405192907575](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405192907575.png)



### 6.打开maven下的setting.xml文件

![image-20240405193043903](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405193043903.png)

### 7.将刚刚的token复制到setting.xml文件中，并修改id为central

![image-20240405193217071](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405193217071.png)





### 8.创建你的maven项目，将下面配置放到pom.xml文件中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
	
    <groupId>io.github.toppwx</groupId>
    <artifactId>mytest</artifactId>
    <version>1.1.0</version>

    <packaging>jar</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
    </properties>

    <name>${project.groupId}:${project.artifactId}</name>

    <description>描述信息</description>
    <url>你的项目仓库地址</url>
    
    
    <licenses>
        <license>
            <name>The Apache License, Version 2.0</name>
            <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
        </license>
    </licenses>

    <developers>
        <developer>
            <name>作者名称</name>
            <email>你github的邮箱</email>
            <organization>Sonatype</organization>
            <organizationUrl>http://www.sonatype.com</organizationUrl>
        </developer>
    </developers>

    <scm>
        <connection>scm:git:git://github.com/simpligility/ossrh-demo.git</connection>
        <developerConnection>scm:git:ssh://github.com:simpligility/ossrh-demo.git</developerConnection>
        <url>http://github.com/simpligility/ossrh-demo/tree/master</url>
    </scm>


	//发布地址
    <distributionManagement>
        <snapshotRepository>
            <id>ossrh</id>
            <url>https://s01.oss.sonatype.org/content/repositories/snapshots</url>
        </snapshotRepository>
        <repository>
            <id>ossrh</id>
            <url>https://s01.oss.sonatype.org/service/local/staging/deploy/maven2</url>
        </repository>
    </distributionManagement>



    <build>
        <plugins>
            <!--    Maven插件发布    -->
            <plugin>
                <groupId>org.sonatype.central</groupId>
                <artifactId>central-publishing-maven-plugin</artifactId>
                <version>0.4.0</version>
                <extensions>true</extensions>
                <configuration>
                    <publishingServerId>central</publishingServerId>
                    <tokenAuth>true</tokenAuth>
                    <autoPublish>false</autoPublish>
                </configuration>
            </plugin>


            <!--    签名插件        -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-gpg-plugin</artifactId>
                <version>1.5</version>
                <executions>
                    <execution>
                        <id>sign-artifacts</id>
                        <phase>verify</phase>
                        <goals>
                            <goal>sign</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!--     生成源文件       -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
                <version>2.2.1</version>
                <executions>
                    <execution>
                        <id>attach-sources</id>
                        <goals>
                            <goal>jar-no-fork</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <!--     生成源javadocs文件       -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-javadoc-plugin</artifactId>
                <version>2.9.1</version>
                <executions>
                    <execution>
                        <id>attach-javadocs</id>
                        <goals>
                            <goal>jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>
</project>
```



### 9.到github创建代码仓库

![image-20240405193729413](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405193729413.png)



![image-20240405193834845](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405193834845.png)

### 创建完成之后，将你的项目共享到刚刚创建的仓库来吧



### 10. 共享之后修改配置文件几个地方

```
<groupId>io.github.toppwx</groupId>
<artifactId>mypwx</artifactId>
<version>1.0.0</version>
```

### 一. groupId的填写

![image-20240405194117089](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405194117089.png)

![image-20240405194149460](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405194149460.png)



### 二. artifactId你的项目名称

### 三. 版本，自定义设置即可



```
<url>你的项目仓库地址</url>
```

### 四.url你刚刚创建的仓库地址

```
例如：<url>https://github.com/toppwx/mypwx</url>
```





### 11.去下载 [GPG](https://gnupg.org/download/index.html#sec-1-2)

### window系统下载

![image-20240405194648705](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405194648705.png)



### 下载之后安装，安装之后打开



![image-20240405194902485](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405194902485.png)



![image-20240405194944074](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405194944074.png)



### 创建密钥后共享到服务器上

![image-20240405195044610](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195044610.png)





### 打开你的maven项目

![image-20240405195235983](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195235983.png)



### 执行发布的时候需要密码要输入(前提是你设置了密钥密码的化则需要输入)



#### 执行编译成功之后的提示

![image-20240405195426266](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195426266.png)





### 打开你的

![image-20240405195503749](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195503749.png)



### 选择发布

![image-20240405195533275](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195533275.png)





### 之后发布成功之后就显示绿色图标

![image-20240405195733491](C:\Users\PWX\Desktop\PWX技术学习资料\maven上传到中央仓库\mavn中央仓库上传\img\image-20240405195733491.png)



### 之后你就可以去通过dependency引入jar包

```
<dependency>
    <groupId>io.github.pwxpwxtop</groupId>
    <artifactId>fastservice</artifactId>
    <version>1.2.0</version>
</dependency>
```

