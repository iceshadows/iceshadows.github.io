#VSCode配置安装(2.0)
##VSCode 快捷键
`Ctrl+P`  开启命令行（Console）
`Ctrl+Shift+B` 编译
##推荐的插件
###1、给文件添加Icon
`Material Icons VSCode Icons
##编译调试C/CPP
###Ubuntu
####准备
1、`sudo apt install build-essential` 安装C编译环境
2、安装 C/CPP 插件
####配置调试和编译文件，launch.json和task.json
我们打开在vscode中打开了文件夹后，看到vscode在最左边第四个好像虫子的图标，这个就是调试的，点一下，然后看到最上面有个齿轮那里，点一下出来一个面板，选择刚刚装好的c++插件
 
 Lauch.json:
 
 	{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "cppdbg",
            "request": "launch",
            "name": "Launch",
            "program": "${workspaceFolder}/a.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
	}
 现在调试已经配置完成了，如果你现在按绿色运行调试，是会报错的，为什么？因为在调试之前并没有经过编译过程，所以在你的工作目录并没有找到你在launch.json里面配置的program项的a.out文件。
所以很显然我们下面就要来配置task.json来进行编译的配置（也是很简单，没改什么东西），按ctrl+p打开命令面板，然后输入>task后选择配置任务

在这里选择Other（即配置通过调用外部命令行完成编译）这里又会生成一个task.json文件，修改内容如下 

	{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build C",
            "type": "shell",
            "command": "gcc",
            "args": [
                "-g",
                "${workspaceRoot}/Test.c"
            ],
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "showReuseMessage": true
            },
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
}
	
其实真正修改的也就只有command项和args项，command就是调用的控制台命令（就是我们平常用控制台编译时输入的命令），然后args就是命令行参数了，-g参数是必须的，否则到时候没有调试信息，vscode会无法设置断点。

第三 这些都配置好后我们就可以先按ctrl+shift+B先编译，然后再按绿色箭头调试了	