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