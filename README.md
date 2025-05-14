#by lanzao (蓝早)
Project Name: 4399 on VSCode

Project Usage Instructions

 I. Functional Overview
4399 on VSCode is an extension that allows developers to access and play 4399 mini-games directly within VSCode. The main functions include:
1. Manual game ID input for launch
2. Game category browsing
3. Random game recommendation
4. Game search functionality
5. History record management
6. Game detail viewing

 II. Installation Guide
1. Prerequisites
   - The latest version of Visual Studio Code (1.70.0+) is installed.
   - Network connection is normal (access to 4399 domain is required).
2. Installation Method
   - Installation Tutorial
     - Method 1: Install within VSCode (Recommended)
       - Press `Ctrl+Shift+X` to open the extension panel.
       - Search for "4399 on VSCode".
       - Click the install button.
     - Method 2: Manual Installation
       - If you have downloaded the `.vsix` file:
         ```
         code --install-extension 4399-on-vscode-1.0.0.vsix
         ```

 III. Quick Start
1. Launch with Game ID Directly (Most Commonly Used!)
   - For example, to play Gold Miner (ID: 185252):
     1. Press `Ctrl+Shift+P` to open the command panel.
     2. Input "4399 get".
     3. Enter `185252`.
   - Tip: You can also directly paste the game link, such as [https://www.4399.com/flash/185252.htm](https://www.4399.com/flash/185252.htm).
2. Random摸鱼Mode:
   - `Ctrl+Shift+P` → Input "4399 random" → Enter.
   - [Ctrl+Shift+P] Input "4399 category".
   - Select a preferred category (Action/Strategy/Sports...).
   - Use the up and down keys to select a game and press Enter to start.

IV. Core Function Usage Instructions
1. Manual Game ID Input
   - Use Case: Quick start when the game ID is known.
   - Steps:
     - Open the command panel (`Ctrl+Shift+P`).
     - Input the command:
       - For Flash games: `4399: Enter Game ID`.
       - For H5 games: `4399: Enter H5 Game ID`.
     - In the input box, enter:
       - A pure numeric ID (e.g., `222735`).
       - Or a complete URL (e.g., [https://www.4399.com/flash/222735.htm](https://www.4399.com/flash/222735.htm)).
     - Press Enter to confirm.
   - Example:
     ```javascript
     // You can also call directly via API
     vscode.commands.executeCommand('4399-on-vscode.get', '222735');
     ```
2. Game Category Browsing
   - Steps:
     - Input `4399: Category Browsing` in the command panel.
     - Use the arrow keys to select a category (e.g., "Action", "Puzzle").
     - Press Enter to start after selecting a specific game.
3. Random Game
   - Command:
     ```
     4399: Try Your Luck
     ```
4. Game Search
   - Steps:
     - Input `4399: Search Games` in the command panel.
     - Enter keywords (e.g., "Gold Miner").
     - Select a game from the search results.

 V. Advanced Features
1. History Record Management
   - View History:
     - Command: `4399: History`.
     - Displays the last 10 games played.
   - Clear History:
     
     // Through settings operation
     vscode.workspace.getConfiguration('4399').update('history', []);
     ```
2. Custom Configuration
   - Add in `settings.json`:
     
     {
         "4399.defaultGameId": "222735",
         "4399.enableH5": true,
         "4399.historyLimit": 15,
         "4399.previewQuality": "high"
     }
     ```

VI. Common Issue Resolution
1. Game Fails to Load
   - Solutions:
     - Check network connection.
     - Confirm the game ID is correct.
     - Try refreshing the page (`F5`).
2. Invalid ID Input
   - Error Prompt:
     ```
     Invalid game ID format. Please enter a number or a complete 4399 game URL.
     ```
3. Performance Optimization
  
   // Lower preview quality to boost performance
   "4399.previewQuality": "low"
   ```

 VII. Development Interface
1. Extension API
   
   interface GameAPI {
       launch(gameId: string): Promise<void>;
       search(keyword: string): Promise<GameInfo[]>;
       getCategories(): Promise<Category[]>;
   }
   // Get API instance
   const api = vscode.extensions.getExtension('dsy4567.4399-on-vscode').exports;

VII. Notes on Using 4399 on VSCode
1. Flash Game Compatibility: Relies on the Ruffle emulator, and some games may have compatibility issues. If the game orientation is incorrect, try widening the game interface.
2. Unsupported Game Types: Does not support Unity3D games and new Flash games.
3. Network Environment:Issues may arise if not logged in or using a foreign IP (e.g., GitHub Codespaces or proxy).
4. Multi-Game Limitations: Cannot open another game before the current one has fully loaded (web games are exempt).
5. Shortcut Key Failure: Most VSCode shortcut keys may fail when the game interface has focus.
6. Mouse Locking Issues: Games cannot lock the mouse (e.g., shooting games).
7. Sound Issues: Users with VSCode versions below 1.71.0 may need to replace `ffmpeg` to enable sound.

 Common Issues and Solutions:
1. Game Fails to Load: Check your network connection, ensure the game ID is correct, and try refreshing the page (`F5`).
2. Invalid ID Input: Ensure you enter a numeric ID or a complete URL.
3. Performance Optimization: Reduce preview quality to enhance performance. Example configuration:
   ```json
   "4399.previewQuality": "low"
   ```
4. Extension Installation Failure: Ensure a stable network connection and check if your VSCode version is compatible.
5. Plugin Conflicts:Disable other extensions that may conflict.
6. Shortcut Key Failure: Shift focus back to the editor.
7. Sound Issues: Check your VSCode version; if below 1.71.0, try replacing `ffmpeg`.

Uninstallation Method
1. Enter the extension view (`Ctrl+Shift+X`).
2. Locate "4399 on VSCode".
3. Click the uninstall icon.
4. Restart VSCode.



#by lanzao （蓝早）

I. Function Name and Introduction

Function Name:Manual Game ID Input  
Introduction: In the VSCode extension "4399 on VSCode," users can quickly locate and launch a target game by manually entering the unique ID of a 4399 mini-game. This feature allows users to easily associate with specific games on the 4399 platform, supporting operations such as data analysis and code debugging.  

Problems Solved: 
1. Precise Access:Directly access the specified game by entering the ID, eliminating the need to search through multiple pages.  
2. Efficiency Improvement:Suitable for teaching/testing scenarios where the game ID is known, saving time.  
3. Development Assistance: Facilitates quick debugging and analysis of specific game resources for developers.  

II. Function Features
1. Precise Direct Access:irectly locate the target game via its unique game ID, avoiding cumbersome searches and enhancing access efficiency.  
2. Quick Operation: Integrated with the VSCode command panel (`Ctrl+Shift+P`), where entering the command triggers the function without the need to switch interfaces.  
3. Developer-Friendly:Supports rapid debugging and testing, allowing developers to quickly access game resources through the ID to optimize workflows.  
4. Flexible Compatibility: Compatible with the 4399 platform API, ensuring strong scalability and the potential for future integration with more games or custom functions.  
5. User Convenience:Simplifies operation steps, making it suitable for teaching, demonstration, and other scenarios to improve user experience.  
Dependencies: The extension's API interface and network environment.

III. Installation and Usage Guide
1. Install the Extension
   Ensure you have the latest version of VSCode installed (1.70.0+).  
   Install via the VSCode Extension Marketplace: Search for `4399 on VSCode`.  
   Or install using the command line:  
     ```bash
     code --install-extension dsy4567.4399-on-vscode
     ```

2. Function Activation 
   After installation, the command is automatically registered and requires no additional configuration.  
   To view registered commands:  
     ```typescript
     vscode.commands.getCommands().then(commands => {
         console.log(commands.filter(cmd => cmd.startsWith('4399-on-vscode')));
     });
     ```

3. Usage Steps
   Open the Command Palette**  
     Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (Mac) and enter the command:  
     For standard Flash games: `4399-on-vscode.get`  
     For H5 web games: `4399-on-vscode.get-h5-web-game`  
   Enter the Game ID
     In the input box that appears, enter one of the following formats:  
     ```typescript
     // Example 1: Directly enter the numeric ID
     222735
     // Example 2: Enter the complete URL
     https://www.4399.com/flash/222735.htm 
     ```  
     The input box supports auto-completion of historical records (implemented via globalStorage).  
   Run the Game 
     After entering the ID and pressing Enter, the extension will call the core logic:  
     ```typescript
     // Internal processing logic
     const parsedId = parseId(userInput); // Extract the numeric ID
     const gameUrl = `https://www.4399.com/flash/  ${parsedId}.htm`;
     play(gameUrl); // Load and run the game
     ```

4. Complete Invocation Code Example**  
   ```typescript
   import  as vscode from 'vscode';

   async function manuallyInputGameId() {
       const gameId = await vscode.window.showInputBox({
           title: '4399 Game ID Input',
           prompt: 'Enter Game ID or Complete URL',
           value: '222735' // Default value
       });
       
       if (gameId) {
           vscode.commands.executeCommand('4399-on-vscode.get', gameId);
       }
   }
   manuallyInputGameId();
   ```

IV. Function Example and Code Analysis
1. unction Example Code
   ```typescript
   import as vscode from 'vscode';

   function playGame(gameId: string) {
       const gameUrl = `https://www.4399.com/flash/  ${gameId}.htm`;
       vscode.window.showInformationMessage(`Launching game: ${gameUrl}`);
       // Actual game loading logic...
   }

   export function activate(context: vscode.ExtensionContext) {
       let disposable = vscode.commands.registerCommand('game.inputId', async () => {
           const gameId = await vscode.window.showInputBox({
               placeHolder: 'Enter 4399 game ID (e.g., 222735)',
               prompt: 'Supports direct input of numeric ID or complete game URL',
               validateInput: text => {
                   const idPattern = /^(\d+|https?:\/\/www\.4399\.com\/flash\/\d+\.htm)$/;
                   return idPattern.test(text) ? null : "Enter a valid game ID or URL";
               }
           });

           if (gameId) {
               const extractedId = gameId.match(/\d+/)?.[0];
               if (extractedId) {
                   playGame(extractedId);
               } else {
                   vscode.window.showErrorMessage('Invalid game ID format');
               }
           }
       });

       context.subscriptions.push(disposable);
   }
   ```

2. Code Analysis  
   Function Entry Point: Registered as a VSCode command via `vscode.commands.registerCommand`, callable from the VSCode command palette.  
   Retrieve Historical Input:Fetches the last entered game ID from global storage to provide a default value.  
   op Up Input Box:Calls `vscode.window.showInputBox` for users to enter the game ID or complete URL.  
   Process User Input:Validates the input format, extracts the numeric ID, constructs the game link, and launches the game.

 V. Extended Functions
1. Quickly Locate Specific Games:Enter the game ID to quickly locate specific games, saving time.  
2. ombine with History Function:Games entered are automatically added to the history for quick access later.  
3. Custom HTML Code Snippets:Inject custom HTML code into specific games to enhance the gaming experience.  
4. Support Multiple Game Types: Supports H5 web games, H5 mini-games, and some Flash mini-games (via Ruffle emulator).  
5. Assist Account Login and Management: Combine with account login functions to quickly switch accounts and continue game progress.  
6. Facilitate Game Development and Testing:Helps developers quickly locate games they are developing or testing, facilitating debugging and performance analysis.
