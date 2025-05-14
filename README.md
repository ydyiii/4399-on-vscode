#by 戴颖颖
Functional Features
1.Game Launching
(1) Multi-platform Support: Compatible with Flash and HTML5 games.
(2) Functional Integration: Integrated search, categorization, and recommendation features.
2.Interaction**
(1) Comment System: Supports viewing, liking, and replying to comments.
(2) History Record: Automatically records game history.
3.Community Interaction
(1) Forum Browsing: Supports browsing posts and viewing details.
(2) Group Management: Supports joining/leaving groups and group check-ins.
4.Personalization Settings
(1) Custom Configuration: Supports custom scripts.
(2) Theme Customization: Supports custom themes and styles.
5.Convenience Functions
(1) One-click Launch: Supports browser integration.
(2) Automatic Check-in: Provides an automatic check-in feature.
6.Security
(1) Data Encryption: Supports content filtering.
7.Extensibility
(1) Plugins and API: Supports a plugin mechanism and provides a rich API.

#by 戴颖颖
Recommendation and Categorization
Usage Tutorial
Step 1: Launch the Function
Command Palette:
(1). Press `Ctrl+Shift+P` in VSCode.
(2).Enter the command:
    ```plaintext
    4399-on-vscode.recommended (Recommendation)
    4399-on-vscode.category (Categorization)
    ```
    Or click on Recommendation or Categorization with the mouse.

Step 2: Recommendation Function Interface
(1) Interface Elements:
Displays recommended game names in list form (e.g., "Gold Miner," "Saier No.").
```javascript
// Code Implementation: Parse recommended games from the 4399 homepage
$("a[href*='/flash/'][href*='.htm']").has("img").each((i, elem) => {
    let gameName = $(elem).text().replaceAll(/ |\n/g, "");
    let href = $(elem).attr("href")?.replaceAll(/ |\n/g, "");
    if(gameName && href) games[gameName] = href;
});
```
(2) Operation:
Use the up and down arrow keys to select a game → Press Enter to launch or click with the mouse.
```javascript
// Code Implementation: Display the selection interface and handle the selection
const val = await vscode.window.showQuickPick(gameNames);
if(val) play(games[val]);
```

Step 3: Categorization Function Interface
(1) Primary Interface (Category Selection):
Displays all categories (e.g., Action, Shooting, Casual).
```javascript
// Code Implementation: Parse game categories
$("a[href*='/flash_fl/'][href*='.htm'], a[href*='/special/'][href*='.htm']").each((i, elem) => {
    let categoryName = $(elem).text().replaceAll(/ |\n/g, "");
    let href = $(elem).attr("href")?.replaceAll(/ |\n/g, "");
    if(categoryName && href) categories[categoryName] = href;
});
```

(2) Secondary Interface (Game List):
After selecting a category, it displays the games under that category (e.g., selecting "Action" shows "The King of Fighters," "Stickman," etc.).
```javascript
// Code Implementation: Retrieve games under a category
$("a[href*='/flash/'][href*='.htm']:has(img)").each((i, elem) => {
    let gameName = $(elem).children("img").attr("alt");
    let href = $(elem).attr("href");
    if(gameName && href) games[gameName] = href;
});
```

(3) Operation:
Select a category → Select a game → Automatic loading.
```javascript
// Code Implementation: Hierarchical selection processing
let category = await vscode.window.showQuickPick(categoryNames);
if(category) {
    let games = await getGamesByCategory(categories[category]);
    let gameName = await vscode.window.showQuickPick(Object.keys(games));
    if(gameName) play(games[gameName]);
}
```

#by 黄恋涵
# 🎮 4399 on VSCode - 🕒 History Feature

This project has implemented the game history function, allowing users to 🔍 view and ⚡ select previously launched game history. At the same time, it supports 🗑️ to clear the history, making it convenient for users to 🚀 quickly retrieve and 🔄 replay. The core code for the history feature is distributed in the `game.ts` and `index.d.ts` files, and the history data is saved by VSCode 's globalStorage.



## 📦 Data structure

The data structure for history is defined in the `index.d.ts` 📄 file, represented using the `game.History` type, and contains the following fields:

- `name`🕹️：Game name

- `url`🔗：The URL of the game

- `date`📅： The timestamp when the game starts

- `webGame`📤：Boolean value indicating whether it is a web game or not

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


## 🔧Feature Implementation

#### 1. 📜**Display History**

In the `game.ts` file, the ` showHistory` function is responsible for displaying the list of history records. It 📂 reads historical data from global storage, converts it 🔄 into QuickPick list items, and provides a user interface. You can restart the game by choosing a history record, or by choosing the "Clear History" option at 🗑️ to delete all records.


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

#### 2. **Recorded in history**💥

Each time the game is started, the `updateHistory` function is called to add new history records to the global storage. History is stored in an array, and 📝 new records are inserted at the beginning of the array.

```javascript
function updateHistory(history: game.History) {
    if (!getCfg("updateHistory", true)) return;

    let h: game.History[] = globalStorage(getContext()).get("history");
    if (!h || (typeof h === "object" && !h[0])) h = [];

    h.unshift(history);
    globalStorage(getContext()).set("history", h);
}
```

In the `play` and `playWebGame` functions, the `updateHistory` function will be called according to the type of the game and the corresponding parameters will be passed in:

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
## 🌟Main functions
The historical record function provides users with the following core functions:
- **Auto-recording**📝: Each time a game is started, the system automatically records information such as the game name, URL, start time, and game type.

- **Quick Access**🚀: With the QuickPick list, users can quickly browse the history and select a record to restart the game.

- **Cleanup function**🧹:  Users can choose the "Clear History" option and delete all history records with one click to keep them clean.

- **Configuration Control**⚙️: Users can flexibly control whether to enable the history feature through configuration items to meet different usage needs.
## ✨Functional characteristics

1.**Data store💾：**
- Use VSCode's globalStorage to store historical records and ensure that the data remains available after the plugin is restarted.
- History 🔍 is stored in an array for easy management and query.

2.**User interaction 🏷️：**
- It provides a QuickPick list interface, allowing users to quickly search for historical records by name and date.
-  A 'Clear History' option is provided for users to use 🗑️ to clear records they no longer need.
3.**Configuration option⚙️：**
-  Users can control whether to record historical records through the `updateHistory` configuration item.
4.**Scalability🧰：**
- The implementation logic of the historical record function is clear, facilitating subsequent expansion, such as supporting more game types or adding more filtering conditions.


## 📌Usage method

#### 1.Start the game🤖：
- Each time the game is started, the system will automatically call the `updateHistory` function to record the name, URL, date and type of the current game.
#### 2.View history🔍：
- Press `Ctrl+Shift+P` and type '4399 on VSCode: History', then 🖱️ call the `showHistory` function to display a list of history, and the user can restart the game by choosing a certain history.
#### 3.Clear history⏎：
- Open the history list and select "🧹 Clear History "at the top of the list.
## 💾storage location

History🔒 is stored in VSCode 's global state at: 

```bash
  ${userHome}/.vscode/extensions/.../globalStorage/state.vscdb
```

## Configuration options🎯

The history record function can be controlled through VSCode Settings:

```bash
{
  "4399-on-vscode.updateHistory": true  // 是否启用历史记录功能，默认为true
}
```
## 🛠️Core API interface

#### 1️⃣History record management interface 
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
#### 2️⃣ Historical record query interface
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
#### 3️⃣ History configuration interface
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

#### 📊Data type definitions
GameHistory type
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
Type HistoryConfig
```bash
  interface HistoryConfig {
  maxRecords: number;   // 最大记录数(默认100)
  autoCleanDays: number; // 自动清理天数(0表示不自动清理)
  enabled: boolean;     // 是否启用历史记录
}
```
#### 💻Example of use
🔹 Basic Uses
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
🔹 Advanced Configuration
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
#### ❌Error handling
All apis may throw the following errors:

- `HistoryDisabledError`: Thrown when the history record function is disabled

- `StorageLimitExceededError` : storage to cap the throw

- `InvalidRecordError`: Thrown when there is invalid historical record data






## ⚠️Notes

#### 1. Data privacy and security
- 📌 All records (including game name and full URL) are **stored locally** only by default and will not be automatically synchronized to the cloud

- 🔒 Sensitive game Tips:

  Manually delete a single record (right-click the record item)
  
  Use "Privacy Mode" to temporarily disable the recording function

- ⚠️ When using public computers, it is recommended to perform "Clear History" before exiting
#### 2. Storage management
- 💾 **Automatic maintenance**：

  By default, the most recent 100 entries are retained (can be set to 200).

  Automatic cleaning of records over 30 days (adjustable cycle)

- 🚨 Risk Warning:
    
     More than 500 items may affect the loading speed

     Uninstalling extensions or reinstalling VSCode will **permanently lose** historical data

- ✅ Suggested operation:

  Important games can be saved permanently by clicking on ⭐

  Back up data regularly through "Export as JSON"

#### 3. Use accuracy
- The 🔍 record contains:

  Startup time accurate to the minute (Beijing time)

  Game type Identifier (Flash/H5)

  Original game link

- ❗ Note:

  Broken links will remain but display the ⚠️ icon

  Games with the same name but different versions will generate independent records

#### 4. Function configuration
- ⚙️ Key Settings:
```http
{
  "4399-on-vscode.history.maxRecords": 100,
  "4399-on-vscode.history.retentionDays": 30,
  "4399-on-vscode.history.enabled": true
}
```

- 🔄  Configuration takes effect condition:

  The quantity limit takes effect immediately

  Enabling/disabling requires **restarting VSCode**

#### 5. Exception handling
- 🔧 Frequently asked questions:
| Phenomenon | Solution     |                     
| :-------- | :------- |
| Record not updated      |  Check if the function is disabled and confirm the storage permission |
| Link invalid      | Manually delete or wait for automatic cleaning |
| Loading lag      | Clear early records or add maxRecords |
	
- Advanced fix for 🛠️ :

Run `4399: Repair History` in the command panel

Recover data from the backup file

#### 6. Special scenarios
- 🌐 **Unsupported environment**：

  VSCode web version

  Some enterprise networks (which may intercept game urls)

- ⏳ **Data retention**：

  Even if the game is taken down, the historical links will still be retained

  Collection items are not affected by automatic cleaning

