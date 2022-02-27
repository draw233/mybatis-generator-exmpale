MBG的主要功能是生成POJO，Mapper.xml和Dao层。

## quickly start

1. 添加依赖

```xml

<properties>
    <mysql.version>8.0.18</mysql.version>
    <mybatis.generator.version>1.4.0</mybatis.generator.version>
</properties>

<dependencies>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>${mysql.version}</version>
</dependency>
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>${mybatis.generator.version}</version>
</dependency>
</dependencies>
```

2. 配置generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="simple" targetRuntime="MyBatis3">
        <!-- 添加 db 表中字段的注释 -->
        <property name="addRemarkComments" value="true"/>
        <!--添加插件-->
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
        <!--分页插件-->
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin"/>
        <!--自定义CommentGenerator-->
        <commentGenerator type="org.example.MyCommentGenerator">
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator> <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/demo"
                        userId="root"
                        password="admin123">
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection> <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为true时把JDBC DECIMAL 和  
 NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
            <property name="useJSR310Types" value="true"/>
        </javaTypeResolver> <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.luck.pojo"
                            targetProject="./src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <sqlMapGenerator targetPackage="mappers" targetProject="./src/main/resources/">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator> <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.luck.dao"
                             targetProject="./src/main/java/">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="demo" tableName="t_log"/>
    </context>
</generatorConfiguration>
```

3. 创建主类，生成POJO，Mapper文件

```java
public static void main(String[]args)throws XMLParserException,IOException,InvalidConfigurationException,SQLException,InterruptedException,URISyntaxException{
        List<String> warnings=new ArrayList<String>();
        boolean overwrite=true;
        File file=new File("src/main/resources/generatorConfig.xml");
        ConfigurationParser cp=new ConfigurationParser(warnings);
        Configuration config=cp.parseConfiguration(file);
        DefaultShellCallback callback=new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator=new MyBatisGenerator(config,callback,warnings);
        myBatisGenerator.generate(null);
        }
```

## 参考

- [MybatisGenerator官网](https://mybatis.org/generator/index.html#)
- [# MyBatis Generator 超详细配置](https://juejin.cn/post/6844903982582743048)
- [自定义CommentGenerator实现中文注释](https://blog.csdn.net/u011781521/article/details/78161201)