#by 蓝早
一. 功能名称与简介
功能名称：手动输入游戏ID
简介：在VSCode扩展"4399 on VSCode"中，手动输入游戏ID功能允许用户直接通过输入4399小游戏的唯一ID，快速定位并启动目标游戏。此功能允许用户通过手动输入的方式，快速定位并关联到4399平台上的特定游戏，从而实现对该游戏的进一步操作，如数据分析、代码调试等。
该功能解决了以下问题：
（1）精准访问：无需在平台内逐页搜索，输入ID即可直达指定游戏
（2）效率提升：适用于已知游戏ID的教学/测试场景，节省浏览时间
（3）开发辅助：方便开发者快速调试或分析特定游戏资源

二. 手动输入游戏ID功能特点
（1）. 精准直达* 
    通过唯一游戏ID直接定位目标游戏，避免平台内繁琐搜索，提升访问效率。  
（2）. 快速操作 
   -集成VSCode命令面板（`Ctrl+Shift+P`），输入指令即可触发，无需切换界面。  
（3）. 开发友好
   支持快速调试与测试，方便开发者通过ID直接调取游戏资源，优化工作流。  
（4）. 灵活兼容* 
   适配4399平台API，可扩展性强，未来可接入更多游戏或自定义功能。  
（5）. 用户便捷 
   简化操作步骤，适合教学、演示等场景，提升用户体验。  
（功能需依赖扩展的API接口及网络环境）
4399 on VSCode中的手动输入游戏ID功能，以其高效精准的定位能力、高度的操作灵活性、实时的校验反馈、易用性强、适应性强以及安全可靠等特点，为用户提供了便捷、高效的游戏开发和调试体验，是4399 on VSCode工具中不可或缺的重要功能之一。


三. 手动输入游戏ID功能 - 安装与使用指南
1. 安装扩展
确保已安装 4399 on VSCode 扩展：
bash
# 通过VSCode扩展市场搜索安装
code --install-extension dsy4567.4399-on-vscode
或直接在VSCode扩展面板搜索 4399 on VSCode 并安装。
2. 功能激活
扩展安装后，在VSCode的 activate 函数中已自动注册命令，无需额外配置。
可通过以下代码查看已注册命令：

typescript
// 查看已注册命令（调试用）
vscode.commands.getCommands().then(commands => {
    console.log(commands.filter(cmd => cmd.startsWith('4399-on-vscode')));
});

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
// 内部处理逻辑（简化版）
const parsedId = parseId(userInput); // 提取数字ID
const gameUrl = `https://www.4399.com/flash/${parsedId}.htm`;
play(gameUrl); // 加载并运行游戏
4. 完整调用代码示例
若需以编程方式调用，可参考以下代码：

typescript
import * as vscode from 'vscode';

// 手动触发游戏ID输入
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
// 调用示例
manuallyInputGameId();


四. 手动输入游戏ID功能示例与代码解析

1. 功能示例代码
typescript
import * as vscode from 'vscode';

// 游戏启动函数
function playGame(gameId: string) {
    const gameUrl = `https://www.4399.com/flash/${gameId}.htm`;
    vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
}

// 手动输入游戏ID功能
export function activate(context: vscode.ExtensionContext) {
    let disposable = vscode.commands.registerCommand('game.inputId', async () => {
        // 弹出输入框
        const gameId = await vscode.window.showInputBox({
            placeHolder: '请输入4399游戏ID（如：222735）',
            prompt: '支持直接输入数字ID或完整游戏URL',
            validateInput: text => {
                // 验证输入是否有效
                const idPattern = /^(\d+|https?:\/\/www\.4399\.com\/flash\/\d+\.htm)$/;
                return idPattern.test(text) ? null : "请输入有效的游戏ID或URL";
            }
        });

        if (gameId) {
            // 提取纯数字ID（处理URL或纯数字输入）
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
2. 代码解析
功能入口：
使用vscode.commands.registerCommand注册game.inputId命令
当用户执行该命令时触发输入流程
输入框设计：
typescript
vscode.window.showInputBox({
    placeHolder: '请输入4399游戏ID（如：222735）',
    prompt: '支持直接输入数字ID或完整游戏URL',
    validateInput: text => { /* 验证逻辑 */ }
})
placeHolder: 输入框提示文本
prompt: 更详细的说明
validateInput: 实时验证输入有效性

输入验证：
typescript
/^(\d+|https?:\/\/www\.4399\.com\/flash\/\d+\.htm)$/
接受两种格式：
纯数字（如222735）
完整URL（如https://www.4399.com/flash/222735.htm）

ID提取：
typescript
const extractedId = gameId.match(/\d+/)?.[0];
使用正则表达式从输入中提取数字部分
同时兼容纯数字和URL格式的输入

游戏启动：
typescript
const gameUrl = `https://www.4399.com/flash/${gameId}.htm`;
拼接标准游戏URL
实际项目中会调用游戏加载逻辑

3. 使用场景示例
直接输入数字ID：
用户输入：222735
结果：启动https://www.4399.com/flash/222735.htm
输入完整URL：
用户输入：https://www.4399.com/flash/185252.htm
结果：启动https://www.4399.com/flash/185252.htm
错误输入处理：
用户输入：abc123
结果：显示错误提示"请输入有效的游戏ID或URL"