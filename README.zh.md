
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
=======
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

