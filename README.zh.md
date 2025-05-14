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