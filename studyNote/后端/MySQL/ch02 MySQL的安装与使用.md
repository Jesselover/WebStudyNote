DBMS分类
分为两类
+ 基于共享文件系统的DBMS（Access）
+ 基于客户机——服务器的DBMS（MySQL，Oracle、SqlServer）

MySql数据库：
1. 关系型数据库（表存储、使用sql语言操作）

## 基础使用方法

### 1. MySQL服务的启动和停止

+ Windows10：<kbd>Ctrl+Shift+Esc</kbd> -> 查看服务->点击，启动和停止服务

- `win + R` --> `services.msc` --> `mySql80`

+ 通过命令行CMD（**管理员权限**）

  ```
  # 启动
  net stop [mysql服务名]
  net stop mysql80
  # 停止
  net start [mysql服务名]
  net start mysql 80
  ```

- 默认开机自启

### 2. MySQL 的服务的登录和退出

1. 方式一：通过 mysql 自带客户端（MySQL 版本号 Command Line Client），输入密码，只限于 root 用户 

2. 方式二：通过 windows 自带的客户端 (使用此方式，需要配置path环境变量)

  ```bash
  # 登录
  mysql [-h 主机名 -P 端口号] -u 用户名 -p密码
  mysql [-h 127.0.0.1] [-P 3306] -u root -p
  # 退出
  exit 或 ctrl+C
  ```

  举例

  ```
  # 本地登录
  mysql -h localhost -p 3306 -U root -p
  # root用户快捷登录
  mysql -u root -p密码
  ```

#### 配置path环境变量

系统 --> 系统信息 --> 高级系统设置 --> 环境变量 --> path --> 编辑 --> 新建mysql路径 `C:\Program Files\MySQL\MySQL Server 8.0\bin\` (最后一个`\`可加可不加 )

### 3.MySQL的常用命令

1. 查看当前所有的数据库
    ```sql
    show databases;
    ```
2. 打开指定的数据库
    ```
    use 库名;
    ```
3. 查看当前所在的库
    ```
    select database();
    ```
4. 查看当前库的所有表
    ```
    show tables;
    ```
5. 查看其他库的所有表
    ```
    show tables from 库名;
    ```
6. 查看表中的所有数据
    ```
    select * from 表名;
    ```
7. 创建表
    ```
    create table 表名(
        列名 列类型,
          列名 列类型,
          ...
    );
    ```
8. 查看表结构
    ```
    desc 表名;
    ```

9. 查看服务器版本
   方式一: 登录到 mysql 服务端
   ```
   select version();
   ```
   方式二：未登录到 mysql 服务端
   ```
   mysql --version
   mysql -V
   ```

### 4. MySQL 语法规范

1. 不区分大小写，但是由规范，建议关键字大小写，表名、列名小写
2. 每条命令用分号 `;` 结尾
3. 每条命令根据需要，可以进行缩进或换行
4. 注释
   + 单行注释：`#注释文字`
   + 单行注释：`-- 注释文字`
   + 多行注释：`/* 注释文字 */`