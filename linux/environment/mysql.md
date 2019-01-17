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
    service mysql start // 启动
    service mysql stop // 关闭
    service mysql restart // 重启
    ```
6. 远程登录
    ```text
    # 开启防火墙
     systemctl start firewalld
    # 开启3306端口
     firewall-cmd --add-port=3306/tcp
    # 重启防火墙
    systemctl start firewalld
    # 配置root不限制登录IP
    mysql> use mysql;
    mysql> update user set host='%' where user='root';
    # 刷新权限
    mysql> flush privileges;
    # 修改用户密码 plugin 为 mysql_native_password 
    # 执行 select host,user,plugin,authentication_string from mysql.user; ，若 plugin非mysql_native_password 则需要修改密码
    mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456'; #更新一下用户的密码 root用户密码为newpassword
    ```

## 5. 常用操作
1. 登录
```text
    #  mysql -u root -p
```
2. 修改登录密码 (用户 root 新密码为 123456)
```sql
    mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456'; #更新一下用户的密码 root用户密码为newpassword
```
3. 退出登录
```sql
  mysql > exit
```
4. 开启3306端口
    1. 查看防火墙状态
     ```text
       systemctl status firewalld
     ```
     2. 开启防火墙
    ```text
    systemctl start firewalld
    ```
    3. 关闭防火墙
    ```text
    systemctl stop firewalld
    ```
    4. 开启3306端口
    ```text
    firewall-cmd --add-port=3306/tcp
    ```
5.