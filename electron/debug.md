# Debug main.js

launch.json file :
```
{
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Debug Electron Main Process",
        "type": "node",
        "request": "launch",
        "cwd": "${workspaceFolder}",
        "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron",
        "args": ["${workspaceFolder}/main.js"],
        "protocol": "inspector",
        "windows": {
          "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron.cmd"
        }
      }
    ]
  }
```
在这个配置中，我们使用 Visual Studio Code 的 Node.js 调试类型来配置 Electron 主进程的调试。以下是各个配置选项的解释：

"name": "Debug Electron Main Process"：配置调试会话的名称，你可以根据需要进行更改。

"type": "node"：指定调试类型为 Node.js。

"request": "launch"：指定调试请求类型为启动。

"cwd": "${workspaceFolder}"：设置工作目录为当前工作区文件夹。

"runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron"：指定要运行的 Electron 可执行文件的路径。${workspaceFolder} 是工作区文件夹的占位符。

"args": ["${workspaceFolder}/main.js"]：指定传递给 Electron 可执行文件的参数，这里是主进程文件 main.js 的路径。

"protocol": "inspector"：使用 V8 Inspector 协议进行调试。

"windows": {...}：如果你是在 Windows 上进行调试，可以在此处指定 Windows 平台特定的配置，例如 runtimeExecutable。

确保将这个配置添加到你的项目的 .vscode 文件夹下的 launch.json 文件中。然后，你就可以在 Visual Studio Code 中选择 "Debug Electron Main Process" 配置，并点击调试按钮来启动并调试你的 Electron 主进程代码了

  
