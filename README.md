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
```

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