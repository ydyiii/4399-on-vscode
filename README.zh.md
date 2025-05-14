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


<!-- by 杜昱蓉 -->

#by  杜昱蓉

**安装步骤**：
(1)安装 VSCode：确保你的系统已安装 Visual Studio Code（VSCode），若未安装，可从官网（https://code.visualstudio.com/）下载并安装。
(2)准备开发环境：
安装 Node.js（建议版本 14.x 及以上）。
安装 npm（Node.js 包管理器，通常随 Node.js 一起安装）。

**部署步骤**:
(1)注册 4399 账号：若没有 4399 账号，需先在官网（https://www.4399.com）注册。
(2)获取 Cookie（可选）：
打开浏览器，登录 4399 账号。
访问 4399 任意页面，按 F12 打开开发者工具，切换到 Application（Chrome）或 Storage（Firefox）选项卡。
在 Cookies 中找到 4399 相关的 Cookie，复制完整的 Cookie 信息备用。
(3)VSCode 中配置扩展：
安装扩展后，在 VSCode 中按Ctrl+Shift+P（Windows/Linux）或Cmd+Shift+P（Mac）打开命令面板。
输入 “4399”，可看到扩展提供的命令，如 “4399: 登录”“4399: 签到” 等。
首次使用需先登录，可选择账号密码登录或使用之前获取的 Cookie 登录。

**注意事项**：
(1)网站结构变化：
该扩展通过爬取 4399 网站内容实现功能，若 4399 网站的结构发生变化，可能导致部分功能失效。请留意扩展开发者在 GitHub 仓库（https://github.com/dsy4567/4399-on-vscode）发布的更新信息。
(2)账号安全：
在使用扩展过程中，需遵守 4399 网站的相关规定，避免过度请求，防止账号因异常操作而出现问题。同时，不要将端口（如本地服务器端口）的可见性设为 Public（公共），以免导致 Cookie 泄露，造成账号安全风险。
(3)远程开发环境：
如果在 GitHub CodeSpaces 或其他远程开发环境中使用该扩展，可能会遇到游戏无法加载或图裂的情况。此时，需要按照扩展的提示完成验证（点击 “去验证” 按钮，访问指定链接），并且不要将端口设为公共可见。
(4)快捷键使用：
为使快捷键能够正常使用，在使用快捷键前需要使游戏失去焦点。如果不希望每次都显示该提示，可以在扩展的配置中关闭 “alert” 选项（默认开启）。
(5)配置管理：
扩展提供了一些配置选项，可以通过getCfg、rmCfg、setCfg函数来获取、移除和设置配置。在进行配置更改时，需谨慎操作，以免影响扩展的正常使用。。




#by 蓝早

项目名称：4399 on VSCode
项目使用说明
一. 功能概述
“4399 on VSCode”是一个扩展，允许开发者在VSCode中直接访问和运行4399小游戏。主要功能包括：
1. 游戏ID手动输入启动。
2. 游戏分类浏览。
3. 随机游戏推荐。
4. 游戏搜索功能。
5. 历史记录管理。
6. 游戏详情查看。

二. 安装指南
安装前提
确保已安装最新版 Visual Studio Code (1.70.0+)。
-网络连接正常（需访问4399域名）。

 安装方法
方法1：VSCode内安装（推荐）
1. 按`Ctrl+Shift+X`（Windows/Linux）或`Cmd+Shift+X`（Mac）打开扩展面板。
2. 搜索“4399 on VSCode”。
3. 点击安装按钮。

 方法2：手动安装
1. 下载`.vsix`文件。
2. 打开终端或命令提示符。
3. 运行以下命令安装扩展：
   ```bash
   code --install-extension 4399-on-vscode-1.0.0.vsix
   ```

三. 快速开始
通过游戏ID启动
1. 按`Ctrl+Shift+P`（Windows/Linux）或`Cmd+Shift+P`（Mac）打开命令面板。
2. 输入`4399 get`并回车。
3. 输入游戏ID（如`185252`）或完整URL（如[https://www.4399.com/flash/185252.htm](https://www.4399.com/flash/185252.htm)）。

 随机游戏模式
1. 按`Ctrl+Shift+P`（Windows/Linux）或`Cmd+Shift+P`（Mac）。
2. 输入`4399 随机`并回车。

 按分类浏览游戏
1. 按`Ctrl+Shift+P`（Windows/Linux）或`Cmd+Shift+P`（Mac）。
2. 输入`4399 分类`。
3. 使用方向键选择分类（如“动作”“益智”）。
4. 选择具体游戏后按回车启动。

四. 核心功能使用说明
1. 手动输入游戏ID
使用场景：已知游戏ID时快速启动。
操作步骤：
  1. 打开命令面板（`Ctrl+Shift+P`或`Cmd+Shift+P`）。
  2. 输入命令：
      Flash游戏：`4399: 输入游戏ID`。
      H5游戏：`4399: 输入H5游戏ID`。
  3. 输入游戏ID（如`222735`）或完整URL（如[https://www.4399.com/flash/222735.htm](https://www.4399.com/flash/222735.htm)）。
  4. 按回车确认。
 示例：
    ```typescript
    vscode.commands.executeCommand('4399-on-vscode.get', '222735');
    ```

2. 游戏分类浏览
操作步骤：
  1. 打开命令面板。
  2. 输入`4399: 分类浏览`。
  3. 使用方向键选择分类（如“动作”“益智”）。
  4. 选择具体游戏后按回车启动。

 3. 随机游戏
命令：`4399: 试试手气`。

 4. 游戏搜索
操作步骤：
  1. 打开命令面板。
  2. 输入`4399: 搜索游戏`。
  3. 输入关键词（如“黄金矿工”）。
  4. 从搜索结果中选择游戏。

四. 高级功能
 1. 历史记录管理
查看历史：
 命令：`4399: 历史记录`。
 显示最近玩过的10个游戏。
 清除历史**：
  ```javascript
  vscode.workspace.getConfiguration('4399').update('history', []);
  ```

 2. 自定义配置
 描述：在`settings.json`中添加自定义配置。
示例配置：
  ```json
  {
      "4399.defaultGameId": "222735",
      "4399.enableH5": true,
      "4399.historyLimit": 15,
      "4399.previewQuality": "high"
  }
  ```

五. 常见问题处理
 1. 游戏无法加载
解决方案：
检查网络连接。
确认游戏ID是否正确。
尝试刷新页面（`F5`）。
 2. 输入无效ID
错误提示：无效的游戏ID格式，请输入数字或完整的4399游戏URL。
 3. 性能优化
解决方案：降低预览质量以提升性能。
  ```json
  "4399.previewQuality": "low"
  ```
六. 开发接口
 扩展API
```typescript
interface GameAPI {
    launch(gameId: string): Promise<void>;
    search(keyword: string): Promise<GameInfo[]>;
    getCategories(): Promise<Category[]>;
}
// 获取API实例
const api = vscode.extensions.getExtension('dsy4567.4399-on-vscode').exports;
```
七.使用4399 on VSCode的注意事项
1.Flash 游戏兼容性：依赖 Ruffle 模拟器，部分游戏可能有兼容性问题。如显示方向不对，可尝试让游戏界面变宽。
2.不支持的游戏类型：不支持 Unity3D 游戏和新 Flash 游戏。
3.网络环境：未登录或使用国外 IP 可能导致问题。
4.多开游戏限制：一个游戏未加载完成前，不能多开另一个游戏（页游除外）。
5.快捷键失效：游戏界面获取焦点时，VSCode 快捷键可能失效。
6.鼠标锁定问题：游戏无法锁定鼠标（如射击类游戏）。
7.声音问题：VSCode 版本低于 1.71.0 的用户可能需替换 ffmpeg 才能有声音。

常见问题及解决方案：
1.游戏无法加载：检查网络连接，确认游戏 ID 正确，尝试刷新页面（F5）。
2.输入无效 ID：确保输入纯数字 ID 或完整 URL。
3.性能优化：降低预览质量以提升性能，配置示例：
"4399.previewQuality": "low"
5.扩展安装失败：确保网络正常，检查 VSCode 版本是否兼容。
6.插件冲突：禁用可能冲突的其他扩展。
7.快捷键失效：将焦点移回编辑器。
8.声音问题：检查 VSCode 版本，如低于 1.71.0，尝试替换 ffmpeg。

八. 卸载方法
1. 打开扩展面板（`Ctrl+Shift+X`或`Cmd+Shift+X`）。
2. 找到“4399 on VSCode”。
3. 点击卸载图标。
4. 重启VSCode。




#by 戴颖颖
功能特点：
1.游戏启动
(1)多平台支持：兼容 Flash 和 HTML5 游戏。
(2)功能集成：集成搜索、分类、推荐功能。
2.用户交互
(1)评论系统：支持查看、点赞、回复评论。
(2)历史记录：自动记录游戏历史。
3.社区互动
(1)论坛浏览：支持浏览帖子及查看详情。
(2)群组管理：支持加入/离开群组，支持群组签到。
4.个性化设置
(1)自定义配置：支持自定义脚本。
(2)主题定制：支持自定义主题和样式。
5.便捷功能
(1)一键启动：支持浏览器集成。
(2)自动签到：提供自动签到功能。
6.安全性
(1)数据加密：支持内容过滤。
7.扩展性
(1)插件与API：支持插件机制，提供丰富API。


#by 戴颖颖
推荐和分类
使用教程
步骤 1：启动功能
命令面板：
(1).在 VSCode 中按下 `Ctrl+Shift+P`。
(2).输入命令：
    ```plaintext
    4399-on-vscode.recommended（推荐）
    4399-on-vscode.category（分类）
    ```
    或鼠标点击推荐或分类。

步骤 2：推荐功能界面
(1) 界面元素：
列表形式展示推荐游戏名称（如《黄金矿工》《赛尔号》等）。
```javascript
// 代码实现：从 4399 首页解析推荐游戏
$("a[href*='/flash/'][href*='.htm']").has("img").each((i, elem) => {
    let gameName = $(elem).text().replaceAll(/ |\n/g, "");
    let href = $(elem).attr("href")?.replaceAll(/ |\n/g, "");
    if(gameName && href) games[gameName] = href;
});
```
(2) 操作：
键盘上下键选择游戏 → 按回车启动或鼠标点击。
```javascript
// 代码实现：显示选择界面并处理选择
const val = await vscode.window.showQuickPick(gameNames);
if(val) play(games[val]);
```

步骤 3：分类功能界面
(1) 一级界面（分类选择）：
显示所有分类（如动作、射击、休闲等）。
```javascript
// 代码实现：解析游戏分类
$("a[href*='/flash_fl/'][href*='.htm'], a[href*='/special/'][href*='.htm']").each((i, elem) => {
    let categoryName = $(elem).text().replaceAll(/ |\n/g, "");
    let href = $(elem).attr("href")?.replaceAll(/ |\n/g, "");
    if(categoryName && href) categories[categoryName] = href;
});
```

(2) 二级界面（游戏列表）：
选择分类后展示该类型下的游戏（如选择“动作”显示《拳皇》《火柴人》等）。
```javascript
// 代码实现：获取分类下的游戏
$("a[href*='/flash/'][href*='.htm']:has(img)").each((i, elem) => {
    let gameName = $(elem).children("img").attr("alt");
    let href = $(elem).attr("href");
    if(gameName && href) games[gameName] = href;
});
```

(3) 操作：
选择分类 → 选择游戏 → 自动加载。
```javascript
// 代码实现：分级选择处理
let category = await vscode.window.showQuickPick(categoryNames);
if(category) {
    let games = await getGamesByCategory(categories[category]);
    let gameName = await vscode.window.showQuickPick(Object.keys(games));
    if(gameName) play(games[gameName]);
}
```



#by 蓝早

一、功能名称与简介
功能名称：手动输入游戏ID  
简介：在VSCode扩展“4399 on VSCode”中，用户可通过手动输入4399小游戏的唯一ID，快速定位并启动目标游戏。此功能便于用户快速关联4399平台上的特定游戏，支持数据分析、代码调试等操作。  
解决的问题：  
1. 精准访问：输入ID直达指定游戏，无需逐页搜索。  
2. 效率提升：适用于已知游戏ID的教学/测试场景，节省时间。  
3. 开发辅助：方便开发者快速调试或分析特定游戏资源。  

二、功能特点
1. 精准直达：通过唯一游戏ID直接定位目标游戏，避免繁琐搜索，提升访问效率。  
2. **快速操作**：集成VSCode命令面板（`Ctrl+Shift+P`），输入指令即可触发，无需切换界面。  
3. 开发友好：支持快速调试与测试，方便开发者通过ID直接调取游戏资源，优化工作流。  
4. 灵活兼容：适配4399平台API，可扩展性强，未来可接入更多游戏或自定义功能。  
5. 用户便捷：简化操作步骤，适合教学、演示等场景，提升用户体验。  
依赖：扩展的API接口及网络环境。

 三、安装与使用指南
1. 安装扩展 
   确保已安装最新版VSCode (1.70.0+)。  
   通过VSCode扩展市场搜索安装：`4399 on VSCode`。  
   或使用命令行安装：  
     ```bash
     code --install-extension dsy4567.4399-on-vscode
     ```

2. 功能激活  
   扩展安装后，命令自动注册，无需额外配置。  
   查看已注册命令：  
     ```typescript
     vscode.commands.getCommands().then(commands => {
         console.log(commands.filter(cmd => cmd.startsWith('4399-on-vscode')));
     });
     ```

3. 使用步骤
(1) 打开命令面板
按下快捷键 Ctrl+Shift+P（Windows/Linux）或 Cmd+Shift+P（Mac），输入命令：
标准Flash游戏：输入 4399-on-vscode.get
H5网页游戏：输入 4399-on-vscode.get-h5-web-game
(2) 输入游戏ID
在弹出的输入框中输入以下任一种格式：
typescript
// 示例1：直接输入数字ID
222735
// 示例2：输入完整URL
https://www.4399.com/flash/222735.htm 
输入框截图
输入框支持自动补全历史记录（通过globalStorage实现）
(3) 运行游戏
输入后按回车，扩展会调用核心逻辑：
typescript
// 内部处理逻辑
const parsedId = parseId(userInput); // 提取数字ID
const gameUrl = `https://www.4399.com/flash/ ${parsedId}.htm`;
play(gameUrl); // 加载并运行游戏

4. 完整调用代码示例  
   ```typescript
   import * as vscode from 'vscode';

   async function manuallyInputGameId() {
       const gameId = await vscode.window.showInputBox({
           title: '4399游戏ID输入',
           prompt: '输入游戏ID或完整URL',
           value: '222735' // 默认值
       });
       
       if (gameId) {
           vscode.commands.executeCommand('4399-on-vscode.get', gameId);
       }
   }
   manuallyInputGameId();
   ```

四、功能示例与代码解析
1. 功能示例代码 
   ```typescript
   import * as vscode from 'vscode';

   function playGame(gameId: string) {
       const gameUrl = `https://www.4399.com/flash/${gameId}.htm`;
       vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
       // 实际游戏加载逻辑...
   }

   export function activate(context: vscode.ExtensionContext) {
       let disposable = vscode.commands.registerCommand('game.inputId', async () => {
           const gameId = await vscode.window.showInputBox({
               placeHolder: '请输入4399游戏ID（如：222735）',
               prompt: '支持直接输入数字ID或完整游戏URL',
               validateInput: text => {
                   const idPattern = /^(\d+|https?:\/\/www\.4399\.com\/flash\/\d+\.htm)$/;
                   return idPattern.test(text) ? null : "请输入有效的游戏ID或URL";
               }
           });

           if (gameId) {
               const extractedId = gameId.match(/\d+/)?.[0];
               if (extractedId) {
                   playGame(extractedId);
               } else {
                   vscode.window.showErrorMessage('无效的游戏ID格式');
               }
           }
       });

       context.subscriptions.push(disposable);
   }
   ```

2. 代码解析
   功能入口：通过`vscode.commands.registerCommand`注册命令，用户可在VSCode命令面板中调用。  
   获取历史输入值：从全局存储中获取上次输入的游戏ID，提供默认值。  
   弹出输入框：调用`vscode.window.showInputBox`，用户输入游戏ID或完整URL。  
   处理用户输入：验证输入格式，提取纯数字ID，拼接游戏链接并启动游戏。

（1）. 功能入口
输入游戏 ID (链接以 http(s)://www.4399.com/flash/ 开头) */ get: () => {
这是一个名为get的函数，用于处理用户手动输入游戏ID的操作，专门针对以http(s)://www.4399.com/flash/开头的游戏链接。
它是commands对象中的一个属性，通过vscode.commands.registerCommand注册为一个VSCode命令，用户可以在VSCode中通过命令面板调用该功能。
（2）. 获取历史输入值
let i = globalStorage(ctx).get("id1");
从全局存储中获取键为id1的值，该值存储了用户上次输入的游戏ID。
如果用户之前使用过该功能并输入过游戏ID，这里会获取到上次输入的值，以便在输入框中为用户提供一个默认值，方便用户快速操作。
（3）. 弹出输入框
vscode.window
    .showInputBox({
        value: i ? String(i) : "222735",
        title: "4399 on VSCode: 输入游戏 ID",
        prompt: "输入游戏链接或 http(s)://www.4399.com/flash/ 后面的数字(游戏 ID)",
    })
调用vscode.window.showInputBox方法弹出一个输入框，让用户可以手动输入游戏ID。
（4）输入框的配置如下：
value：输入框的初始值。如果从全局存储中获取到了历史输入值i，则将其作为初始值；否则，默认值为"222735"。
title：输入框的标题，显示为"4399 on VSCode: 输入游戏 ID"，明确告知用户当前操作的功能。
prompt：输入框的提示信息，告诉用户可以输入游戏链接，或者输入http(s)://www.4399.com/flash/后面的数字（游戏ID），为用户提供明确的输入指引。
（5）. 处理用户输入
.then(id => {
    if (id) {
        log("用户输入 ", id);
        globalStorage(ctx).set("id1", id);
        play(
            "https://www.4399.com/flash/ </url> " + parseId(id) + ".htm"
        );
    }
});

五、扩展功能
1. 快速定位特定游戏：输入游戏ID快速定位特定游戏，节省时间。  
2. 结合历史记录功能：输入游戏ID后，游戏自动添加到历史记录中，方便后续快速访问。  
3. 搭配自定义HTML代码片段：为特定游戏注入自定义HTML代码，优化游戏体验。  
4. 支持多类型游戏：支持H5页游、H5小游戏和部分Flash小游戏（借助Ruffle模拟器）。  
5. 辅助账号登录与管理：结合登录账号功能，快速切换账号，继续游戏进度。  
6. 助力游戏开发与测试：帮助开发者快速定位正在开发或测试的游戏，便于调试和性能分析。




#by 黄恋涵
# 🎮 4399 on VSCode - 🕒历史记录功能

本项目实现了游戏历史记录功能，允许用户🔍查看和⚡选择之前启动的游戏记录，同时支持🗑️清空历史记录，方便用户🚀快速找回和🔄重新游玩。历史记录功能的核心代码分布在 `game.ts` 和 `index.d.ts` 文件中，通过 VSCode 的全局存储（globalStorage）来保存历史记录数据。


## 📦数据结构

历史记录的数据结构定义在 `index.d.ts` 📄文件中，使用 `game.History` 类型表示，包含以下字段：

- `name`🕹️：游戏名称

- `url`🔗：游戏的 URL

- `date`📅：游戏启动的时间戳

- `webGame`📤：布尔值，表示是否为网页游戏

```bash
  export namespace game {
    export type History = {
        date: string;
        webGame: boolean;
        name: string;
        url: string;
    };
  }
```


## 🔧功能实现

#### 1. 📜**显示历史记录**

在 `game.ts` 文件中，`showHistory` 函数负责显示历史记录列表。它📂从全局存储中读取历史记录数据，将其🔄转换为 QuickPick 列表项，并提供用户交互界面。用户可以选择某个历史记录来重新启动游戏，或者选择🗑️“清空历史记录”选项来删除所有记录。


```javascript
async function showHistory() {
    try {
        let h: game.History[] = globalStorage(getContext()).get("history");
        if (!h || (typeof h === "object" && !h[0])) h = [];

        h.unshift({
            webGame: false,
            name: "🧹 清空历史记录",
            url: "",
            date: "",
        });

        let quickPickList: string[] = [];
        h.forEach(obj => {
            quickPickList.push(obj.name + obj.date);
        });
        const gameName = await vscode.window.showQuickPick(quickPickList);
        if (gameName === "🧹 清空历史记录")
            return globalStorage(getContext()).set("history", []);

        if (gameName)
            for (let index = 0; index < h.length; index++) {
                const item = h[index];
                if (item.name + item.date === gameName) {
                    if (item.webGame) playWebGame(item.url);
                    else play(item.url);

                    break;
                }
            }
    } catch (e) {
        err("无法读取历史记录", String(e));
    }
}
```

#### 2. **写入历史记录**💥

每次启动游戏时，`updateHistory` 函数会被调用，将新的历史记录添加到全局存储中。历史记录以数组形式存储，📝新的记录会被插入到数组的开头。

```javascript
function updateHistory(history: game.History) {
    if (!getCfg("updateHistory", true)) return;

    let h: game.History[] = globalStorage(getContext()).get("history");
    if (!h || (typeof h === "object" && !h[0])) h = [];

    h.unshift(history);
    globalStorage(getContext()).set("history", h);
}
```

在 `play` 和 `playWebGame` 函数中，会根据游戏的类型调用 `updateHistory` 函数，并传入相应的参数：

```javascript
try {
    let D = new Date();
    updateHistory({
        date: ` (${D.getFullYear()}年${
            D.getMonth() + 1
        }月${D.getDate()}日${D.getHours()}时${D.getMinutes()}分)`,
        name: title,
        webGame: false,
        url: url,
    });
} catch (e) {
    err("写入历史记录失败", String(e));
}

try {
    let D = new Date();
    updateHistory({
        date: ` (${D.getFullYear()}年${
            D.getMonth() + 1
        }月${D.getDate()}日${D.getHours()}时${D.getMinutes()}分)`,
        name: title,
        webGame: true,
        url: url,
    });
} catch (e) {
    err("写入历史记录失败", String(e));
}
```
## 🌟主要功能
历史记录功能为用户提供了以下核心功能：
- **自动记录**📝：每次启动游戏时，系统会自动记录游戏的名称、URL、启动时间和游戏类型等信息。

- **快速访问**🚀：通过 QuickPick 列表，用户可以快速浏览历史记录，并选择某条记录重新启动游戏。

- **清理功能**🧹：用户可以选择“清空历史记录”选项，一键删除所有历史记录，保持记录的整洁。

- **配置控制**⚙️：用户可以通过配置项灵活控制是否启用历史记录功能，以满足不同的使用需求。
## ✨功能特点

1.**数据存储💾：**
- 使用 VSCode 的 globalStorage 存储历史记录，确保数据在插件重启后依然可用。
- 历史记录🔍以数组形式存储，便于管理和查询。

2.**用户交互🏷️：**
- 提供 QuickPick 列表界面，用户可以通过名称和日期快速查找历史记录。
- 提供“清空历史记录”选项，方便用户🗑️清理不再需要的记录。
3.**配置选项⚙️：**
- 用户可以通过 `updateHistory` 配置项控制是否记录历史记录。
4.**扩展性🧰：**
- 历史记录功能的实现逻辑清晰，便于后续扩展，例如支持更多游戏类型或增加更多筛选条件。


## 📌使用方法

#### 1.启动游戏🤖：
- 每次启动游戏时，系统会自动调用 `updateHistory` 函数，记录当前游戏的名称、URL、日期和类型。
#### 2.查看历史记录🔍：
- 按`Ctrl+Shift+P`，输入"4399 on VSCode：历史记录"，🖱️通过命令调用 `showHistory` 函数，显示历史记录列表，用户可以选择某个历史记录重新启动游戏。
#### 3.清空历史记录⏎：
- 打开历史记录列表，在列表顶部选择"🧹 清空历史记录"
## 💾存储位置

历史记录🔒存储在 VSCode 的全局状态中，位于：

```bash
  ${userHome}/.vscode/extensions/.../globalStorage/state.vscdb
```


## 配置选项🎯

可以通过 VSCode 设置控制历史记录功能：

```bash
{
  "4399-on-vscode.updateHistory": true  // 是否启用历史记录功能，默认为true
}
```
## 🛠️核心 API 接口

#### 1️⃣历史记录管理接口 
`getHistoryList()`

```bash
/**
 * 获取完整历史记录列表
 * @returns Promise<GameHistory[]> 返回历史记录数组
 */
function getHistoryList(): Promise<GameHistory[]>
```

`addHistory(record)`

```bash
/**
 * 添加新的历史记录
 * @param record 历史记录对象
 * @param record.name 游戏名称
 * @param record.url 游戏URL
 * @param record.type 游戏类型('flash'|'h5')
 * @returns Promise<boolean> 是否添加成功
 */
function addHistory(record: {
  name: string;
  url: string;
  type: 'flash' | 'h5';
}): Promise<boolean>
```
`clearHistory()`
```bash
/**
 * 清空所有历史记录
 * @returns Promise<boolean> 是否清空成功
 */
function clearHistory(): Promise<boolean>
```
#### 2️⃣ 历史记录查询接口
`getRecentHistory(limit)`
```bash
/**
 * 获取最近的历史记录
 * @param limit 获取条数 (默认10条)
 * @returns Promise<GameHistory[]> 
 */
function getRecentHistory(limit?: number): Promise<GameHistory[]>
```
`searchHistory(keyword)`
```bash
/**
 * 搜索历史记录
 * @param keyword 搜索关键词
 * @returns Promise<GameHistory[]>
 */
function searchHistory(keyword: string): Promise<GameHistory[]>
```
#### 3️⃣ 历史记录配置接口
`getHistoryConfig()`
```bash
/**
 * 获取历史记录配置
 * @returns Promise<HistoryConfig>
 */
function getHistoryConfig(): Promise<HistoryConfig>
```
`updateHistoryConfig(config)`

```bash
/**
 * 更新历史记录配置
 * @param config 配置对象
 * @param config.maxRecords 最大记录数
 * @param config.autoCleanDays 自动清理天数
 * @param config.enabled 是否启用
 * @returns Promise<boolean>
 */
function updateHistoryConfig(config: {
  maxRecords?: number;
  autoCleanDays?: number;
  enabled?: boolean;
}): Promise<boolean>
```

#### 📊数据类型定义
GameHistory 类型
```bash
  interface GameHistory {
  id: string;           // 记录ID
  name: string;         // 游戏名称
  url: string;          // 游戏URL
  type: 'flash' | 'h5'; // 游戏类型
  playTime: Date;       // 游玩时间
  lastPlayed?: Date;    // 最后游玩时间(可选)
  playCount?: number;   // 游玩次数(可选)
  favorite?: boolean;   // 是否收藏(可选)
}
```
HistoryConfig 类型
```bash
  interface HistoryConfig {
  maxRecords: number;   // 最大记录数(默认100)
  autoCleanDays: number; // 自动清理天数(0表示不自动清理)
  enabled: boolean;     // 是否启用历史记录
}
```
#### 💻使用示例
🔹 基本使用
```bash
// 添加历史记录
await addHistory({
  name: '黄金矿工',
  url: 'https://www.4399.com/flash/12345.htm',
  type: 'flash'
});

// 获取最近10条记录
const recentGames = await getRecentHistory(10);

// 搜索游戏
const results = await searchHistory('矿工');
```
🔹 高级配置
```bash
// 更新配置
await updateHistoryConfig({
  maxRecords: 200,
  autoCleanDays: 30
});

// 获取当前配置
const config = await getHistoryConfig();
console.log(config);
```
#### ❌错误处理
所有API都可能抛出以下错误：

- `HistoryDisabledError`: 历史记录功能被禁用时抛出

- `StorageLimitExceededError`: 存储达到上限时抛出

- `InvalidRecordError`: 无效的历史记录数据时抛出






## ⚠️注意事项

#### 1. 数据隐私与安全
- 📌 所有记录（含游戏名称、完整URL）默认仅**存储于本地**，不会自动同步云端

- 🔒 敏感游戏建议：

  手动删除单条记录（右键记录项）
  
  使用「隐私模式」临时禁用记录功能

- ⚠️ 公共电脑使用时，退出前建议执行「清空历史记录」
#### 2. 存储管理
- 💾 **自动维护**：

  默认保留最近100条（可设置增至200条）

  超过30天的记录自动清理（可调整周期）

- 🚨 风险提示：
    
     超过500条可能影响加载速度

     卸载扩展/重装VSCode将**永久丢失**历史数据

- ✅ 建议操作：

  重要游戏点击「⭐收藏」永久保存

  定期通过「导出为JSON」备份数据

#### 3. 使用准确性
- 🔍 记录内容包含：

  精确到分钟的启动时间（北京时间）

  游戏类型标识（Flash/H5）

  原始游戏链接

- ❗ 注意：

  失效链接会保留但显示⚠️图标

  同名不同版本游戏会生成独立记录

#### 4. 功能配置
- ⚙️ 关键设置项：
```http
{
  "4399-on-vscode.history.maxRecords": 100,
  "4399-on-vscode.history.retentionDays": 30,
  "4399-on-vscode.history.enabled": true
}
```

- 🔄 配置生效条件：

  数量限制即时生效

  启用/禁用需**重启VSCode**

#### 5. 异常处理
- 🔧 常见问题：
| 现象 | 解决方案     |                     
| :-------- | :------- |
| 记录不更新      | 检查是否禁用功能、确认存储权限 |
| 链接失效      | 手动删除或等待自动清理 |
| 加载卡顿      | 清理早期记录或增加maxRecords |
	
- 🛠️ 高级修复：

命令面板运行 `4399: Repair History`

从备份文件恢复数据

#### 6. 特殊场景
- 🌐 **不支持的环境**：

  VSCode网页版

  部分企业网络（可能拦截游戏URL）

- ⏳ **数据保留**：

  即使游戏下架，历史链接仍会保留

  收藏项不受自动清理影响



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



by 张晓雯
### 1. 功能名称与简介
**功能名称**：4399 on VSCode - 逛群组功能

**功能简介**：“4399 on VSCode - 逛群组功能” 集成于 VSCode 扩展 “4399 on VSCode” 中，允许用户在 VSCode 开发环境内搜索、浏览和参与 4399 游戏相关的群组。用户可搜索感兴趣的群组，查看群组内的帖子，还能进行加入、离开群组以及群组签到等操作，为 4399 游戏爱好者提供便捷的交流途径。

### 2. 功能特点

#### 2.1 群组搜索与帖子查看
**功能描述**：
用户能在输入框输入关键词搜索 4399 的游戏群组，搜索结果以列表形式展示群组标题和 ID，且支持分页搜索。点击搜索结果中的群组，可进入该群组查看所有帖子，帖子列表会展示帖子标题、类型和 ID，点击帖子即可查看帖子详情。

**操作步骤**：
```typescript
// 步骤 1: 打开 “4399 on VSCode: 逛群组” 输入框
import { showForums } from './src/forums';
// 调用 showForums 函数打开逛群组输入框
showForums();

// 步骤 2: 在输入框中输入关键词，系统会延迟 1 秒发起请求获取搜索结果
import { AxiosResponse } from "axios";
import * as cheerio from "cheerio";
import * as iconv from "iconv-lite";
import * as vscode from "vscode";
import {
    API_URLS,
    createQuickPick,
    err,
    getContext,
    globalStorage,
    httpRequest,
    log
} from "./utils";

let threadQp: vscode.QuickPick<any>;
let threadQpItems: any[] = [];
let threadPage: number = 1;
let threadTimeout: NodeJS.Timeout;

/**
 * 搜索群组
 * @param kwd 搜索词
 */
function searchForums(kwd: string) {
    if (threadQp.busy) return;

    clearTimeout(threadTimeout);
    log(`页码: ${threadPage}`);
    threadTimeout = setTimeout(async () => {
        threadQp.busy = true;
        let res;
        try {
            res = (await httpRequest.get(
                API_URLS.search(kwd),
                "arraybuffer"
            )) as AxiosResponse<Buffer | string>;
        } catch (e) {
            return err("获取搜索建议失败", String(e));
        }
        if (!res.data) return err("获取搜索建议失败");

        res.data = iconv.decode(res.data as Buffer, "utf8");
        const d: string = res.data;
        const $ = cheerio.load(d);
        threadQpItems = [];

        $("ul > li > a > span.title").each((i, elem) => {
            let g = $(elem).text();
            let id: string | undefined | number = $(elem)
               .parent()
               .attr("href")
               ?.split("-")
               ?.at(-1);
            if (!id || isNaN(Number(id))) return;

            id = Number(id);
            threadQpItems.push({
                label: g,
                description: `群组 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    // 这里可以添加点击群组后的处理逻辑
                },
                data: {
                    id,
                    title: g
                }
            });
        });

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
            alwaysShow: true,
            action() {
                threadPage++;
                searchForums(threadQp.value);
            }
        });

        if (threadQpItems[0]) threadQp.items = threadQpItems;
        threadQp.busy = false;
    }, 1000);
}

// 初始化输入框
threadQp = createQuickPick({
    title: "4399 on VSCode: 搜索群组",
    prompt: "输入搜索词"
});
threadQp.onDidChangeValue(kwd => {
    searchForums(kwd);
});
threadQp.show();
```

**代码解释**：
- **输入框监听**：通过 `threadQp.onDidChangeValue` 监听输入框内容的变化，当输入框内容改变时，调用 `searchForums` 函数。
- **延迟请求**：在 `searchForums` 函数中，使用 `setTimeout` 延迟 1 秒发起请求，同时使用 `clearTimeout` 清除之前的定时器，以避免多次请求。
- **请求处理**：使用 `httpRequest.get` 发起请求，获取搜索结果，并使用 `cheerio` 解析 HTML 内容。
- **结果展示**：将搜索结果添加到 `threadQpItems` 中，并更新输入框的 `items` 属性。

```typescript
// 步骤 3: 浏览搜索结果，点击感兴趣的群组，进入该群组的帖子列表页面

// 步骤 4: 在帖子列表页面，浏览帖子标题和类型，点击帖子可查看帖子详情
/**
 * 显示所有帖子
 * @param id 群组 ID
 * @param title 群组名
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`群组 ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // 获取标题和类型
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[顶] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `帖子 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("帖子 ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `群组: ${title}`;
        }
        showingThreads = true;

        // 这里可以添加后续处理逻辑
    } else {
        err("无法获取群组页面");
    }
}
```

**代码解释**：
- **获取帖子详情数据**：通过 `httpRequest.get` 方法请求指定帖子的详情页面，使用 `cheerio` 解析 HTML 内容。
- **预处理 HTML**：对 HTML 中的图片链接进行处理，修改协议和解除防盗链限制，并移除一些不必要的元素。
- **提取从帖内容**：遍历 HTML 中的从帖元素，提取从帖的作者、标题和正文，并将这些内容拼接成 HTML 字符串。
- **生成文章 HTML**：将主帖和从帖的内容拼接成完整的文章 HTML，并进行一些替换操作。
- **显示帖子详情**：使用 `vscode.window.createWebviewPanel` 创建一个 Webview 面板，将生成的文章 HTML 显示在面板中。

#### 2.2 群组操作（加入 / 离开群组、签到）
**功能描述**：在群组帖子列表页面，用户可根据自身需求选择加入或离开群组，还能进行群组签到操作。加入或离开群组以及签到操作会向服务器发送请求，并根据返回结果提示用户操作是否成功。

```typescript
/**
 * 显示所有帖子
 * @param id 群组 ID
 * @param title 群组名
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`群组 ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // 获取标题和类型
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[顶] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `帖子 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("帖子 ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `群组: ${title}`;
        }
        showingThreads = true;
        threadQp.buttons = [
            vscode.QuickInputButtons.Back,
            {
                tooltip: joined ? "离开群组" : "加入群组",
                iconPath: joined
                   ? new vscode.ThemeIcon("remove")
                    : new vscode.ThemeIcon("add"),
                async action(button) {
                    let result: any;
                    switch (button.tooltip) {
                        case "离开群组":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.leave(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code !== 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "操作失败"
                                );
                            }
                            break;
                        case "加入群组":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.join(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code === 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "加入成功"
                                );
                            } else {
                                vscode.window.showInformationMessage(
                                    result?.msg || "操作失败"
                                );
                            }
                            break;
                    }
                    showThreads(forumId, forumTitle);
                },
            },
            {
                tooltip: "签到",
                iconPath: new vscode.ThemeIcon("check"),
                async action() {
                    const result = (
                        await httpRequest.post(
                            ...API_URLS.sign(forumId),
                            "json"
                        )
                    ).data;
                    if (result?.code === 100) {
                        vscode.window.showInformationMessage(
                            result?.msg ||
                            `签到成功, 已连续签到 ${+result?.result?.totalDays} 天`
                        );
                    } else {
                        vscode.window.showInformationMessage(
                            result?.msg || "操作失败"
                        );
                    }
                    showThreads(forumId, forumTitle);
                },
            },
        ];
        threadQp.busy = false;
    } else {
        err("无法获取群组页面");
    }
}
```

**代码解释**：
- **获取群组帖子列表**：使用 `httpRequest.get` 方法请求指定群组的帖子列表页面，使用 `cheerio` 解析 HTML 内容。遍历 HTML 中的帖子元素，提取帖子的标题、类型和 ID，并将这些信息存储在 `threadQpItems` 数组中。
- **判断是否已加入群组**：通过检查 HTML 中的 `a.join` 元素是否包含 `hasjoin` 类来判断用户是否已加入群组。
- **添加操作按钮**：在 `threadQp.buttons` 中添加了三个按钮：返回按钮、加入 / 离开群组按钮和签到按钮。
- **加入或离开群组操作**：当用户点击加入 / 离开群组按钮时，根据按钮的 `tooltip` 属性判断用户的操作类型。使用 `httpRequest.post` 方法向服务器发送加入或离开群组的请求，并根据返回结果提示用户操作是否成功。
- **签到操作**：当用户点击签到按钮时，使用 `httpRequest.post` 方法向服务器发送签到请求，并根据返回结果提示用户签到是否成功。
- **更新页面**：在每次操作完成后，调用 `showThreads` 函数更新页面，显示最新的群组信息和帖子列表。

**操作步骤**：
1. 进入群组帖子列表页面。
2. 若当前未加入群组，点击 “加入群组” 按钮，系统会向服务器发送加入请求，根据返回结果提示操作是否成功。
3. 若当前已加入群组，点击 “离开群组” 按钮，系统会向服务器发送离开请求，根据返回结果提示操作是否成功。
4. 点击 “签到” 按钮，系统会向服务器发送签到请求，若签到成功，会显示连续签到天数。

### 3. “逛群组” 安装步骤
1. 确保安装 Node.js 和 npm。
2. 克隆项目：
```bash
git clone https://github.com/dsy4567/4399-on-vscode.git && cd 4399-on-vscode
```
3. 安装依赖：
```bash
npm install
```
4. 编译代码：
```bash
npm run compile
```
5. （可选）配置开发环境。
6. 启动项目：在 VSCode 中按 <kbd>F5</kbd> 或执行相应启动命令。



<!-- by 赵文佳 -->
（一）搜索游戏的核心功能模块的简单介绍以及代码定位

1.  游戏搜索入口（searchGames）

游戏搜索入口简介:初始化搜索交互界面（VSCode QuickPick），接收用户输入的关键词，触发搜索建议或直接执行搜索。支持默认搜索词填充、界面状态管理（如分页按钮显示）和用户操作监听。

代码定位: async function searchGames(s: string)

2. 执行搜索（searchByKwd）

简介:根据关键词调用 4399 搜索接口，解析返回的 HTML 页面，提取游戏名称、ID 等信息，并生成可操作的 QuickPick 列表项（含“下一页”分页功能）。

代码定位: async function searchByKwd(s: string)


3. 联想词建议（suggest）

简介:提供实时搜索联想词功能，通过防抖机制（延迟 1 秒）调用 4399 建议接口，解析 JSON 数据并展示建议列表。用户选择建议词可直接跳转至对应游戏。

代码定位: async function suggest(kwd: string)

4.  API 地址管理（API_URLS）

简介:定义 4399 搜索相关接口的 URL 模板，包括：搜索结果分页接口（result）：支持动态关键词和页码；搜索建议接口（suggestion）：支持动态关键词。

代码定位: const API_URLS 常量。


5. 分页与界面状态管理

简介:管理当前搜索的页码（searchPage）、搜索输入框历史值（searchValue）及界面状态（如是否正在展示结果）。

关键代码定位:

searchPage: number：记录当前页码。

showingSearchResult: boolean：标记是否正在展示搜索结果。

searchValue: string：保存用户输入的搜索词。

6. 异常捕获与日志记录

简介:捕获网络请求、数据解析等异常，通过 err 函数输出错误信息，并通过 log 记录调试信息，保障搜索流程的健壮性。

代码定位:

try...catch 块包裹网络请求。

err("无法获取搜索页: ", e) 和 log("成功获取到4399搜索页面") 调用。

<!-- by 赵文佳 -->
（二）搜索功能模块协作流程图
用户输入 → searchGames() → 触发 suggest()（联想词）或 searchByKwd()（直接搜索）
                │
                ├─→ suggest() → 调用 API_URLS.suggestion → 解析建议词 → 填充 QuickPick
                │
                └─→ searchByKwd() → 调用 API_URLS.result → 解析 HTML → 生成结果列表
                                      │
                                      └─→ 用户点击“下一页” → searchPage++ → 重新调用 searchByKwd()

<!-- by 赵文佳 -->
（三）搜索功能模块的使用教程

一、安装插件
方法一：通过 VSCode 扩展商店安装：
首先打开 VSCode，点击左侧边栏的 Extensions 图标（或按下 Ctrl+Shift+X）；其次搜索 4399 on VSCode，找到插件后点击 Install；
在安装完成后，点击 Reload 重启 VSCode 激活插件。

方法二：手动安装：
从 GitHub 仓库下载 .vsix 文件
'''git clone https://github.com/dsy4567/4399-on-vscode.git
cd 4399-on-vscode
vsce package'''
完成后在 VSCode 中，通过 Extensions 面板右上角菜单选择 Install from VSIX，选择生成的 .vsix 文件。

二、启动搜索功能
打开搜索界面，按下 Ctrl+Shift+P 打开命令面板；输入命令 4399: 搜索游戏 并按回车。此时会弹出 QuickPick 输入框，标题为 4399 on VSCode: 进行搜索。

三、基础操作
1. 输入关键词搜索
在输入框中输入游戏名称（如 黄金矿工）。
直接搜索：按回车键，插件会调用搜索接口并展示结果列表。
实时建议：输入过程中会显示联想词（如 黄金矿工双人版），点击联想建议词可直接跳转至对应游戏。

2. 浏览搜索结果
搜索结果以列表形式展示，包含 游戏名称 和 游戏 ID（如 游戏 ID: 12345）。
点击游戏名称后：插件会自动调用 play 函数，在内嵌浏览器中打开游戏页面；输入框关闭，最后一次搜索词会保存到全局存储中。

3. 翻页功能
列表底部有 下一页 按钮，点击可加载更多搜索结果，当前页码显示在按钮描述中（如 第 1 页），翻页后，输入框标题会更新为 关键词 的搜索结果（第 N 页）。

4. 返回搜索建议
若在搜索结果界面点击左上角的 返回按钮（←），输入框会切换回搜索建议模式，清空当前结果并显示联想词。

四、其余高级功能
1. 历史记录存档
最后一次成功搜索的关键词会通过 globalStorage 自动保存，下次打开搜索界面时，输入框会自动填充该关键词。
2. 错误处理
网络异常：若搜索请求失败，插件会通过 VSCode 通知栏显示错误信息（如 无法获取搜索页: 超时）。


<!-- by 赵文佳 -->
（四）搜索功能特点详解：

一、深度集成 VSCode 特性
 1. 原生搜索交互界面
详细代码：
```typescript
// 使用 VSCode QuickPick 实现类搜索引擎的交互
searchQp = createQuickPick({
  title: "4399 on VSCode: 搜索",
  prompt: "输入搜索词"
});
```
特点：  
1.完全融入 VSCode UI 风格
2.支持键盘快速导航（↑↓方向键选择，Enter确认）
3.内置加载状态指示（`busy` 属性）

2. Webview 游戏容器
详细代码：
```typescript
// 在 VSCode 内嵌浏览器中运行游戏
const panel = vscode.window.createWebviewPanel(
  "4399-game", 
  "4399小游戏", 
  vscode.ViewColumn.One, 
  { enableScripts: true }
);
panel.webview.html = `<iframe src="${url}"></iframe>`;
```
特点：  
1.无需离开编辑器即可玩游戏
2.支持 Flash/HTML5 游戏（取决于浏览器兼容性）
3.多标签页支持（可同时打开多个游戏）


二、智能搜索功能
1. 多级搜索建议
详细代码：
```typescript
// 输入关键词时动态获取建议
suggestions = JSON.parse(decodedData.split(" =")[1].replaceAll("'", '"'));
```
特点：  
1.输入即时反馈（1秒防抖延迟）
2.建议项包含精准游戏 ID
3.支持键盘快速选择建议项

2. 分页搜索系统
详细代码：
```typescript
// 搜索结果分页处理
searchQpItems.push({
  label: "下一页",
  action() { 
    searchPage++;
    searchByKwd(searchQp.value); 
  }
});
```
特点：  
1.无限滚动式分页加载
2.页码状态持久化（`searchPage` 全局变量）
3.智能结果去重（每次搜索重置列表）


三、数据处理核心
1.GB2312 编码自动转换
详细代码：
```typescript
// 处理中文编码兼容性问题
res.data = iconv.decode(res.data as Buffer, "gb2312");
```
特点：  
1.解决 4399 接口返回非 UTF-8 编码问题
2.支持完整中文字符显示


四、用户个性化体验
1. 搜索历史记忆 
详细代码：
```typescript
// 保存最后一次有效搜索词
globalStorage(getContext()).set("kwd", searchQp.value);
```
特点：  
1.使用 VSCode 全局存储（`Memento` API）
2.重启编辑器后仍可恢复上次搜索



<!-- by 杜昱蓉 -->

#by  杜昱蓉

**登陆账号功能**：
**核心功能**：
1.登录认证
(1)支持两种登录方式：账号密码登录和 Cookie 登录。
(2)使用httpRequest.post发送登录请求，并解析响应中的 Cookie 信息。
(3)将用户 Cookie 存储在 VSCode 的secrets存储中，确保安全性。
2.Cookie 管理
(1)getCookie()：异步获取存储的 Cookie。
(2)getCookieSync()：同步获取 Cookie（若未加载则触发异步加载）。
(3)setCookie()：设置并保存 Cookie 到 VSCode 存储。
3.用户信息
getUid()：从 Cookie 中解析用户 ID（Pauth字段）。
4.签到功能
sign()：调用 4399 签到 API，支持静默签到（不显示通知）。
5.个人中心
my()：显示用户个人中心菜单，支持查看收藏、历史记录、推荐游戏等。

**代码结构解析**：
**API接口定义**：
{{
/** 接口地址 **/
const API_URLS = {
    /** 登录 **/
    login: (username: string, password: string) =>
        [
            "https://ptlogin.4399.com/ptlogin/login.do?v=1",  // 登录API的URL
            `username=${username}&password=${password}`,  // 登录表单数据
        ] as const,
    /** 签到 **/
    get sign() {
        // 生成带时间戳的签到URL，防止缓存
        return "https://my.4399.com/plugins/sign/set-t-" + new Date().getTime();
    },
};

let COOKIE: string;  // 全局变量存储当前用户的Cookie
}}

**登录流程**：
1.检查登录状态：通过getCookieSync()判断用户是否已登录。
{{
if (getCookieSync()) {
        if (loginOnly)
            return vscode.window
                .showInformationMessage("是否退出登录?", "是", "否")
                .then(async value => {
                    if (value === "是") {
                        await setCookie();
                        vscode.window.showInformationMessage("退出登录成功");
                    }
                });
        return callback(getCookieSync());
    }
}}
2.选择登录方式：
(1)账号密码登录：发送表单请求到 4399 登录接口，解析响应中的 Cookie。
{{
 const user = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用账号密码)",
                prompt: "请输入 4399 账号",
            });}}
(2)Cookie 登录：直接使用用户提供的 Cookie 字符串。
            {{
            let c = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用 cookie)",
                prompt: "请输入 cookie, 获取方法请见 GitHub wiki 页",
            });}}
3.验证与存储：确保 Cookie 包含必要的Pauth字段，然后加密存储。 

**签到流程**:
1.调用login()确保用户已登录。
 login(async () => {  // 确保用户已登录
2.发送签到请求到API_URLS.sign。
// 发送签到请求
            {{(
            const data: {
                code?: number;
                result?:
                    | null
                    | string
                    | {
                          days?: number;
                          credit?: number;
                      };
                msg?: string;
            } = (await httpRequest.get(API_URLS.sign, "json")).data;)}}
3.解析响应结果（成功 / 失败 / 连续签到天数）。
{{
if (data.result === null) err("签到失败, 其他错误: " + data.msg);
            else if (typeof data.result === "string")  // 如果返回消息文本
                !quiet && vscode.window.showInformationMessage(data.result);
            else if (typeof data.result === "object")  // 如果返回对象(包含连续签到天数)
                !quiet &&
                    vscode.window.showInformationMessage(
                        `签到成功, 您已连续签到${data.result.days}天`
                    );
            else err("签到失败, 返回数据非法");
        } catch (e) {
            err("签到失败: ", String(e));
        }}}
        
        
个人中心流程:
1.显示用户昵称（从 Cookie 中解析Pnick字段）。
{{{
// 从Cookie中解析用户昵称
        let Pnick = cookie.parse(c)["Pnick"] || "未知";
        Pnick = Pnick === "0" ? "未知" : Pnick;
}}}
2.提供多个功能选项：
(1)我的收藏：获取并展示用户收藏的游戏列表。
(2)猜你喜欢：获取推荐游戏。
(3)我玩过的：获取历史记录。
(4)签到：触发签到功能。
(5)退出登录：清除 Cookie 并登出。
{{
// 显示个人中心菜单
        const value = await vscode.window.showQuickPick([
            "🆔 昵称: " + Pnick,
            "❤️ 我的收藏盒",
            "✨ 猜你喜欢",
            "🕒 我玩过的",
            "🖊 签到",
            "🚪 退出登录",
        ]);
    }}
关键技术细节:
1.字符编码处理
使用iconv-lite解码 GBK 编码的响应内容（4399 网站使用 GBK 编码）。
2.错误处理
(1)通过err()函数显示错误信息，并记录日志。
(2)处理网络请求失败、Cookie 无效等异常情况。
3.异步操作
使用Promise处理异步请求，确保代码流程正确。
4.安全措施
(1)使用 VSCode 的secrets存储敏感信息（Cookie）。
(2)对用户输入进行验证（如检查 Cookie 是否包含Pauth字段）