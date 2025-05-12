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
通过VSCode扩展市场搜索安装
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
            "<url id="d0gqdfms1rhbjs7jdigg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> " + parseId(id) + ".htm"
        );
    }
});
使用.then方法处理用户在输入框中输入的内容（id）。
如果用户输入了内容（id不为空）：
调用log函数记录用户输入的内容，方便开发者进行调试和跟踪用户的操作。
将用户输入的游戏ID存储到全局存储中，键为id1，以便下次用户使用该功能时可以获取到上次输入的值。
调用play函数，将用户输入的游戏ID拼接到基础的游戏链接https://www.4399.com/flash/后面，并加上.htm后缀，形成完整的游戏链接，然后通过play函数启动游戏。

（6）. 游戏ID解析
play(
    "<url id="d0gqdfms1rhbjs7jdigg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> " + parseId(id) + ".htm"
);
在拼接游戏链接时，调用了parseId函数对用户输入的id进行解析。
parseId函数的作用可能是对用户输入的内容进行处理，提取出符合要求的游戏ID部分。例如，用户可能输入的是一个完整的游戏链接，parseId函数可以从链接中解析出游戏ID，或者对输入的ID进行格式化等操作，确保最终拼接的游戏链接是正确的。

（7）. 类似的H5游戏ID输入功能
输入游戏 ID (链接以 http(s)://www.zxwyouxi.com/g/ 开头) */ "get-h5-web-game":
    () => {
        let i = globalStorage(ctx).get("id2");
        vscode.window
            .showInputBox({
                value: i ? String(i) : "100060323",
                title: "4399 on VSCode: 输入游戏 ID",
                prompt: "输入游戏链接或 http(s)://www.zxwyouxi.com/g/ 后面的数字(游戏 ID)",
            })
            .then(id => {
                if (id) {
                    log("用户输入 ", id);
                    globalStorage(ctx).set("id2", id);
                    playWebGame(
                        "<url id="d0gqdfms1rhbjs7jdih0" type="url" status="parsed" title="页面不见啦" wc="1087">https://www.zxwyouxi.com/g/</url> " + parseId(id)
                    );
                }
            });
    },
这是一个与get功能类似的函数，专门用于处理以http(s)://www.zxwyouxi.com/g/开头的H5游戏链接。
它的实现逻辑与get函数基本相同，只是存储历史输入值的键为id2，默认值为"100060323"，并且调用的是playWebGame函数来启动H5游戏。

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

五. 扩展功能
手动输入游戏ID功能的相关扩展功能：
1.快速定位特定游戏：用户可以通过手动输入游戏ID，快速定位到4399平台上的特定游戏，无需在众多游戏列表中逐一查找，节省时间，提高效率。

// 快速定位特定游戏
function playGame(gameId: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
}

2.结合其他功能增强体验：
与历史记录功能结合：输入游戏ID后，该游戏会被添加到历史记录中，方便用户后续快速访问曾经玩过的游戏。

// 历史记录功能
function addToHistory(gameId: string) {
    let history = globalStorage(ctx).get("history") || [];
    if (!history.includes(gameId)) {
        history.push(gameId);
        globalStorage(ctx).set("history", history);
    }
}

// 在playGame函数中调用addToHistory
function playGame(gameId: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
    addToHistory(gameId);
}

3.搭配自定义HTML代码片段：用户可以为特定游戏注入自定义HTML代码片段，优化游戏体验或实现一些特殊功能。例如，调整游戏界面布局以更好地适配VSCode窗口。

// 自定义HTML代码片段功能
function injectCustomHtml(gameId: string, customHtml: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
    // 注入自定义HTML代码
    vscode.window.showInformationMessage(`注入自定义HTML代码: ${customHtml}`);
}

// 在playGame函数中调用injectCustomHtml
function playGame(gameId: string) {
    const customHtml = globalStorage(ctx).get("customHtml") || "";
    injectCustomHtml(gameId, customHtml);
}

4.支持多类型游戏：手动输入游戏ID功能不仅适用于H5页游和H5小游戏，还支持部分Flash小游戏（借助Ruffle模拟器），满足用户对不同类型游戏的需求。

// H5页游和H5小游戏
function playH5Game(gameId: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1bfg" type="url" status="waiting" title="" wc="0">https://www.zxwyouxi.com/g/</url> ${gameId}`;
    vscode.window.showInformationMessage(`正在启动H5游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
}

5.辅助账号登录与管理：在输入游戏ID后，用户可以结合登录账号功能，快速切换到自己的4399账号，继续之前的游戏进度或体验账号内的游戏内容。

// Flash小游戏
function playFlashGame(gameId: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`正在启动Flash游戏: ${gameUrl}`);
    // 实际游戏加载逻辑...
    // 使用Ruffle模拟器
    vscode.window.showInformationMessage(`使用Ruffle模拟器加载Flash游戏`);
}

6.助力游戏开发与测试：对于开发者来说，手动输入游戏ID功能可以帮助他们快速定位到自己正在开发或测试的游戏，便于进行代码调试、性能分析等操作，提升开发效率。

// 助力游戏开发与测试
function debugGame(gameId: string) {
    const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`正在调试游戏: ${gameUrl}`);
    // 实际游戏调试逻辑...
}

// 在playGame函数中调用debugGame
function playGame(gameId: string) {
    const isDebugMode = globalStorage(ctx).get("isDebugMode") || false;
    if (isDebugMode) {
        debugGame(gameId);
    } else {
        const gameUrl = `<url id="d0grh35kfv3sjf0b1beg" type="url" status="parsed" title="最新小游戏,最新网页游戏,最新单机小游戏,2016最新热门小游戏大全" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
        vscode.window.showInformationMessage(`正在启动游戏: ${gameUrl}`);
        // 实际游戏加载逻辑...
    }
}