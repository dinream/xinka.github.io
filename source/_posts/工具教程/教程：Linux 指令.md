# Zip
> 将 /root/test 这个目录下所有文件和文件夹打包为当前目录下的 test.zip：

```bash
zip -q -r test.zip /home/test
```

> 将 /root/test 这个目录下所有文件和文件夹加密打包为当前目录下的 test.zip：

```bash
zip -q  -e -r test.zip /home/test
```

> 将 test.zip 解压到指定目录：

```bash
unzip -o -d /root test.zip
```

# Tar
1. 解压缩到指定文件夹：

```bash
tar -xf archive.tar -C /path/to/destination
```

上述命令将解压缩名为 "archive.tar" 的 tar 文件到指定的目标文件夹 "/path/to/destination"

2. 解压缩特定文件到指定位置：

```bash
tar -xf archive.tar /path/to/file -C /path/to/destination
```

上述命令将解压缩名为 "archive.tar" 的 tar 文件中的特定文件 "/path/to/file" 到指定的目标文件夹 "/path/to/destination"。

3. 压缩为指定文件名：

```
tar -cf new_archive.tar /path/to/source
```

上述命令将把指定文件夹 "/path/to/source" 打包成名为 "new_archive.tar" 的 tar 文件。

# Vim
复制 yy 粘贴 p

# Bzip2
1. 安装
```shell
apt-get install bzip2
```
2. 解压 `.bz2` 文件：

```
bzip2 -d file.bz2
```
	其中，`file.bz2` 是你要解压的文件名。解压后的文件将会放置在当前目录，并且会去掉原始文件的 `.bz2` 扩展名。例如，如果你解压的文件名为 `example.bz2`，解压后会生成一个名为 `example` 的文件。

3. 如果你想同时保留原始的 `.bz2` 文件，可以使用 `-k` 选项：

```
bzip2 -dk file.bz2
```

	这样，解压后的文件和原始的 `.bz2` 文件都会存在。

4. `bzip2` 命令也支持通过管道方式解压文件，例如：
```
bzcat file.bz2 > outputfile
```

	上述命令将解压 `.bz2` 文件并将输出重定向到 `outputfile`。



# GoLang
## 安装

>去 golang 的网站 [https://studygolang.com/dl](https://studygolang.com/dl)
```sh
wget https://studygolang.com/dl/golang/go1.20.3.linux-amd64.tar.gz
```

> 解压至 /usr/local 
```sh
tar -C /usr/local -xzf go1.20.3.linux-amd64.tar.gz 
```
> 修改终端文件 
```sh
export GOROOT=/usr/local/go 
export PATH=$PATH:$GOROOT/bin 
export CGO_ENABLED=1

export PATH="/usr/bin:/bin:/usr/local/bin:/sbin:/usr/sbin:$PATH"

```

>配置 
```sh
go env -w GO111MODULE=on 
go env -w GOPROXY=https://goproxy.cn,direct
```







# Docker 
## Docker 安装与卸载
### Centos
#### 安装
1. 更新系统软件包列表：

   ```bash
   sudo yum update
   ```

2. 安装所需的依赖软件包，以便使用 Docker：

   ```bash
   sudo yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

3. 添加 Docker 软件源：

   ```bash
   sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

4. 安装 Docker 引擎：

   ```bash
   sudo yum install -y docker-ce docker-ce-cli containerd.io
   ```

5. 启动 Docker 服务：

   ```bash
   sudo systemctl start docker
   ```

6. （可选）将当前用户添加到 Docker 用户组，以便无需使用 sudo 运行 Docker 命令：

   ```bash
   sudo usermod -aG docker $USER
   ```

   请注意，这将需要您重新登录才能生效。

7. 验证 Docker 是否成功安装：

   ```bash
   docker version
   ```

   这将显示 Docker 引擎的版本信息。
#### 卸载

1. 停止 Docker 服务：运行以下命令停止正在运行的 Docker 服务：

   ```bash
   sudo systemctl stop docker
   ```

2. 删除 Docker 软件包：运行以下命令删除 Docker 软件包及其依赖项：

   ```bash
   sudo yum remove docker-ce docker-ce-cli containerd.io
   ```

3. 删除 Docker 数据和配置：运行以下命令删除 Docker 相关的数据和配置文件：

   ```bash
   sudo rm -rf /var/lib/docker
   ```

   请注意，这将删除 Docker 所有镜像、容器和卷数据，以及配置文件。如果您希望保留这些数据，请备份相应的目录。

4. 删除 Docker 用户组：运行以下命令删除 Docker 用户组：

   ```bash
   sudo groupdel docker
   ```

   这将删除名为 "docker" 的用户组。如果您在安装 Docker 时没有创建该用户组，可能会出现错误提示，可以忽略此步骤。

5. 删除 Docker 相关的存储库文件：如果您添加了 Docker 的存储库文件，请运行以下命令删除这些文件。存储库文件通常位于 `/etc/yum.repos.d/` 目录下，文件名可能类似于 `docker-ce.repo` 或 `docker.repo`。

	```bash
   sudo rm -rf /etc/yum.repos.d/docker*.repo
   ```

   请注意，这一步仅适用于您手动添加了 Docker 的存储库文件的情况。
6. 删除 Docker 命令。
	```bash
	rm -rf /usr/bin/docker
	```

完成上述步骤后，Docker 将从 CentOS 系统中被完全卸载。

## Docker 使用

> 创建一个容器，允许使用 GPU
```sh
docker run -itd -v /home/fengyuening/container_2204/:/root/ --gpus all --name fyn_ubuntu_2204 ubuntu:22.04
```

> 产生基础镜像：
```sh
docker build -f Dockerfile . -t bell/backend:1.0
docker pull mysql:5.7
```

> Docker保存为离线文件：
```shell
docker save -o mysql-5.7.tar mysql:5.7
```

> 将离线文件载入镜像：
```shell
docker load -i mysql-5.7.tar
```

> 修改镜像
	bell/backend:1.0作为基础镜像，对其进行修改产生新镜像。
```shell
docker build -f Dockerfile2 . -t bell/backend:1.1
```



## Apach

> 使用 ApacheBench (ab) 工具进行性能测试
```shell
ab -c 400 -n 1000 http://localhost:8080/
# - `ab`：是 ApacheBench 工具的命令。
# - `-c 400`：表示并发请求数（concurrency），即同时发送的请求数量。在这个例子中，设置为 400，表示会同时发送 400 个请求。
# - `-n 1000`：表示总请求数（number of requests），即发送的总请求数量。在这个例子中，设置为 1000，表示会发送总共 1000 个请求。
# - `http://localhost:8080/`：表示待测试的目标 URL，即请求要发送到的服务器的地址和端口。
```
	但是 17 年的一个帖子说`ab` 是一个糟糕的测试服务器的工具，它只是 http/1.0，甚至没有在这里使用 keepalive。尝试使用 -race 标志（如 `go build -race` ）构建服务器或使用 `go run -race main.go` 运行



# 数据竞争检测 TODO

使用 `-race` 标志可以在 Go 中进行数据竞争检测，以帮助发现并发程序中的潜在竞态条件。

1. 构建服务器：
    
    - 打开终端或命令提示符。
    - 进入服务器的代码目录。
    - 运行以下命令：`go build -race`。
    - 如果构建成功，则会生成一个可执行文件，可以使用 `./<可执行文件名>` 执行服务器。
2. 运行服务器：
    
    - 打开终端或命令提示符。
    - 进入服务器的代码目录。
    - 运行以下命令：`go run -race main.go`。
    - 这将使用 Go 运行时环境直接运行 `main.go` 文件，并启动服务器。

使用 `-race` 标志构建或运行服务器后，Go 编译器/运行时将会执行数据竞争检测。如果检测到潜在的竞态条件，它将在终端或命令提示符中显示相关的警告或错误信息。

注意，使用 `-race` 标志构建或运行服务器可能会增加程序的执行时间和资源消耗，因为数据竞争检测会引入一些额外的开销。因此，它通常用于开发和调试阶段，以帮助发现并解决并发问题，而不是在生产环境中使用。


