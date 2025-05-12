#by lanzao （蓝早）

I. Function Name and Introduction
Function Name: Manual Game ID Input
Introduction: In the VSCode extension "4399 on VSCode," the Manual Game ID Input function allows users to directly locate and launch a target game by entering the unique ID of a 4399 mini-game. This function enables users to quickly associate with a specific game on the 4399 platform through manual input, thereby facilitating further operations on the game, such as data analysis and code debugging.
The function addresses the following issues:
1. Precise Access: No need to search through pages within the platform; simply enter the ID to reach the designated game.
2. Efficiency Improvement: Suitable for teaching/testing scenarios where the game ID is known, saving browsing time.
3. Development Assistance: Convenient for developers to quickly debug or analyze specific game resources.
II. Features of the Manual Game ID Input Function
1. Precise Direct Access
Directly locate the target game via the unique game ID, avoiding cumbersome searches within the platform and enhancing access efficiency.
2. Quick Operation
Integrated with the VSCode command panel (Ctrl+Shift+P), the function can be triggered by simply entering the command without switching interfaces.
3. Developer-Friendly
Supports rapid debugging and testing, enabling developers to directly access game resources through the ID to optimize workflows.
4. Flexible Compatibility
Compatible with the 4399 platform API, it is highly extensible and can be integrated with more games or customized functions in the future.
5. User Convenience
Simplifies operational steps, making it suitable for teaching, demonstration, and other scenarios to enhance 6. user experience.
(The function relies on the extension's API interface and network environment.)
The Manual Game ID Input function in 4399 on VSCode, with its efficient and precise locating capabilities, high operational flexibility, real-time validation feedback, strong usability, adaptability, and reliability, provides users with a convenient and efficient game development and debugging experience. It is an essential feature of the 4399 on VSCode tool.
III. Installation and Usage Guide for the Manual Game ID Input Function
1. Extension Installation
Ensure that the 4399 on VSCode extension is installed:
Bash
Install through the VSCode Extension Marketplace
code --install-extension dsy4567.4399-on-vscode
Alternatively, search for 4399 on VSCode in the VSCode Extension panel and install it.
2. Function Activation
After the extension is installed, the command is automatically registered in the activate function of VSCode, and no additional configuration is required.
You can view the registered commands with the following code:

// View registered commands (for debugging purposes)
vscode.commands.getCommands().then(commands => {
    console.log(commands.filter(cmd => cmd.startsWith('4399-on-vscode')));
});
3. Usage Steps
(1) Open the Command Palette
Press the shortcut key Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (Mac) and enter the command:
For standard Flash games: type 4399-on-vscode.get
For H5 web games: type 4399-on-vscode.get-h5-web-game
(2) Enter the Game ID
In the input box that appears, enter one of the following formats:
// Example 1: Directly enter the numeric ID
222735

// Example 2: Enter the complete URL
<url id="d0grk9mg0jb51fad5de0" type="url" status="parsed" title="Life Restart Simulator_Life Restart Simulator html5 game_4399h5 game-4399 mini-game" wc="1007">https://www.4399.com/flash/222735.htm</url>
The input box supports auto-completion of historical records (implemented through globalStorage).
(3) Run the Game
After entering and pressing Enter, the extension will call the core logic:
// Internal processing logic (simplified version)
const parsedId = parseId(userInput); // Extract the numeric ID
const gameUrl = `<url id="d0grk9mg0jb51fad5dcg" type="url" status="parsed" title="Latest Mini-games, Latest Web Games, Latest Single-player Mini-games, 2016 Popular Mini-games" wc="1423">https://www.4399.com/flash/</url> ${parsedId}.htm`;
play(gameUrl); // Load and run the game

4. Complete Invocation Code Example
If you need to call it programmatically, you can refer to the following code:
import * as vscode from 'vscode';

// Manually trigger game ID input
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
// Invocation example
manuallyInputGameId();

IV. Examples and Code Analysis of the Manual Game ID Input Function
1. Function Example Code

import * as vscode from 'vscode';

// Game launch function
function playGame(gameId: string) {
    const gameUrl = `<url id="d0grk9mg0jb51fad5dcg" type="url" status="parsed" title="Latest Mini-games, Latest Web Games, Latest Single-player Mini-games, 2016 Popular Mini-games" wc="1423">https://www.4399.com/flash/</url> ${gameId}.htm`;
    vscode.window.showInformationMessage(`Launching game: ${gameUrl}`);
    // Actual game loading logic...
}

// Manual game ID input function
export function activate(context: vscode.ExtensionContext) {
    let disposable = vscode.commands.registerCommand('game.inputId', async () => {
        // Pop up input box
        const gameId = await vscode.window.showInputBox({
            placeHolder: 'Enter 4399 Game ID (e.g., 222735)',
            prompt: 'Supports direct input of numeric ID or complete game URL',
            validateInput: text => {
                // Validate if the input is valid
                const idPattern = /^(\d+|https?:\/\/www\.4399\.com\/flash\/\d+\.htm)$/;
                return idPattern.test(text) ? null : "Enter a valid game ID or URL";
            }
        });

        if (gameId) {
            // Extract the pure numeric ID (handle URL or numeric input)
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

2. Code Analysis
Function Entry Point
Function Name: get
Description: This function handles the user's manual input of the game ID, specifically for game links starting with http(s)://www.4399.com/flash/.
Implementation: It is a property of the commands object and is registered as a VSCode command using vscode.commands.registerCommand. Users can invoke this function through the VSCode command panel.
Retrieve Historical Input Value
Function: globalStorage(ctx).get("id1")
Description: Retrieves the value associated with the key id1 from the global storage, which stores the game ID the user last entered.
Purpose: If the user has previously used this function and entered a game ID, this value will be retrieved to provide a default value in the input box, facilitating quick user operations.
Pop Up Input Box
Function: vscode.window.showInputBox
Description: Displays an input box to allow users to manually enter the game ID.
Configuration:
value: The initial value of the input box. If a historical input value i is retrieved from global storage, it is used as the initial value; otherwise, the default value is "222735".
title: The title of the input box, displayed as "4399 on VSCode: Enter Game ID", clearly indicating the current operation to the user.
prompt: The prompt message in the input box, informing the user that they can enter the game link or the numeric ID (game ID) following http(s)://www.4399.com/flash/, providing clear input guidance