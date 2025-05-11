by何洁芳
Custom Title Feature Overview
This project supports customizing the title of the Webview panel.
Users can set a custom title according to their preferences, thereby enhancing the user experience.
If no custom title is configured, the system will first attempt to use the game's native title, and if unavailable, fall back to the default title: "4399 on VSCode".
Usage Instructions
Step 1: Locate the Configuration File
Open the package.json file located in the root directory of the project. This file contains various configuration settings for the project, including the setting for the custom Webview title.

Step 2: Find the Custom Title Configuration
Within the package.json file, locate the 4399-on-vscode.title property under the configuration section. This property controls the display title of the Webview panel.
```
""configuration": {
    // The title of this configuration group, describing its purpose
    "title": "4399 on VSCode",

    // Definitions of individual configuration properties
    "properties": {

        // User-Agent settings
        "4399-on-vscode.user-agent": {
            "description": "User-Agent used in request headers by 4399 on VSCode to simulate a browser environment",
            "type": "string",
            "default": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36"
        },

        // Referer header setting
        "4399-on-vscode.referer": {
            "description": "Referer used in request headers, indicating the source URL",
            "type": "string",
            "default": "https://www.4399.com/"
        },

        // Local server port setting
        "4399-on-vscode.port": {
            "description": "Port number used by the local server in 4399 on VSCode; must restart VSCode after changing",
            "type": "integer",
            "ignoreSync": true,
            "default": 44399
        },

        // Log output control
        "4399-on-vscode.printLogs": {
            "description": "Controls whether debug logs are printed in the developer tools",
            "type": "boolean",
            "default": false
        },

        // Webview title setting
        "4399-on-vscode.title": {
            "description": "Controls the Webview title; defaults to the game name or '4399 on VSCode' if left empty",
            "type": "string",
            "default": ""
        }

    }
}
```
Step 3: Modify the Default Title
Change the value of the default field to your desired custom title. For example:"摸鱼必备"
```
"4399-on-vscode.title": {
            "description": "Controls the Webview title; defaults to the game name or '4399 on VSCode' if left empty",
            "type": "string",
    "default": "摸鱼必备"
}
```