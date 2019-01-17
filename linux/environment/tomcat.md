# Linux下配置 tomcat 8 
## 1. 下载并上传
 1. 官网下载 https://tomcat.apache.org/download-80.cgi ，下载 core - tar.gz
 2. 上传至 /home/data 
## 2. 移动至 /usr/local/tomcat8 
1. 解压
    ```text
    tar xvzf apache-tomcat-8.5.37.tar.gz 
    ```
2. 移动
    ```text
    mv /home/data/apache-tomcat-8.5.37/ /usr/local/tomcat8
    ```
## 启动
    
```text
    cd /usr/local/tomcat8/bin
    ./startup.sh
```
## 开启8080端口供外界访问
```text
    # 开启端口
    firewall-cmd --add-port=8080/tcp
    # 关闭防火墙并重启
    systemctl stop firewalld
    systemctl start firewalld

```
