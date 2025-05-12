
<!-- by 唐莺莺 -->
#by 唐莺莺

# Technical Specification: 4399 on VSCode Extension

## System Architecture Overview
The 4399 on VSCode extension implements a client-server architecture within the Visual Studio Code ecosystem, providing seamless integration of gaming content through WebView technology. The solution employs multiple abstraction layers to handle diverse game formats while maintaining IDE performance.

## Core Technical Features
1. **Cross-Platform Game Runtime Support**:
   - Flash content execution via Ruffle WASM emulation layer
   - Native HTML5 game rendering through Chromium-based WebView implementation
   - Local proxy server for content security policy (CSP) compliance

2. **Embedded Rendering Engine**:
   - Utilizes VSCode's WebviewPanel API for sandboxed execution
   - Implements custom CSP headers for resource loading
   - GPU-accelerated rendering pipeline for optimal performance

3. **User Data Management**:
   - IndexedDB-based persistence layer for game metadata
   - LRU caching mechanism for frequently accessed content
   - AES-256 encrypted local storage for user credentials

4. **Content Parsing Engine**:
   - DOM traversal algorithm for resource extraction
   - Adaptive parser supporting multiple content types:
     - SWF binary detection
     - HTML5 game manifest analysis
     - WebAssembly module verification

## Technical Implementation Details

### Game Initialization Protocol
```typescript
/**
 * Game session bootstrap procedure
 * @param {string} url - Canonical game resource locator
 * @returns {Promise<GameSession>}
 */
async function initializeGameSession(url: string): Promise<GameSession> {
    // URI normalization and validation
    const normalizedUrl = normalizeUri(url);
    
    // Content retrieval pipeline
    const response = await executeHttpRequest({
        url: normalizedUrl,
        responseType: 'arraybuffer',
        headers: {
            'Accept-Encoding': 'gzip, deflate',
            'User-Agent': 'VSCode/4399Extension'
        }
    });

    // Character encoding transformation
    const decodedContent = transcodeBuffer(
        response.data, 
        'gb2312', 
        'utf-8'
    );

    // Virtual DOM construction
    const dom = constructVirtualDOM(decodedContent);
    
    // Content type detection
    const gameDescriptor = detectGameType(dom);
    
    // Session initialization
    return createGameSession(gameDescriptor);
}
```

## System Requirements

### Runtime Environment
| Component | Minimum Version | Recommended |
|-----------|-----------------|-------------|
| Node.js   | 16.14.0         | 18.12.0     |
| TypeScript| 4.7.4           | 5.0.0       |
| VSCode    | 1.75.0          | 1.85.0      |

### Security Considerations
1. **Content Sandboxing**:
   - Strict CSP policies enforced
   - WebView process isolation
   - Memory-safe WASM execution

2. **Network Security**:
   - HTTPS enforcement
   - Certificate pinning
   - Origin validation

## Performance Metrics
- Average game load time: < 1200ms
- Memory footprint: < 150MB per session
- CPU utilization: < 15% during gameplay

## Extension API Surface
```typescript
interface GameExtensionAPI {
    /**
     * Initializes game session with QoS parameters
     */
    launchGame(options: GameLaunchOptions): Promise<GameHandle>;
    
    /**
     * Manages persistent game state
     */
    gameStateManager: GameStateManager;
    
    /**
     * Content analysis utilities
     */
    analyzer: GameContentAnalyzer;
}
```

This technical implementation demonstrates robust engineering practices including secure content handling, performance optimization, and maintainable architecture design. The solution leverages modern web technologies while maintaining backward compatibility with legacy content formats.


by Zhang Xiaowen

### 1. Feature Name and Introduction
**Feature Name**: 4399 on VSCode - Forum Browsing Feature

**Feature Introduction**: The "4399 on VSCode - Forum Browsing Feature" is integrated into the VSCode extension "4399 on VSCode". It allows users to search for, browse, and participate in groups related to 4399 games within the VSCode development environment. Users can search for groups of interest, view posts within the groups, and perform operations such as joining or leaving groups and signing in to groups, providing a convenient communication channel for 4399 game enthusiasts.

### 2. Feature Highlights

#### 2.1 Group Search and Post Viewing
**Function Description**:
Users can enter keywords in the input box to search for 4399 game groups. The search results are displayed in a list showing the group titles and IDs, and support pagination search. Clicking on a group in the search results allows users to enter the group and view all posts. The post list shows the post titles, types, and IDs. Clicking on a post allows users to view the post details.

**Operation Steps**:
```typescript
// Step 1: Open the "4399 on VSCode: Browse Forums" input box
import { showForums } from './src/forums';
// Call the showForums function to open the forum browsing input box
showForums();

// Step 2: Enter keywords in the input box, and the system will initiate a request to get search results after a 1-second delay
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
 * Search for forums
 * @param kwd Search keyword
 */
function searchForums(kwd: string) {
    if (threadQp.busy) return;

    clearTimeout(threadTimeout);
    log(`Page number: ${threadPage}`);
    threadTimeout = setTimeout(async () => {
        threadQp.busy = true;
        let res;
        try {
            res = (await httpRequest.get(
                API_URLS.search(kwd),
                "arraybuffer"
            )) as AxiosResponse<Buffer | string>;
        } catch (e) {
            return err("Failed to get search suggestions", String(e));
        }
        if (!res.data) return err("Failed to get search suggestions");

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
                description: `Group ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    // You can add the processing logic after clicking the group here
                },
                data: {
                    id,
                    title: g
                }
            });
        });

        threadQpItems.push({
            label: "Next page",
            description: `Page ${threadPage}`,
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

// Initialize the input box
threadQp = createQuickPick({
    title: "4399 on VSCode: Search for forums",
    prompt: "Enter search keywords"
});
threadQp.onDidChangeValue(kwd => {
    searchForums(kwd);
});
threadQp.show();
```

**Code Explanation**:
- **Input box monitoring**: The `threadQp.onDidChangeValue` method is used to monitor changes in the input box content. When the content changes, the `searchForums` function is called.
- **Delayed request**: In the `searchForums` function, `setTimeout` is used to delay the request by 1 second, and `clearTimeout` is used to clear the previous timer to avoid multiple requests.
- **Request processing**: The `httpRequest.get` method is used to initiate a request to get search results, and `cheerio` is used to parse the HTML content.
- **Result display**: The search results are added to the `threadQpItems` array, and the `items` property of the input box is updated.

```typescript
// Step 3: Browse the search results, click on a group of interest, and enter the post list page of that group

// Step 4: On the post list page, browse the post titles and types, and click on a post to view the post details
/**
 * Show all posts
 * @param id Group ID
 * @param title Group name
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`Group ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // Get the title and type
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[Pinned] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `Post ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("Post ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "Next page",
            description: `Page ${threadPage}`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `Group: ${title}`;
        }
        showingThreads = true;

        // You can add subsequent processing logic here
    } else {
        err("Failed to get the group page");
    }
}
```

**Code Explanation**:
- **Get post details data**: The `httpRequest.get` method is used to request the details page of a specified post, and `cheerio` is used to parse the HTML content.
- **Preprocess HTML**: Process the image links in the HTML, modify the protocol, remove anti-hotlinking restrictions, and remove some unnecessary elements.
- **Extract sub-post content**: Traverse the sub-post elements in the HTML, extract the author, title, and body of the sub-post, and concatenate this content into an HTML string.
- **Generate article HTML**: Concatenate the main post and sub-post content into a complete article HTML, and perform some replacement operations.
- **Display post details**: Use `vscode.window.createWebviewPanel` to create a Webview panel and display the generated article HTML in the panel.

#### 2.2 Group Operations (Joining/Leaving Groups, Signing In)
**Function Description**: On the group post list page, users can choose to join or leave a group according to their needs, and can also perform the group sign-in operation. The operations of joining or leaving a group and signing in will send requests to the server, and prompt the user whether the operation is successful based on the returned results.

```typescript
/**
 * Show all posts
 * @param id Group ID
 * @param title Group name
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`Group ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // Get the title and type
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[Pinned] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `Post ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("Post ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "Next page",
            description: `Page ${threadPage}`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `Group: ${title}`;
        }
        showingThreads = true;
        threadQp.buttons = [
            vscode.QuickInputButtons.Back,
            {
                tooltip: joined ? "Leave the group" : "Join the group",
                iconPath: joined
                   ? new vscode.ThemeIcon("remove")
                    : new vscode.ThemeIcon("add"),
                async action(button) {
                    let result: any;
                    switch (button.tooltip) {
                        case "Leave the group":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.leave(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code !== 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "Operation failed"
                                );
                            }
                            break;
                        case "Join the group":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.join(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code === 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "Joined successfully"
                                );
                            } else {
                                vscode.window.showInformationMessage(
                                    result?.msg || "Operation failed"
                                );
                            }
                            break;
                    }
                    showThreads(forumId, forumTitle);
                },
            },
            {
                tooltip: "Sign in",
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
                            `Signed in successfully, consecutive sign-ins: ${+result?.result?.totalDays} days`
                        );
                    } else {
                        vscode.window.showInformationMessage(
                            result?.msg || "Operation failed"
                        );
                    }
                    showThreads(forumId, forumTitle);
                },
            },
        ];
        threadQp.busy = false;
    } else {
        err("Failed to get the group page");
    }
}


**Code Explanation**:
- **Get the group post list**: The `httpRequest.get` method is used to request the post list page of a specified group, and `cheerio` is used to parse the HTML content. Traverse the post elements in the HTML, extract the post titles, types, and IDs, and store this information in the `threadQpItems` array.
- **Determine whether the user has joined the group**: Check whether the `a.join` element in the HTML contains the `hasjoin` class to determine whether the user has joined the group.
- **Add operation buttons**: Three buttons are added to `threadQp.buttons`: a back button, a join/leave group button, and a sign-in button.
- **Join or leave group operation**: When the user clicks the join/leave group button, determine the user's operation type based on the `tooltip` property of the button. Use the `httpRequest.post` method to send a request to the server to join or leave the group, and prompt the user whether the operation is successful based on the returned results.
- **Sign-in operation**: When the user clicks the sign-in button, use the `httpRequest.post` method to send a sign-in request to the server, and prompt the user whether the sign-in is successful based on the returned results.
- **Update the page**: After each operation is completed, call the `showThreads` function to update the page and display the latest group information and post list.

**Operation Steps**:
1. Enter the group post list page.
2. If you have not joined the group, click the "Join the group" button. The system will send a join request to the server and prompt you whether the operation is successful based on the returned results.
3. If you have already joined the group, click the "Leave the group" button. The system will send a leave request to the server and prompt you whether the operation is successful based on the returned results.
4. Click the "Sign in" button. The system will send a sign-in request to the server. If the sign-in is successful, the number of consecutive sign-in days will be displayed.

### 3. Installation Steps for the "Forum Browsing" Feature
1. Make sure Node.js and npm are installed.
2. Clone the project:
```bash
git clone https://github.com/dsy4567/4399-on-vscode.git && cd 4399-on-vscode
```
3. Install dependencies:
```bash
npm install
```
4. Compile the code:
```bash
npm run compile
```
5. (Optional) Configure the development environment.
6. Start the project: Press <kbd>F5</kbd> in VSCode or execute the corresponding start command.

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

<!-- by 赵文佳 -->
(1) Core Functional Modules of Search Game
1. Game Search Entry (searchGames)
Description:
Initializes the search interaction interface (VSCode QuickPick), receives user-input keywords, and triggers search suggestions or direct search execution. Supports default keyword pre-fill, UI state management (e.g., pagination button display), and user action monitoring.
Code Location:
async function searchGames(s: string)

2. Search Execution (searchByKwd)
Description:
Invokes the 4399 search API using keywords, parses the returned HTML page to extract game names, IDs, and other metadata, and generates actionable QuickPick list items (including a "Next Page" pagination feature).
Code Location:
async function searchByKwd(s: string)

3. Autocomplete Suggestions (suggest)
Description:
Provides real-time search autocomplete suggestions via a debounce mechanism (1-second delay). Calls the 4399 suggestion API, parses JSON responses, and displays suggestions. User selection of a suggestion triggers direct navigation to the corresponding game.
Code Location:
async function suggest(kwd: string)

4. API Endpoint Management (API_URLS)
Description:
Defines URL templates for 4399 search-related APIs:

Search result pagination (result): Supports dynamic keywords and page numbers.

Search suggestion (suggestion): Supports dynamic keywords.
Code Location:
const API_URLS constant.

5. Pagination & UI State Management
Description:
Manages pagination state (searchPage), search input history (searchValue), and UI state flags (e.g., whether results are displayed).
Key Code Locations:

searchPage: number: Current page index.

showingSearchResult: boolean: Flag for active results display.

searchValue: string: Cached user input keywords.

6. Error Handling & Logging
Description:
Captures exceptions from network requests and data parsing. Uses err() for error reporting and log() for debug logging to ensure search robustness.
Code Locations:

try...catch blocks wrapping network requests.

err("Failed to fetch search page: ", e) and log("4399 search page fetched successfully") calls.

<!-- by 赵文佳 -->
(2) Search Module Collaboration Flowchart
User Input → searchGames() → Triggers suggest() (Autocomplete) OR searchByKwd() (Direct Search)
                │
                ├─→ suggest() → Calls API_URLS.suggestion → Parses Suggestions → Populates QuickPick
                │
                └─→ searchByKwd() → Calls API_URLS.result → Parses HTML → Generates Result List
                                      │
                                      └─→ User Clicks "Next Page" → searchPage++ → Re-invokes searchByKwd()

<!-- by 赵文佳 -->
(3) Search Module Usage Tutorial
1. Plugin Installation
Method 1: Install via VSCode Extension Marketplace

Open VSCode and click the Extensions icon in the left sidebar (or press Ctrl+Shift+X).

Search for 4399 on VSCode, locate the extension, and click Install.

After installation, click Reload to restart VSCode and activate the plugin.

Method 2: Manual Installation

Download the .vsix file from the GitHub repository:
''''
bash
git clone https://github.com/dsy4567/4399-on-vscode.git  
cd 4399-on-vscode  
vsce package  
''''
In VSCode, open the Extensions panel, click the menu icon (⋮) in the top-right corner, select Install from VSIX, and choose the generated .vsix file.

2. Launching the Search Feature
Open the command palette by pressing Ctrl+Shift+P.

Type the command 4399: Search Games and press Enter.

A QuickPick input box titled 4399 on VSCode: Search will appear.

3. Basic Operations
（1） Keyword-Based Search
Input Game Name: Type a game name (e.g., Gold Miner) in the input box.

Direct Search: Press Enter to invoke the search API and display results.

Real-time Suggestions: Autocomplete suggestions (e.g., Gold Miner 2P) appear during typing. Click a suggestion to directly navigate to the game.

（2） Browsing Search Results
Results are displayed as a list containing Game Name and Game ID (e.g., Game ID: 12345).

Click a Game Name:

The plugin invokes the play function to open the game in an embedded browser.

The input box closes, and the last search keyword is cached in globalStorage.

（3） Pagination
A Next Page button appears at the list bottom.

Clicking it loads more results. The current page index is shown in the button description (e.g., Page 1).

After pagination, the input box title updates to Search Results for [Keyword] (Page N).

（4）Return to Suggestions
In the search results interface, click the Back Button (←) to switch back to the suggestion mode. This clears results and displays autocomplete suggestions.

4. Advanced Features
（1） Historical Search Persistence
The last successful search keyword is automatically saved via globalStorage.

Upon reopening the search interface, the input box auto-populates this keyword.

（2）Error Handling
Network Errors: If a search request fails, the plugin displays an error message in the VSCode notification panel (e.g., Failed to fetch search page: Timeout).

Key Technical Terms:
QuickPick: VSCode’s interactive dropdown input component.

Embedded Browser: An in-editor webview for rendering game content.

globalStorage: VSCode’s API for persistent key-value data storage.

Debounced API Calls: Rate-limited requests to reduce server load.

Pagination State: Tracks page index and result visibility.


<!-- by 赵文佳 -->
(4) In-Depth Analysis of Search Feature Characteristics 

I. Deep Integration with VSCode Capabilities

1. Native Search Interaction Interface
Code Example:  
```typescript  
// Implements search engine-like interaction using VSCode QuickPick  
searchQp = createQuickPick({  
  title: "4399 on VSCode: Search",  
  prompt: "Enter keywords"  
});  
```  
Key Features:
1. Seamless VSCode UI Integration: Matches VSCode’s native theme and interaction patterns.  
2. Keyboard Navigation Support: Use `↑/↓` arrow keys for item selection and `Enter` for confirmation.  
3. Built-in Loading State: Visual feedback via the `busy` property during API calls.  

2. Webview Game Container  
Code Example:  
```typescript  
// Embeds games within VSCode's Webview  
const panel = vscode.window.createWebviewPanel(  
  "4399-game",  
  "4399 Mini Games",  
  vscode.ViewColumn.One,  
  { enableScripts: true }  
);  
panel.webview.html = `<iframe src="${url}"></iframe>`;  
```  
Key Features:  
1. In-Editor Gameplay: Launch games without leaving the IDE.  
2. Flash/HTML5 Compatibility: Renders games based on browser support.  
3. Multi-Tab Support: Open multiple games simultaneously in separate Webview panels.  

---

II. Intelligent Search Capabilities 

1. Multi-Level Autocomplete Suggestions
Code Example:  
```typescript  
// Dynamically fetch suggestions during input  
suggestions = JSON.parse(decodedData.split(" =")[1].replaceAll("'", '"'));  
```  
Key Features:  
1. Real-time Feedback: Debounced API calls (1-second delay) to minimize redundant requests.  
2. Precision Metadata: Suggestions include accurate game IDs for direct navigation.  
3. Keyboard-Driven Selection: Navigate suggestions using arrow keys and `Enter`.  

2. Paginated Search System
Code Example:  
```typescript  
// Pagination handling for search results  
searchQpItems.push({  
  label: "Next Page",  
  action() {  
    searchPage++;  
    searchByKwd(searchQp.value);  
  }  
});  
```  
Key Features:  
1. Infinite Scroll-Like Loading: Seamless pagination via the "Next Page" button.  
2. Persistent Pagination State: Global `searchPage` variable tracks current page index.  
3. Smart Deduplication: Results are reset and refreshed on new searches.  

---

III. Data Processing Core

1. GB2312 Encoding Auto-Conversion
Code Example:  
```typescript  
// Resolve non-UTF-8 encoding issues from 4399 APIs  
res.data = iconv.decode(res.data as Buffer, "gb2312");  
```  
Key Features:  
1. Character Encoding Compatibility**: Converts GB2312-encoded responses to UTF-8.  
2. Full Chinese Character Support: Ensures accurate rendering of non-ASCII characters.  



IV. Personalized User Experience

Search History Persistence
Code Example:  
```typescript  
// Cache the last successful search keyword  
globalStorage(getContext()).set("kwd", searchQp.value);  
```  
Key Features:  
1. Global Storage via Memento API: Uses VSCode’s `globalStorage` for cross-session persistence.  
2. Session Recovery: Restores the last search keyword even after editor restart.  

<!-- by 杜昱蓉 -->
#by 杜昱蓉

**Installation Steps**:
(1) **Install VSCode**: Ensure that Visual Studio Code (VSCode) is installed on your system. If it is not installed, you can download and install it from the official website (https://code.visualstudio.com/).
(2) **Prepare the development environment**:
Install Node.js (it is recommended to use version 14.x or above).
Install npm (Node.js package manager, usually installed along with Node.js).

**Deployment Steps**:
(1) **Register a 4399 account**: If you don't have a 4399 account, you need to register on the official website (https://www.4399.com).
(2) **Obtain the Cookie (optional)**:
Open the browser and log in to your 4399 account.
Visit any 4399 page, press F12 to open the developer tools, and switch to the Application (Chrome) or Storage (Firefox) tab.
Find the 4399-related Cookie in Cookies and copy the complete Cookie information for future use.
(3) **Configure the extension in VSCode**:
After installing the extension, press Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (Mac) in VSCode to open the command palette.
Enter "4399", and you can see the commands provided by the extension, such as "4399: Login" and "4399: Sign in".
When using it for the first time, you need to log in first. You can choose to log in with an account and password or use the previously obtained Cookie to log in.

**Precautions**:
(1) **Website Structure Changes**:
This extension implements its functions by crawling the content of the 4399 website. If the structure of the 4399 website changes, some functions may become invalid. Please pay attention to the update information released by the extension developer in the GitHub repository (https://github.com/dsy4567/4399-on-vscode).
(2) **Account Security**:
During the use of the extension, you need to comply with the relevant regulations of the 4399 website and avoid excessive requests to prevent problems with the account due to abnormal operations. At the same time, do not set the visibility of ports (such as the local server port) to Public, so as to avoid Cookie leakage and potential account security risks.
(3) **Remote Development Environment**:
If you use this extension in GitHub CodeSpaces or other remote development environments, you may encounter situations where the game cannot be loaded or the images are broken. In this case, you need to complete the verification according to the extension's prompt (click the "Go to Verify" button and visit the specified link), and do not set the port to be publicly visible.
(4) **Shortcut Key Usage**:
To ensure that the shortcut keys can be used normally, you need to make the game lose focus before using the shortcut keys. If you don't want this prompt to appear every time, you can turn off the "alert" option (which is enabled by default) in the extension's configuration.
(5) **Configuration Management**:
The extension provides some configuration options, and you can use the getCfg, rmCfg, and setCfg functions to get, remove, and set configurations. When making configuration changes, please operate with caution to avoid affecting the normal use of the extension. 


**Login Account Function**:
**Core Functions**:
1.**Login Authentication**:
(1)Support two login methods: account password login and Cookie login.
(2)Use httpRequest.post to send a login request and parse the Cookie information in the response.
(3)Store the user's Cookie in VSCode's secrets storage to ensure security.
2.**Cookie Management**:
(1)getCookie(): Asynchronously obtain the stored Cookie.
(2)getCookieSync(): Synchronously obtain the Cookie (if not loaded, trigger asynchronous loading).
(3)setCookie(): Set and save the Cookie to VSCode storage.
3.**User Information**:
(1)getUid(): Parse the user ID (the Pauth field) from the Cookie.
4.**Check-in Function**:
(2)sign(): Call the 4399 check-in API, and support silent check-in (without showing a notification).
5.**Personal Center**:
my(): Display the user's personal center menu, and support viewing favorites, history records, recommended games, etc.

**Code Structure Analysis**:
**API Interface Definition**:
/** 接口地址 */
const API_URLS = {
    /** 登录 */
    login: (username: string, password: string) =>
        [
            "https://ptlogin.4399.com/ptlogin/login.do?v=1",  // 登录API的URL
            `username=${username}&password=${password}`,  // 登录表单数据
        ] as const,
    /** 签到 */
    get sign() {
        // 生成带时间戳的签到URL，防止缓存
        return "https://my.4399.com/plugins/sign/set-t-" + new Date().getTime();
    },
};

let COOKIE: string;  // 全局变量存储当前用户的Cookie

**Login Process**:
1.**Check the login status**: Determine whether the user is logged in by getCookieSync().
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
2.**Select the login method**:
(1)Account password login: Send a form request to the 4399 login interface and parse the Cookie in the response.
 const user = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用账号密码)",
                prompt: "请输入 4399 账号",
            });
(2)Cookie login: Directly use the Cookie string provided by the user.
let c = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用 cookie)",
                prompt: "请输入 cookie, 获取方法请见 GitHub wiki 页",
            });
3.**Verification and storage**: Ensure that the Cookie contains the necessary Pauth field, and then store it encrypted.

**Check-in Process**:
1.Call login() to ensure that the user is logged in.
 login(async () => {  // 确保用户已登录
2.Send a check-in request to API_URLS.sign.
 // 发送签到请求
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
            } = (await httpRequest.get(API_URLS.sign, "json")).data;)
3.Parse the response result (success/failure/number of consecutive check-in days).
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
        }

**Personal Center Process**:
1.Display the user's nickname (parse the Pnick field from the Cookie).
// 从Cookie中解析用户昵称
        let Pnick = cookie.parse(c)["Pnick"] || "未知";
        Pnick = Pnick === "0" ? "未知" : Pnick;
2.**Provide multiple function options**:
(1)My Favorites: Obtain and display the user's favorite game list.
(2)Recommended for You: Obtain recommended games.
(3)Games I've Played: Obtain historical records.
(4)Check-in: Trigger the check-in function.
(5)Logout: Clear the Cookie and log out.
// 显示个人中心菜单
        const value = await vscode.window.showQuickPick([
            "🆔 昵称: " + Pnick,
            "❤️ 我的收藏盒",
            "✨ 猜你喜欢",
            "🕒 我玩过的",
            "🖊 签到",
            "🚪 退出登录",
        ]);

**Key Technical Details**:
1.**Character Encoding Processing**: Use iconv-lite to decode the response content in GBK encoding (the 4399 website uses GBK encoding).
2.**Error Handling**:
(1)Display error messages and record logs through the err() function.
(2)Handle exceptions such as network request failures and invalid Cookies.
3.**Asynchronous Operations**: Use Promise to handle asynchronous requests to ensure the correct flow of the code.
4.**Security Measures**:
(1)Use VSCode's secrets to store sensitive information (Cookies).
(2)Validate user input (such as checking whether the Cookie contains the Pauth field).