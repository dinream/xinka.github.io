# 在线差查看 gpu 的配置 

1. 查看GPU 名称
	lshw -C display
2. 在线查看 GPU 配置
	[GPU 配置信息](https://www.techpowerup.com/gpu-specs/)


# GPU 驱动安装

1. 从NVIDIA官网下载最新的NVIDIA驱动程序。

   从官方网址下进行下载（打开慢把.com换成.cn）
   [Official Drivers | NVIDIA![icon-default.png?t=N7T8](D:\Users\fyn\Documents\Note\Typora\字典生成\python文档.assets\icon-default-17058459398052.png)https://www.nvidia.com/Download/index.aspx?lang=en-us](https://www.nvidia.com/Download/index.aspx?lang=en-us)
   
2. 使用root用户登录系统，然后运行以下命令：

	\# sh NVIDIA-Linux-x86_64-xxx.run

3. 按照提示完成安装。
	1. -no-nouveau-check：安装驱动时禁用nouveau
	2. -no-opengl-files：只安装驱动文件，不安装OpenGL文件

安装完成后，您可以使用 nvidia-smi 命令查看 NVIDIA GPU 的状态。
