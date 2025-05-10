
#by 唐莺莺
项目介绍：

4399 on VSCode 是一个基于 Visual Studio Code 的扩展，旨在将 4399 平台上的小游戏带到您的开发环境中。这一扩展允许开发者在编码时通过玩小游戏来放松心情，并提高工作效率。以下是其主要功能：

核心功能
1.支持 Flash 和 H5 游戏：
能够运行 4399 平台上的 Flash 游戏（通过本地服务器模拟支持）。
支持 H5 网页小游戏，无需额外插件。

2.内嵌游戏窗口：
在 VSCode 内使用 Webview 面板直接加载游戏，方便快捷。

3.游戏收藏功能：
能够收藏常玩的小游戏，并快速访问。

4.历史记录：
自动记录游戏历史，方便用户回顾之前玩的游戏。

5.智能解析 4399 游戏页面：
自动从 4399 的游戏详情页提取游戏资源地址。
支持复杂的游戏资源解析规则，包括 Flash 和 H5 页游。

<!-- by 唐莺莺  -->

#by 唐莺莺
玩 h5 页游、H5 小游戏和部分 flash 小游戏。

游戏类型特征：
1、H5页游（网页游戏）
需要4399账号授权
通过iframe或专用API加载
典型URL：https://www.4399.com/flash/游戏ID.htm
2、H5小游戏
纯HTML5实现
无Flash依赖
路径包含.html/.htm
3、Flash小游戏
使用Ruffle模拟器
路径以.swf结尾

使用教程
步骤 
1、准备游戏链接
找到对应的 4399 游戏详情页链接，例如：
(1).H5 游戏链接：https://www.4399.com/h5game/123456.htm
(2).Flash 游戏链接：https://www.4399.com/flash/654321.htm
2、调用 play 方法
(1).使用项目
3、启动游戏后
(1).游戏会通过 WebView 面板展示。
(2).运行时会记录游戏历史。

使用示例：
1、启动 Flash 游戏：play("https://www.4399.com/flash/654321.htm");
2、启动 H5 游戏：play("https://www.4399.com/h5game/123456.htm");

以下是启动小游戏的核心代码逻辑：
/**
 * 启动小游戏
 * @param url 游戏详情页链接
 */
async function play(url: string) {
    if (url.startsWith("//")) url = "https:" + url;
    else if (url.startsWith("/")) url = "https://www.4399.com" + url;

    let res = await httpRequest.get(url, "arraybuffer");
    const $ = cheerio.load(iconv.decode(res.data as Buffer, "gb2312"));
    let $flash = $("iframe#flash22").attr("src");

    if ($flash) {
        let gameUrl = new URL($flash, "https://www.4399.com");
        console.log(`启动 Flash 游戏：${gameUrl}`);
    } else {
        console.log("启动 H5 游戏...");
    }
}


注意事项
1、环境要求：
(1).确保系统支持 TypeScript 和Node.js。
(2).如果需要运行 Flash 游戏，请确保本地支持 Flash。

<!-- by 唐莺莺  -->