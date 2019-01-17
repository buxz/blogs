# Linux下配置 server-jre 8
## 1. 下载
1. 下载地址 https://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html
## 2. 上传并解压
1. 通过Xftp上传至 /home/data 目录下 （该目录不存在可以新建） 
2. 解压 
    ```text
    tar zxvf server-jre-8u201-linux-x64.tar.gz
    ```
3. 移动并改文件夹名称(jdk1.8.0_201 改为 java)
```text
     mv /home/data/jdk1.8.0_201/  /usr/local/java
```
## 3. 配置
1. 修改 /etc/profile 
    ```text
    # 进入文件
    vi /etc/profie
    # 在文件底部添加下列内容
     export JAVA_HOME=/usr/local/java
     export CLASSPATH=.:$JAVA_HOME/lib
     export PATH=$PATH:$JAVA_HOME/bin 
    ```
2. 配置生效
    ```text
    source /etc/profile
    ```
3. 校验
    ```text
    java -version
    ```

