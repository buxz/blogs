# Linux下配置MySQL
## 1. 下载
1. 官网下载 https://dev.mysql.com/downloads/mysql/ 
2. 操作系统选择-Linux -Generic
3. 版本选择 64，下载  (mysql-8.0.13-linux-glibc2.12-x86_64.tar.xz)
## 2. 上传至服务器
1. 上传文件至 /home/data, 并解压
2. 修改文件夹名称为 mysql   
  ```text
mv mysql-8.0.12-linux-glibc2.12-x86_64/ mysql 
```
3. 移动至 /usr/local/mysql
```text
mv mysql/ /usr/local/
```
## 3. 添加mysql用户及mysql用户组
```text
groupadd mysql
useradd -r -g mysql mysql
```
## 4. 安装
1. 切进mysql 软件目录
```text
cd /usr/local/mysql
```
2. 修改 /usr/local/mysql 目录权限
```text
chown -R mysql:mysql ./
```
3. 初始化
    1. 先安装 libnuma 
    ```text
        yum install libnuma
        yum -y install numactl
        yum install libaio1 libaio-dev
    ```
    2. 初始化
    > 该处初始化之后会生成root的初始密码，需要记录下备用
    ```text
        ./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/
    ```
4. 配置
    1. 配置my.cnf , vi /etc/my.cnf,进入文件
    ```text
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    basedir=/usr/local/mysql
    datadir=/usr/local/mysql/data
    port=3306
    socket=/tmp/mysql.sock
    log-error=error.log
    user = mysql
    ```
    2. 配置 profile 
    > vi /etc/profile 进入文件,在底部添加下列内容
    ```text
    export PATH=$PATH:/usr/local/mysql/bin:/usr/local/mysql/lib
    ```
    3. 配置生效
    ```text
    source /etc/prpfile
    ```
5. 建立mysql服务并添加进系统服务
    ```text
    cp -a ./support-files/mysql.server /etc/init.d/mysql
    chmod +x /etc/init.d/mysql
    chkconfig --add mysql
    # 检验服务是否有效
    chkconfig  --list mysql
    ```
5. 启动mysql 
    ```text
    service mysql start
    ```