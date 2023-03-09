## Maven

> Maven是一款自动化构建工具,专注服务于Java平台的项目构建和依赖管理
> 
> -   搜索jar包
> -   管理jar包
> -   通过坐标直接在统一仓库中搜索,工程需要的jar包可以直接引用仓库即可

**使用Maven可以自动解决jar包间的依赖关系/解决jar包冲突**

项目构建

> 构建项目并非创建项目,他有以下过程
> 
> 1.  清理
> 2.  编译
> 3.  测试
> 4.  报告
> 5.  打包
> 6.  安装
> 7.  部署

### 

基本使用

-   准备
    -   配置JAVA_HOME
    -   配置MAVEN_HOME
-   Maven目录结构
    -   bin 可执行脚本文件
    -   boot
    -   conf 存放核心配置文件 `settings.xml`
    -   lib 存放jar包
-   Maven 使用前配置
    -   设置本地仓库
    -   设置阿里云镜像仓库(远程仓库)
    -   设置Maven依赖的JDK版本

### 

Maven核心概念

> Java中规定了 约束>配置>代码

#### 

POM

> Project Object Model
> 
> 将整个项目封装为对象模型,便于操作和更改

#### 

插件与目标

> 插件:由jar包及配置文件组成,Maven的核心仅仅定义了抽象的生命周期,所有操作都由插件完成
> 
> 目标:每个插件都有多个功能,每个功能就是插件目标

#### 

Maven的生命周期

> Maven生命周期定义了各个构建环节的执行顺序(定义了构建项目7个步骤的执行顺序)

-   Maven由三套相互独立的生命周期
    -   Clean Lifecycle 清理
    -   **Default Lifecycle 编译/测试/打包/安装/部署(执行后面的会从编译开始依次执行**
    -   Site Lifecycle 生成项目报告,站点,发布站点

#### 

Maven仓库(重点)

-   仓库分类
    -   本地仓库,为当前本机电脑的上的所有Maven工程服务
        -   为本地项目提供jar包
    -   远程仓库,为本地仓库提供jar包
        -   私服,架设在当前局域网环境下,为为局域网范围内所有Maven提供服务
        -   中央仓库
        -   中央仓库的镜像
-   仓库中文件
    -   jar包
    -   配置文件
        -   pom.xml,定义了当前jar包所依赖jar包的坐标

#### 

Maven坐标(重点)

-   在Maven中,使用GAV来定义本地仓库中的jar包
    -   groupId,公司或组织域名倒叙
    -   artifactId,项目或模块名称
    -   version,版本号
-   特点
    -   g-a-v,就是本地仓库jar包位置
    -   a-v,jar包名称
-   自己的Maven工程必须先安装才能进入本地仓库

#### 

Maven依赖(重点)

-   依赖的范围
    
    -   `语法 <scope>...</scope>`
    -   `compile`[默认范围],在`main/test/Tomcat`下均有效
    -   `test`,仅在test目录下有效
        -   一般`junit`会使用`test`依赖范围
    -   `provided`,可以在`main/test`下有效,不会提交到Tomcat
        -   一般使用`servlet-api`时`provided`依赖范围
-   依赖的传递性
    
    -   路径最短者优先[就近原则]
    -   先声明者优先原则
-   统一管理版本号
    
    -   ```xml
        <properties>
        	<spring-version>3.5.1</spring-version>
        </properties>
        ...
        <version>${spring-version}</version>
        ```
        

#### 

Maven继承

> 父工程的特点是打包方式必须是pom方式
> 
> 打包方式共三种`Java-jar,web-war,父工程-pom`
> 
> 父工程的jar包都会被子工程继承

#### 

Maven聚合

-   将子工程聚合到父工程中,在清除或安装父工程时会自动清除或安装子工程
    
-   安装父工程时,子工程的顺序是由依赖关系决定的
    
-   ```xml
    <modules>
            <module>day01_HelloMaven</module>
            <module>day01_MakeFriend</module>
            <module>day01_MakeFriend</module>
            <module>day01_HelloFriend</module>
        </modules>
    ```
    

### 

Maven酷站

> 搜索坐标数值及所需jar包的依赖关系

-   [http://mvnrepository.com](http://mvnrepository.com/)
-   [http://search.maven.org](http://search.maven.org/)

### 

Maven创建Web工程

-   Maven导包到Web工程要选择打包方式为`<packaging>war</packaging>`