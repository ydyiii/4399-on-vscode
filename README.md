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