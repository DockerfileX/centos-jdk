# CentOS-JDK

## 1. 简要说明

基于CentOS系统，集成了JDK8的环境。

当前JDK的小版本为251，Dockerfile的版本为7.8.251(第一个版本号意思是CentOS7，第二个是JDK8，第三个是JDK的小版本)

## 2. 特性

1. CentOS 7.x
2. JDK 1.8.0_xxx
3. JCE
4. TZ=Asia/Shanghai
5. en_US.UTF-8
6. JAVA_HOME=/usr/local/lib/jdk1.8.0_xxx/
7. jstatd

## 3. 拉取与制作标签

1. pull

   在自动构建后，拉取下来

   ```sh
   docker pull nnzbz/centos-jdk
   ```

2. tag(注意修改**xxx**为当前版本号)

   ```sh
   docker tag nnzbz/centos-jdk:latest nnzbz/centos-jdk:xxx
   ```

3. push(注意修改**xxx**为当前版本号)

   ```sh
   docker push nnzbz/centos-jdk:xxx
   ```

## 4. 创建、运行并进入容器

```sh
docker run --net=host --name centos-jdk -it nnzbz/centos-jdk /bin/sh
```

## 5. 进入容器

```sh
docker exec -it centos-jdk /bin/bash
```

## 6. 如果要运行jstatd监控

进入容器中，执行如下命令

如果要前台运行（注意Ctrl+C则会关闭程序，直接关闭命令行则不会）

```sh
jstatd -J-Djava.rmi.server.hostname=<ip> -J-Dcom.sun.management.jmxremote.authenticate=false -J-Dcom.sun.management.jmxremote.rmi.port=1099 -J-Dcom.sun.management.jmxremote.ssl=false -J-Djava.security.policy=/usr/local/jvm/jstatd.all.policy
```

如果要后台运行

```sh
nohup jstatd -J-Djava.rmi.server.hostname=<ip> -J-Dcom.sun.management.jmxremote.authenticate=false -J-Dcom.sun.management.jmxremote.rmi.port=1099 -J-Dcom.sun.management.jmxremote.ssl=false -J-Djava.security.policy=/usr/local/jvm/jstatd.all.policy >> /usr/local/output.log 2>&1 &
```

- -J-Djava.rmi.server.hostname一定要填写正确的docker容器的宿主IP地址
- jstatd默认端口号是1099，如果要自定义，用 ```-p``` 参数指定端口号
- 运行命令后，可在宿主服务器运行 ```ss -anp | grep 1099``` 查看是否启动成功

## 7. jdk更新重新制作镜像

从oracle官网下载最新的jdk8的文件，文件名为 `jdk-8uxxx-linux-x64.tar.gz` (其中xxx为jdk8的小版本号)，放入本级目录，将旧的文件删除，并打开 `Dockerfile` 文件，用新的小版本号全文替换旧的版本号，最后提交更新
