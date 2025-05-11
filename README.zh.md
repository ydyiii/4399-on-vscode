by何洁芳
自定义标题功能概述：
1. 本项目支持自定义 Webview 面板的标题。
2. 用户可以根据自己的需求设置独特的标题，提升使用体验。
3. 若未设置自定义标题，系统将依次使用游戏本身的标题或默认标题 "4399 on VSCode"。
   
使用方法：
步骤一：定位配置文件
打开项目根目录下的 `package.json` 文件。此文件负责管理项目的各种配置信息，自定义标题的配置也在此文件中进行设置。

步骤二：找到自定义标题配置项
在 `package.json` 文件里，找到 `configuration` 部分下的 `4399-on-vscode.title` 属性。该属性控制着 Webview 面板的标题显示。
```
"configuration": {
    // 整个配置块的标题，用于描述本组配置的用途
    "title": "4399 on VSCode",

    // 配置项的具体属性定义
    "properties": {

        // 用户代理（User-Agent）设置
        "4399-on-vscode.user-agent": {
            "description": "4399 on VSCode 发出的请求头中的 UA（User-Agent），用于模拟浏览器环境",
            "type": "string",
            "default": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36"
        },

        // 请求头中的 Referer 设置
        "4399-on-vscode.referer": {
            "description": "4399 on VSCode 发出的请求头中的 Referer，表示请求来源地址",
            "type": "string",
            "default": "https://www.4399.com/"
        },

        // 本地服务器监听端口号
        "4399-on-vscode.port": {
            "description": "4399 on VSCode 本地服务器端口，修改后必须重启 VSCode 才能生效",
            "type": "integer",
            "ignoreSync": true,  // 表示该设置不会被同步到其他设备（如通过 VSCode Settings Sync）
            "default": 44399     // 默认使用的端口号
        },

        // 是否打印扩展日志
        "4399-on-vscode.printLogs": {
            "description": "控制是否在开发工具中输出调试日志，用于排查问题",
            "type": "boolean",
            "default": false      // 默认不输出日志
        },

        // Webview 页面标题设置
        "4399-on-vscode.title": {
            "description": "控制 Webview 标题，若留空则使用默认标题：游戏名称 或 '4399 on VSCode'",
            "type": "string",
            "default": ""         // 默认为空，使用自动标题
        }

    }
}
```
步骤三： 将 "default" 字段的值修改为你期望的自定义标题，例如 "摸鱼必备"。
```
"4399-on-vscode.title": {
    "description": "控制 Webview 标题, 若留空则使用游戏名称或 4399 on VSCode",
    "type": "string",
    "default": "摸鱼必备"
}
```



by何洁芳
随即游戏功能概述：
“随机游戏” 功能为用户提供了在 4399 游戏平台上随机挑选一款游戏进行游玩的体验。由于游戏链接是随机生成的，所以可能会出现所选链接对应的游戏不存在或无法正常访问的情况。
使用步骤：
步骤1：启动 VSCode：打开 Visual Studio Code 编辑器。
步骤2：触发命令：按下 Ctrl + Shift + P（Windows/Linux）或 Cmd + Shift + P（Mac）打开命令面板。
步骤3：选择随机游戏：在命令面板中输入 4399-on-vscode.random 并回车，即可触发随机游戏功能。

核心代码：
随机游戏的核心代码位于 4399-on-vscode/src/extension.ts 文件中。
```
random: () => {
    // 调用 play 函数，传入一个随机生成的小游戏链接
    play(
        // 构建目标 URL：
        // 基础路径为 4399 的小游戏 flash 页面
        "https://www.4399.com/flash/" +
        
        // 使用 Math.random() 生成一个 0 到 1 之间的随机小数，
        // 乘以 10000 得到 0 到 10000 之间的数，
        // 再通过 Math.floor 向下取整得到整数，
        // 最后加上 200000，使最终范围为 200000 ~ 209999
        (Math.floor(Math.random() * 10000) + 200000) +

        // 拼接 .htm 扩展名，构成完整的游戏页面地址
        ".htm"
    );
}
```