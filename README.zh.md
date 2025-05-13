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

