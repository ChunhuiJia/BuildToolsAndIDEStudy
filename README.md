# CMakeAndIDEStudy

# 1.基于VSCode和CMake实现C/C++开发 | Linux篇

[基于VSCode和CMake实现C/C++开发 | Linux篇-视频（推荐）](https://www.bilibili.com/video/BV1fy4y1b7TC?from=search&seid=17147323456237447809&spm_id_from=333.337.0.0)

配套的课件获取方法：微信公众号搜索“VSCode”，点击课程七讲即可在线看。

[课程配套代码](VScodeAndCmake_7Lessons/source_code)

issue:

课程中tasks.json编写的时候，make的label中command写的是mingw32-make，好像是需要安装这个才能用，把这个地方替换为make比较通用，up主给的课程配套代码已经修改为make了。

## launch.json配置

launch.json，这个文件是在左侧边栏Run and Debug页，create launch.json生成的在.vscode文件夹的文件，需要修改的地方为:

mark1:  "program": "${workspaceFolder}/build/my_cmake_exe",表示要调试的可执行文件的路径，${workspaceFolder}表示当前工作文件夹的绝对路径。

mark2: "preLaunchTask": "Build",  "Build"是tasks.json命令操作的label名称，通常在tasks.json执行的是程序编译操作。

```json
//launch.json

{

​    // Use IntelliSense to learn about possible attributes.

​    // Hover to view descriptions of existing attributes.

​    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387

​    "version": "0.2.0",

​    "configurations": [

​        {

​            "name": "g++ - 生成和调试活动文件",

​            "type": "cppdbg",

​            "request": "launch",

​            "program": "${workspaceFolder}/build/my_cmake_exe",      //mark1: The absolute path of the executable file

​            "args": [],

​            "stopAtEntry": false,

​            "cwd": "${workspaceFolder}",

​            "environment": [],

​            "externalConsole": false,

​            "MIMode": "gdb",

​            "setupCommands": [

​                {

​                    "description": "为 gdb 启用整齐打印",

​                    "text": "-enable-pretty-printing",

​                    "ignoreFailures": true

​                }

​            ],

​            "preLaunchTask": "Build",       //mark2: Operations before starting debug

​            "miDebuggerPath": "/usr/bin/gdb"

​        }

​    ]

}
```

## tasks.json配置

如果CMakeLists.txt写好的话，下方tasks.json的内容就可以满足编译task的需求，不需要进行修改。

tasks.json内容代表的command操作内容：

```bash
mkdir build

cd build/

cmake ..

make
```



```
//tasks.json
{

​    "version": "2.0.0",

​    "options": {

​        "cwd": "${workspaceFolder}/build"

​    },

​    "tasks": [

​        {

​            "type": "shell",

​            "label": "cmake",

​            "command":"cmake",

​            "args": [

​                ".."

​            ]

​        },

​        {

​            "label": "make",

​            "group": {

​                "kind": "build",

​                "isDefault": true

​            },

​            "command":"make",

​            "args": [



​            ]

​        },

​        {

​            "label": "Build",

​            "dependsOrder": "sequence",

​            "dependsOn":[

​                "cmake",

​                "make"

​            ]

​        }

​    ]

}
```

