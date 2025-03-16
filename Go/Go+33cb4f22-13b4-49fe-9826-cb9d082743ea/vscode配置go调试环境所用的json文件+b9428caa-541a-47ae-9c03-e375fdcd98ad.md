# vscode配置go调试环境所用的json文件

```JSON
{
    // 使用 IntelliSense 了解相关属性。
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
      // mac
        {
            "name": "Go Debug",
            "type": "go",
            "request": "launch",
            "mode": "auto",
          // 下面路径根据实际的情况填写，如果要在一个vscode中调试多个程序就要多个这种对象
            "program": "${workspaceFolder}",
            "env": {
                "GOPATH": "/Users/midou/go",
                "GOROOT": "/usr/local/go"
            },
            "args": [],
            "showLog": true
        },
      
      // win
      {
      "name": "Go Debug",
      "type": "go",
      "request": "launch",
      "mode": "auto",
    // 下面路径根据实际的情况填写，如果要在一个vscode中调试多个程序就要多个这种对象
      "program": "${workspaceFolder}//cmd//admin",
      "env": {
          "GOPATH": "C://Users//lyh//go",
          "GOROOT": "C://Program Files//Go"
      },
      "args": [],
      "showLog": true
    },
        {
          ...
        }
    ]
}
```

