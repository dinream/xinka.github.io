1. 假设当前文件夹为 project
2. 初始文件
	server.go
	myserver.proto
3. 在命令行中执行
	```bash
	go mod init project
	go mod tidy 
	go mod downloads
	protoc --go_out=. --go-grpc_out=. --proto_path=. myserver.proto
	```

4. 生成可执行文件
5. 执行文件
	```bash
	./project
```

