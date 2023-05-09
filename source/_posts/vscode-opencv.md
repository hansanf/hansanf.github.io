---
title: vscode_opencv
date: 2021-11-12 23:13:26
tags:
 - vscode 
 - opencv

---
### win10 vscode 加载opencv库
从头到尾的配置：
1、Visual Studio Code 配置
2、openCV 配置
3、MinGw 配置
4、cmake 配置
完整过程参考：[https://blog.csdn.net/zhaiax672/article/details/88971248](https://blog.csdn.net/zhaiax672/article/details/88971248)

如果使用vscode已经可以编译c++程序了，即可以省略掉大部分vscode和的MinGW的配置过程（只要vscode中配置好opencv的头文件路径和库路径即可）

opencv的配置过程，实际上就是库的加载过程，如果是已经编译好的opencv库，只要配置好头文件路径和库文件路径即可，如果是下载的源文件，则需要通过cmake进行编译。
cmake编译opencv参考：[https://blog.csdn.net/zhaiax672/article/details/88971248](https://blog.csdn.net/zhaiax672/article/details/88971248)

opencv已经编译好了之后，把头文件路径和库文件路径加到系统的环境变量中。

最后配置vscode的三个.json文件：`launch.json`、`tasks.json`、`c_cpp_properties.json`

各自的配置如下：
`launch.json`：主要注意**miDebuggerPath**和**program**两项
```yaml
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        
        
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.o",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/Program Files/mingw-w64/x86_64-5.3.0-posix-seh-rt_v4-rev0/mingw64/bin/gdb.exe",
            "preLaunchTask": "g++",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        },
    ]
}
```
`tasks.json`：
主要修改**args**中大i、大L和小l
这里实际上就是g++ -I 头文件路径 -L 库文件路径 -l 库文件名
注意实际库文件的命名和这里写库文件名的区别   实际库文件名=lib库文件名.dll.a
```yaml
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "command": "g++",
    "args": [
        "-g", 
        "-std=c++11", 
        "${file}", 
        "-o", 
        "${fileBasenameNoExtension}.o",  
        "-I", "D:\\Program Files\\opencv\\opencv\\build\\include",
        "-I", "D:\\Program Files\\opencv\\opencv\\build\\include\\opencv2",
        "-I", "D:\\Program Files\\opencv\\opencv\\build\\include\\opencv",
        "-L", "D:\\Program Files\\opencv\\opencv\\build\\x64\\MinGW\\lib",
        "-l","opencv_core3416",
        "-l","opencv_dnn3416", 
        "-l","opencv_features2d3416", 
        "-l","opencv_flann3416", 
        "-l","opencv_highgui3416", 
        "-l","opencv_imgcodecs3416",
        "-l","opencv_imgproc3416",
        "-l","opencv_ml3416", 
        "-l","opencv_objdetect3416", 
        "-l","opencv_photo3416", 
        "-l","opencv_shape3416", 
        "-l","opencv_stitching3416",
        "-l","opencv_superres3416", 
        "-l","opencv_video3416", 
        "-l","opencv_videoio3416", 
        "-l","opencv_videostab3416",
        "-l","opencv_ts3416"

  
    ],// 编译命令参数
    "problemMatcher":{
        "owner": "cpp",
        "fileLocation":[
            "relative",
            "${workspaceFolder}"
        ],
        "pattern":[
            {
                "regexp": "^([^\\\\s].*)\\\\((\\\\d+,\\\\d+)\\\\):\\\\s*(.*)$",
                "file": 1,
                "location": 2,
                "message": 3
            }
        ]
    },
    "group": {
        "kind": "build",
        "isDefault": true
    }
  }
  
```

`c_cpp_properties.json`：
主要修改**includePath**和**compilerPath**
```yaml
{
    "configurations": [
        {
            "name": "win",
            "includePath": [
                "${workspaceFolder}/**",
                "D:\\Program Files\\opencv\\opencv\\build\\include",
                "D:\\Program Files\\opencv\\opencv\\build\\include\\opencv2",
                "D:\\Program Files\\opencv\\opencv\\build\\include\\opencv"         
            ],
            "defines": [],
            "compilerPath": "C:/Program Files/mingw-w64/x86_64-5.3.0-posix-seh-rt_v4-rev0/mingw64/bin/gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}

```
如果不是完全照搬上述的配置，需要注意：

 - `launch.json`的**preLaunchTask**和`tasks.json`中的**label**内容需要一致，否则报错，比如修改`launch.json`的**preLaunchTask**为g++.exe，则会报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/0539074e59fe455882f429a521c318b4.png)
三个json文件的作用：
`tasks.json` (build instructions)
`launch.json` (debugger settings)
`c_cpp_properties.json` (compiler path and IntelliSense settings

vscode配置官方教程：[https://code.visualstudio.com/docs/cpp/config-mingw](https://code.visualstudio.com/docs/cpp/config-mingw)

