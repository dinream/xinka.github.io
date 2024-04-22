[最新最全！吊打付费！这13款才是真正的电脑必装软件！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV12z421o74i/?spm_id_from=333.1007.tianma.6-4-22.click&vd_source=59461060c1867e9bf731e467ae6f00b5)
# vscode 配置
```json
    "window.restoreWindows": "none",

    "explorer.confirmDelete": false,
    
    "diffEditor.ignoreTrimWhitespace": false,

    "files.eol": "\n"
```

[官方文档](https://code.visualstudio.com/docs/cpp/launch-json-reference)
# win11 配置 node js 
[Node.js安装及环境配置之Windows 11篇_nodejs安装及环境配置 win11-CSDN博客](https://blog.csdn.net/liangfei8402/article/details/136099884)


# C++
[十六、windows11下VSCode配置C/C++编译环境_vscode c语言编译环境-CSDN博客](https://blog.csdn.net/qq_41742043/article/details/127750483)

c_cpp_properties.json
```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "D:\\Program Files\\mingw64\\bin\\g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-msvc-x64"
        }
    ],
    "version": 4
}
```


launch.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++.exe build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\Program Files\\mingw64\\bin\\gdb.exe",		/**** 修改成自己bin目录下的gdb.exe，这里的路径和电脑里复制的文件目录有一点不一样，这里是两个反斜杠\\ ****/
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "task g++"
        }
    ]
}

```
tasks.json
```json
{
    "version": "2.0.0",
    "tasks": [
        {
        "type": "shell",
        "label": "task g++",
        "command": "D:\\Program Files\\mingw64\\bin\\g++.exe",	/**** 修改成自己bin目录下的g++.exe，这里的路径和电脑里复制的文件目录有一点不一样，这里是两个反斜杠\\ ****/
        "args": [
            "-g",
            "${file}",
            "-o",
            "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "-I",
            "D:\\Users\fyn\\Pro_Cod\\leetcode",      /**** 修改成自己放c/c++项目的文件夹，这里的路径和电脑里复制的文件目录有一点不一样，这里是两个反斜杠\\ ****/
            "-std=c++17"
        ],
        "options": {
            "cwd": "D:\\Program Files\\mingw64\\bin"	/**** 修改成自己的bin目录，这里的路径和电脑里复制的文件目录有一点不一样，这里是两个反斜杠\\ ****/
        },
        "problemMatcher":[
            "$gcc"
        ],
        "group": "build",
        
        }
    ]
}


```

### C++ 终端闪退的问题

C/C++ Compile Run 并且选中 run in external terminal 





# 命令行
```sh
# 清空 DNS 缓冲
ipconfig /flushdns


```
