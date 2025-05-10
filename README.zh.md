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
1. 在 VSCode 中按下 `Ctrl+Shift+P`。
2. 输入命令：
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
