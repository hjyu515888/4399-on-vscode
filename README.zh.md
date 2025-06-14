
by何洁芳
自定义标题功能概述：
1. 本项目支持自定义 Webview 面板的标题。
2. 用户可以根据自己的需求设置独特的标题，提升使用体验。
3. 若未设置自定义标题，系统将依次使用游戏本身的标题或默认标题 "4399 on VSCode"。
   
使用方法：
步骤一：定位配置文件
打开项目根目录下的 `package.json` 文件。此文件负责管理项目的各种配置信息，自定义标题的配置也在此文件中进行设置。

步骤二：找到自定义标题配置项
在 `package.json` 文件里，找到 `configuration` 部分下的 `4399-on-vscode.title` 属性。该属性控制着 Webview 面板的标题显示。
```
"configuration": {
    // 整个配置块的标题，用于描述本组配置的用途
    "title": "4399 on VSCode",

    // 配置项的具体属性定义
    "properties": {

        // 用户代理（User-Agent）设置
        "4399-on-vscode.user-agent": {
            "description": "4399 on VSCode 发出的请求头中的 UA（User-Agent），用于模拟浏览器环境",
            "type": "string",
            "default": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36"
        },

        // 请求头中的 Referer 设置
        "4399-on-vscode.referer": {
            "description": "4399 on VSCode 发出的请求头中的 Referer，表示请求来源地址",
            "type": "string",
            "default": "https://www.4399.com/"
        },

        // 本地服务器监听端口号
        "4399-on-vscode.port": {
            "description": "4399 on VSCode 本地服务器端口，修改后必须重启 VSCode 才能生效",
            "type": "integer",
            "ignoreSync": true,  // 表示该设置不会被同步到其他设备（如通过 VSCode Settings Sync）
            "default": 44399     // 默认使用的端口号
        },

        // 是否打印扩展日志
        "4399-on-vscode.printLogs": {
            "description": "控制是否在开发工具中输出调试日志，用于排查问题",
            "type": "boolean",
            "default": false      // 默认不输出日志
        },

        // Webview 页面标题设置
        "4399-on-vscode.title": {
            "description": "控制 Webview 标题，若留空则使用默认标题：游戏名称 或 '4399 on VSCode'",
            "type": "string",
            "default": ""         // 默认为空，使用自动标题
        }

    }
}
```
步骤三： 将 "default" 字段的值修改为你期望的自定义标题，例如 "摸鱼必备"。
```
"4399-on-vscode.title": {
    "description": "控制 Webview 标题, 若留空则使用游戏名称或 4399 on VSCode",
    "type": "string",
    "default": "摸鱼必备"
}
```



by何洁芳
随即游戏功能概述：
“随机游戏” 功能为用户提供了在 4399 游戏平台上随机挑选一款游戏进行游玩的体验。由于游戏链接是随机生成的，所以可能会出现所选链接对应的游戏不存在或无法正常访问的情况。
使用步骤：
步骤1：启动 VSCode：打开 Visual Studio Code 编辑器。
步骤2：触发命令：按下 Ctrl + Shift + P（Windows/Linux）或 Cmd + Shift + P（Mac）打开命令面板。
步骤3：选择随机游戏：在命令面板中输入 4399-on-vscode.random 并回车，即可触发随机游戏功能。

核心代码：
随机游戏的核心代码位于 4399-on-vscode/src/extension.ts 文件中。
```
random: () => {
    // 调用 play 函数，传入一个随机生成的小游戏链接
    play(
        // 构建目标 URL：
        // 基础路径为 4399 的小游戏 flash 页面
        "https://www.4399.com/flash/" +
        
        // 使用 Math.random() 生成一个 0 到 1 之间的随机小数，
        // 乘以 10000 得到 0 到 10000 之间的数，
        // 再通过 Math.floor 向下取整得到整数，
        // 最后加上 200000，使最终范围为 200000 ~ 209999
        (Math.floor(Math.random() * 10000) + 200000) +

        // 拼接 .htm 扩展名，构成完整的游戏页面地址
        ".htm"
    );
}
```

#by 唐莺莺
项目介绍：

4399 on VSCode 是一个基于 Visual Studio Code 的扩展，旨在将 4399 平台上的小游戏带到您的开发环境中。这一扩展允许开发者在编码时通过玩小游戏来放松心情，并提高工作效率。以下是其主要功能：

核心功能
1.支持 Flash 和 H5 游戏：
能够运行 4399 平台上的 Flash 游戏（通过本地服务器模拟支持）。
支持 H5 网页小游戏，无需额外插件。

2.内嵌游戏窗口：
在 VSCode 内使用 Webview 面板直接加载游戏，方便快捷。

3.游戏收藏功能：
能够收藏常玩的小游戏，并快速访问。

4.历史记录：
自动记录游戏历史，方便用户回顾之前玩的游戏。

5.智能解析 4399 游戏页面：
自动从 4399 的游戏详情页提取游戏资源地址。
支持复杂的游戏资源解析规则，包括 Flash 和 H5 页游。

<!-- by 唐莺莺  -->

#by 唐莺莺
玩 h5 页游、H5 小游戏和部分 flash 小游戏。

游戏类型特征：
1、H5页游（网页游戏）
需要4399账号授权
通过iframe或专用API加载
典型URL：https://www.4399.com/flash/游戏ID.htm
2、H5小游戏
纯HTML5实现
无Flash依赖
路径包含.html/.htm
3、Flash小游戏
使用Ruffle模拟器
路径以.swf结尾

使用教程
步骤 
1、准备游戏链接
找到对应的 4399 游戏详情页链接，例如：
(1).H5 游戏链接：https://www.4399.com/h5game/123456.htm
(2).Flash 游戏链接：https://www.4399.com/flash/654321.htm
2、调用 play 方法
(1).使用项目
3、启动游戏后
(1).游戏会通过 WebView 面板展示。
(2).运行时会记录游戏历史。

使用示例：
1、启动 Flash 游戏：play("https://www.4399.com/flash/654321.htm");
2、启动 H5 游戏：play("https://www.4399.com/h5game/123456.htm");

以下是启动小游戏的核心代码逻辑：
/**
 * 启动小游戏
 * @param url 游戏详情页链接
 */
async function play(url: string) {
    if (url.startsWith("//")) url = "https:" + url;
    else if (url.startsWith("/")) url = "https://www.4399.com" + url;

    let res = await httpRequest.get(url, "arraybuffer");
    const $ = cheerio.load(iconv.decode(res.data as Buffer, "gb2312"));
    let $flash = $("iframe#flash22").attr("src");

    if ($flash) {
        let gameUrl = new URL($flash, "https://www.4399.com");
        console.log(`启动 Flash 游戏：${gameUrl}`);
    } else {
        console.log("启动 H5 游戏...");
    }
}


注意事项
1、环境要求：
(1).确保系统支持 TypeScript 和Node.js。
(2).如果需要运行 Flash 游戏，请确保本地支持 Flash。

<!-- by 唐莺莺  -->

by 张晓雯
### 1. 功能名称与简介
**功能名称**：4399 on VSCode - 逛群组功能

**功能简介**：“4399 on VSCode - 逛群组功能” 集成于 VSCode 扩展 “4399 on VSCode” 中，允许用户在 VSCode 开发环境内搜索、浏览和参与 4399 游戏相关的群组。用户可搜索感兴趣的群组，查看群组内的帖子，还能进行加入、离开群组以及群组签到等操作，为 4399 游戏爱好者提供便捷的交流途径。

### 2. 功能特点

#### 2.1 群组搜索与帖子查看
**功能描述**：
用户能在输入框输入关键词搜索 4399 的游戏群组，搜索结果以列表形式展示群组标题和 ID，且支持分页搜索。点击搜索结果中的群组，可进入该群组查看所有帖子，帖子列表会展示帖子标题、类型和 ID，点击帖子即可查看帖子详情。

**操作步骤**：
```typescript
// 步骤 1: 打开 “4399 on VSCode: 逛群组” 输入框
import { showForums } from './src/forums';
// 调用 showForums 函数打开逛群组输入框
showForums();

// 步骤 2: 在输入框中输入关键词，系统会延迟 1 秒发起请求获取搜索结果
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
 * 搜索群组
 * @param kwd 搜索词
 */
function searchForums(kwd: string) {
    if (threadQp.busy) return;

    clearTimeout(threadTimeout);
    log(`页码: ${threadPage}`);
    threadTimeout = setTimeout(async () => {
        threadQp.busy = true;
        let res;
        try {
            res = (await httpRequest.get(
                API_URLS.search(kwd),
                "arraybuffer"
            )) as AxiosResponse<Buffer | string>;
        } catch (e) {
            return err("获取搜索建议失败", String(e));
        }
        if (!res.data) return err("获取搜索建议失败");

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
                description: `群组 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    // 这里可以添加点击群组后的处理逻辑
                },
                data: {
                    id,
                    title: g
                }
            });
        });

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
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

// 初始化输入框
threadQp = createQuickPick({
    title: "4399 on VSCode: 搜索群组",
    prompt: "输入搜索词"
});
threadQp.onDidChangeValue(kwd => {
    searchForums(kwd);
});
threadQp.show();
```

**代码解释**：
- **输入框监听**：通过 `threadQp.onDidChangeValue` 监听输入框内容的变化，当输入框内容改变时，调用 `searchForums` 函数。
- **延迟请求**：在 `searchForums` 函数中，使用 `setTimeout` 延迟 1 秒发起请求，同时使用 `clearTimeout` 清除之前的定时器，以避免多次请求。
- **请求处理**：使用 `httpRequest.get` 发起请求，获取搜索结果，并使用 `cheerio` 解析 HTML 内容。
- **结果展示**：将搜索结果添加到 `threadQpItems` 中，并更新输入框的 `items` 属性。

```typescript
// 步骤 3: 浏览搜索结果，点击感兴趣的群组，进入该群组的帖子列表页面

// 步骤 4: 在帖子列表页面，浏览帖子标题和类型，点击帖子可查看帖子详情
/**
 * 显示所有帖子
 * @param id 群组 ID
 * @param title 群组名
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`群组 ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // 获取标题和类型
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[顶] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `帖子 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("帖子 ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `群组: ${title}`;
        }
        showingThreads = true;

        // 这里可以添加后续处理逻辑
    } else {
        err("无法获取群组页面");
    }
}
```

**代码解释**：
- **获取帖子详情数据**：通过 `httpRequest.get` 方法请求指定帖子的详情页面，使用 `cheerio` 解析 HTML 内容。
- **预处理 HTML**：对 HTML 中的图片链接进行处理，修改协议和解除防盗链限制，并移除一些不必要的元素。
- **提取从帖内容**：遍历 HTML 中的从帖元素，提取从帖的作者、标题和正文，并将这些内容拼接成 HTML 字符串。
- **生成文章 HTML**：将主帖和从帖的内容拼接成完整的文章 HTML，并进行一些替换操作。
- **显示帖子详情**：使用 `vscode.window.createWebviewPanel` 创建一个 Webview 面板，将生成的文章 HTML 显示在面板中。

#### 2.2 群组操作（加入 / 离开群组、签到）
**功能描述**：在群组帖子列表页面，用户可根据自身需求选择加入或离开群组，还能进行群组签到操作。加入或离开群组以及签到操作会向服务器发送请求，并根据返回结果提示用户操作是否成功。

```typescript
/**
 * 显示所有帖子
 * @param id 群组 ID
 * @param title 群组名
 */
async function showThreads(id: number, title: string) {
    if (threadQp.busy) return;

    threadQpItems = [];
    threadQp.busy = true;

    log(`群组 ID: ${id}`);
    const d: Buffer = (await httpRequest.get(API_URLS.forum(id), "arraybuffer")).data;

    if (d) {
        const $ = cheerio.load(d);
        let joined = false;

        // 获取标题和类型
        $("div.listtitle > div.title").each((i, elem) => {
            const $title = $(elem).children("a.thread_link");
            const id = Number($title.attr("href")?.split("-").at(-1));
            let title = $title.text();
            let type = $(elem).children("a.type").text();
            if (!id || isNaN(id) || !title) return;

            type = type || "[顶] ";
            title = type + title;
            threadQpItems.push({
                label: title,
                description: `帖子 ID: ${id}`,
                alwaysShow: true,
                action(target) {
                    enterThread(
                        +(target.description?.replace("帖子 ID: ", "") || -1)
                    );
                },
                data: { id, title },
            });
        });
        joined = $("a.join").hasClass("hasjoin");

        threadQpItems.push({
            label: "下一页",
            description: `第 ${threadPage} 页`,
            alwaysShow: true,
            action() {
                threadPage++;
                showThreads(forumId, forumTitle);
            },
        });

        if (threadQpItems[0]) {
            threadQp.items = threadQpItems;
            threadQp.title = `群组: ${title}`;
        }
        showingThreads = true;
        threadQp.buttons = [
            vscode.QuickInputButtons.Back,
            {
                tooltip: joined ? "离开群组" : "加入群组",
                iconPath: joined
                   ? new vscode.ThemeIcon("remove")
                    : new vscode.ThemeIcon("add"),
                async action(button) {
                    let result: any;
                    switch (button.tooltip) {
                        case "离开群组":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.leave(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code !== 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "操作失败"
                                );
                            }
                            break;
                        case "加入群组":
                            result = (
                                await httpRequest.post(
                                    ...API_URLS.join(forumId),
                                    "json"
                                )
                            ).data;
                            if (result?.code === 100) {
                                vscode.window.showInformationMessage(
                                    result?.msg || "加入成功"
                                );
                            } else {
                                vscode.window.showInformationMessage(
                                    result?.msg || "操作失败"
                                );
                            }
                            break;
                    }
                    showThreads(forumId, forumTitle);
                },
            },
            {
                tooltip: "签到",
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
                            `签到成功, 已连续签到 ${+result?.result?.totalDays} 天`
                        );
                    } else {
                        vscode.window.showInformationMessage(
                            result?.msg || "操作失败"
                        );
                    }
                    showThreads(forumId, forumTitle);
                },
            },
        ];
        threadQp.busy = false;
    } else {
        err("无法获取群组页面");
    }
}
```

**代码解释**：
- **获取群组帖子列表**：使用 `httpRequest.get` 方法请求指定群组的帖子列表页面，使用 `cheerio` 解析 HTML 内容。遍历 HTML 中的帖子元素，提取帖子的标题、类型和 ID，并将这些信息存储在 `threadQpItems` 数组中。
- **判断是否已加入群组**：通过检查 HTML 中的 `a.join` 元素是否包含 `hasjoin` 类来判断用户是否已加入群组。
- **添加操作按钮**：在 `threadQp.buttons` 中添加了三个按钮：返回按钮、加入 / 离开群组按钮和签到按钮。
- **加入或离开群组操作**：当用户点击加入 / 离开群组按钮时，根据按钮的 `tooltip` 属性判断用户的操作类型。使用 `httpRequest.post` 方法向服务器发送加入或离开群组的请求，并根据返回结果提示用户操作是否成功。
- **签到操作**：当用户点击签到按钮时，使用 `httpRequest.post` 方法向服务器发送签到请求，并根据返回结果提示用户签到是否成功。
- **更新页面**：在每次操作完成后，调用 `showThreads` 函数更新页面，显示最新的群组信息和帖子列表。

**操作步骤**：
1. 进入群组帖子列表页面。
2. 若当前未加入群组，点击 “加入群组” 按钮，系统会向服务器发送加入请求，根据返回结果提示操作是否成功。
3. 若当前已加入群组，点击 “离开群组” 按钮，系统会向服务器发送离开请求，根据返回结果提示操作是否成功。
4. 点击 “签到” 按钮，系统会向服务器发送签到请求，若签到成功，会显示连续签到天数。

### 3. “逛群组” 安装步骤
1. 确保安装 Node.js 和 npm。
2. 克隆项目：
```bash
git clone https://github.com/dsy4567/4399-on-vscode.git && cd 4399-on-vscode
```
3. 安装依赖：
```bash
npm install
```
4. 编译代码：
```bash
npm run compile
```
5. （可选）配置开发环境。
6. 启动项目：在 VSCode 中按 <kbd>F5</kbd> 或执行相应启动命令。

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

<!-- by 赵文佳 -->
（一）搜索游戏的核心功能模块的简单介绍以及代码定位

1.  游戏搜索入口（searchGames）

游戏搜索入口简介:初始化搜索交互界面（VSCode QuickPick），接收用户输入的关键词，触发搜索建议或直接执行搜索。支持默认搜索词填充、界面状态管理（如分页按钮显示）和用户操作监听。

代码定位: async function searchGames(s: string)

2. 执行搜索（searchByKwd）

简介:根据关键词调用 4399 搜索接口，解析返回的 HTML 页面，提取游戏名称、ID 等信息，并生成可操作的 QuickPick 列表项（含“下一页”分页功能）。

代码定位: async function searchByKwd(s: string)


3. 联想词建议（suggest）

简介:提供实时搜索联想词功能，通过防抖机制（延迟 1 秒）调用 4399 建议接口，解析 JSON 数据并展示建议列表。用户选择建议词可直接跳转至对应游戏。

代码定位: async function suggest(kwd: string)

4.  API 地址管理（API_URLS）

简介:定义 4399 搜索相关接口的 URL 模板，包括：搜索结果分页接口（result）：支持动态关键词和页码；搜索建议接口（suggestion）：支持动态关键词。

代码定位: const API_URLS 常量。


5. 分页与界面状态管理

简介:管理当前搜索的页码（searchPage）、搜索输入框历史值（searchValue）及界面状态（如是否正在展示结果）。

关键代码定位:

searchPage: number：记录当前页码。

showingSearchResult: boolean：标记是否正在展示搜索结果。

searchValue: string：保存用户输入的搜索词。

6. 异常捕获与日志记录

简介:捕获网络请求、数据解析等异常，通过 err 函数输出错误信息，并通过 log 记录调试信息，保障搜索流程的健壮性。

代码定位:

try...catch 块包裹网络请求。

err("无法获取搜索页: ", e) 和 log("成功获取到4399搜索页面") 调用。

<!-- by 赵文佳 -->
（二）搜索功能模块协作流程图
用户输入 → searchGames() → 触发 suggest()（联想词）或 searchByKwd()（直接搜索）
                │
                ├─→ suggest() → 调用 API_URLS.suggestion → 解析建议词 → 填充 QuickPick
                │
                └─→ searchByKwd() → 调用 API_URLS.result → 解析 HTML → 生成结果列表
                                      │
                                      └─→ 用户点击“下一页” → searchPage++ → 重新调用 searchByKwd()

<!-- by 赵文佳 -->
（三）搜索功能模块的使用教程

一、安装插件
方法一：通过 VSCode 扩展商店安装：
首先打开 VSCode，点击左侧边栏的 Extensions 图标（或按下 Ctrl+Shift+X）；其次搜索 4399 on VSCode，找到插件后点击 Install；
在安装完成后，点击 Reload 重启 VSCode 激活插件。

方法二：手动安装：
从 GitHub 仓库下载 .vsix 文件
'''git clone https://github.com/dsy4567/4399-on-vscode.git
cd 4399-on-vscode
vsce package'''
完成后在 VSCode 中，通过 Extensions 面板右上角菜单选择 Install from VSIX，选择生成的 .vsix 文件。

二、启动搜索功能
打开搜索界面，按下 Ctrl+Shift+P 打开命令面板；输入命令 4399: 搜索游戏 并按回车。此时会弹出 QuickPick 输入框，标题为 4399 on VSCode: 进行搜索。

三、基础操作
1. 输入关键词搜索
在输入框中输入游戏名称（如 黄金矿工）。
直接搜索：按回车键，插件会调用搜索接口并展示结果列表。
实时建议：输入过程中会显示联想词（如 黄金矿工双人版），点击联想建议词可直接跳转至对应游戏。

2. 浏览搜索结果
搜索结果以列表形式展示，包含 游戏名称 和 游戏 ID（如 游戏 ID: 12345）。
点击游戏名称后：插件会自动调用 play 函数，在内嵌浏览器中打开游戏页面；输入框关闭，最后一次搜索词会保存到全局存储中。

3. 翻页功能
列表底部有 下一页 按钮，点击可加载更多搜索结果，当前页码显示在按钮描述中（如 第 1 页），翻页后，输入框标题会更新为 关键词 的搜索结果（第 N 页）。

4. 返回搜索建议
若在搜索结果界面点击左上角的 返回按钮（←），输入框会切换回搜索建议模式，清空当前结果并显示联想词。

四、其余高级功能
1. 历史记录存档
最后一次成功搜索的关键词会通过 globalStorage 自动保存，下次打开搜索界面时，输入框会自动填充该关键词。
2. 错误处理
网络异常：若搜索请求失败，插件会通过 VSCode 通知栏显示错误信息（如 无法获取搜索页: 超时）。


<!-- by 赵文佳 -->
（四）搜索功能特点详解：

一、深度集成 VSCode 特性
 1. 原生搜索交互界面
详细代码：
```typescript
// 使用 VSCode QuickPick 实现类搜索引擎的交互
searchQp = createQuickPick({
  title: "4399 on VSCode: 搜索",
  prompt: "输入搜索词"
});
```
特点：  
1.完全融入 VSCode UI 风格
2.支持键盘快速导航（↑↓方向键选择，Enter确认）
3.内置加载状态指示（`busy` 属性）

2. Webview 游戏容器
详细代码：
```typescript
// 在 VSCode 内嵌浏览器中运行游戏
const panel = vscode.window.createWebviewPanel(
  "4399-game", 
  "4399小游戏", 
  vscode.ViewColumn.One, 
  { enableScripts: true }
);
panel.webview.html = `<iframe src="${url}"></iframe>`;
```
特点：  
1.无需离开编辑器即可玩游戏
2.支持 Flash/HTML5 游戏（取决于浏览器兼容性）
3.多标签页支持（可同时打开多个游戏）


二、智能搜索功能
1. 多级搜索建议
详细代码：
```typescript
// 输入关键词时动态获取建议
suggestions = JSON.parse(decodedData.split(" =")[1].replaceAll("'", '"'));
```
特点：  
1.输入即时反馈（1秒防抖延迟）
2.建议项包含精准游戏 ID
3.支持键盘快速选择建议项

2. 分页搜索系统
详细代码：
```typescript
// 搜索结果分页处理
searchQpItems.push({
  label: "下一页",
  action() { 
    searchPage++;
    searchByKwd(searchQp.value); 
  }
});
```
特点：  
1.无限滚动式分页加载
2.页码状态持久化（`searchPage` 全局变量）
3.智能结果去重（每次搜索重置列表）


三、数据处理核心
1.GB2312 编码自动转换
详细代码：
```typescript
// 处理中文编码兼容性问题
res.data = iconv.decode(res.data as Buffer, "gb2312");
```
特点：  
1.解决 4399 接口返回非 UTF-8 编码问题
2.支持完整中文字符显示


四、用户个性化体验
1. 搜索历史记忆 
详细代码：
```typescript
// 保存最后一次有效搜索词
globalStorage(getContext()).set("kwd", searchQp.value);
```
特点：  
1.使用 VSCode 全局存储（`Memento` API）
2.重启编辑器后仍可恢复上次搜索

<!-- by 杜昱蓉 -->

#by  杜昱蓉

**安装步骤**：
(1)安装 VSCode：确保你的系统已安装 Visual Studio Code（VSCode），若未安装，可从官网（https://code.visualstudio.com/）下载并安装。
(2)准备开发环境：
安装 Node.js（建议版本 14.x 及以上）。
安装 npm（Node.js 包管理器，通常随 Node.js 一起安装）。

**部署步骤**:
(1)注册 4399 账号：若没有 4399 账号，需先在官网（https://www.4399.com）注册。
(2)获取 Cookie（可选）：
打开浏览器，登录 4399 账号。
访问 4399 任意页面，按 F12 打开开发者工具，切换到 Application（Chrome）或 Storage（Firefox）选项卡。
在 Cookies 中找到 4399 相关的 Cookie，复制完整的 Cookie 信息备用。
(3)VSCode 中配置扩展：
安装扩展后，在 VSCode 中按Ctrl+Shift+P（Windows/Linux）或Cmd+Shift+P（Mac）打开命令面板。
输入 “4399”，可看到扩展提供的命令，如 “4399: 登录”“4399: 签到” 等。
首次使用需先登录，可选择账号密码登录或使用之前获取的 Cookie 登录。

**注意事项**：
(1)网站结构变化：
该扩展通过爬取 4399 网站内容实现功能，若 4399 网站的结构发生变化，可能导致部分功能失效。请留意扩展开发者在 GitHub 仓库（https://github.com/dsy4567/4399-on-vscode）发布的更新信息。
(2)账号安全：
在使用扩展过程中，需遵守 4399 网站的相关规定，避免过度请求，防止账号因异常操作而出现问题。同时，不要将端口（如本地服务器端口）的可见性设为 Public（公共），以免导致 Cookie 泄露，造成账号安全风险。
(3)远程开发环境：
如果在 GitHub CodeSpaces 或其他远程开发环境中使用该扩展，可能会遇到游戏无法加载或图裂的情况。此时，需要按照扩展的提示完成验证（点击 “去验证” 按钮，访问指定链接），并且不要将端口设为公共可见。
(4)快捷键使用：
为使快捷键能够正常使用，在使用快捷键前需要使游戏失去焦点。如果不希望每次都显示该提示，可以在扩展的配置中关闭 “alert” 选项（默认开启）。
(5)配置管理：
扩展提供了一些配置选项，可以通过getCfg、rmCfg、setCfg函数来获取、移除和设置配置。在进行配置更改时，需谨慎操作，以免影响扩展的正常使用。。

**登陆账号功能**：
**核心功能**：
1.登录认证
(1)支持两种登录方式：账号密码登录和 Cookie 登录。
(2)使用httpRequest.post发送登录请求，并解析响应中的 Cookie 信息。
(3)将用户 Cookie 存储在 VSCode 的secrets存储中，确保安全性。
2.Cookie 管理
(1)getCookie()：异步获取存储的 Cookie。
(2)getCookieSync()：同步获取 Cookie（若未加载则触发异步加载）。
(3)setCookie()：设置并保存 Cookie 到 VSCode 存储。
3.用户信息
getUid()：从 Cookie 中解析用户 ID（Pauth字段）。
4.签到功能
sign()：调用 4399 签到 API，支持静默签到（不显示通知）。
5.个人中心
my()：显示用户个人中心菜单，支持查看收藏、历史记录、推荐游戏等。

**代码结构解析**：
**API接口定义**：
{{
/** 接口地址 **/
const API_URLS = {
    /** 登录 **/
    login: (username: string, password: string) =>
        [
            "https://ptlogin.4399.com/ptlogin/login.do?v=1",  // 登录API的URL
            `username=${username}&password=${password}`,  // 登录表单数据
        ] as const,
    /** 签到 **/
    get sign() {
        // 生成带时间戳的签到URL，防止缓存
        return "https://my.4399.com/plugins/sign/set-t-" + new Date().getTime();
    },
};

let COOKIE: string;  // 全局变量存储当前用户的Cookie
}}

**登录流程**：
1.检查登录状态：通过getCookieSync()判断用户是否已登录。
{{
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
}}
2.选择登录方式：
(1)账号密码登录：发送表单请求到 4399 登录接口，解析响应中的 Cookie。
{{
 const user = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用账号密码)",
                prompt: "请输入 4399 账号",
            });}}
(2)Cookie 登录：直接使用用户提供的 Cookie 字符串。
            {{
            let c = await vscode.window.showInputBox({
                title: "4399 on VSCode: 登录(使用 cookie)",
                prompt: "请输入 cookie, 获取方法请见 GitHub wiki 页",
            });}}
3.验证与存储：确保 Cookie 包含必要的Pauth字段，然后加密存储。 

**签到流程**:
1.调用login()确保用户已登录。
 login(async () => {  // 确保用户已登录
2.发送签到请求到API_URLS.sign。
// 发送签到请求
            {{(
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
            } = (await httpRequest.get(API_URLS.sign, "json")).data;)}}
3.解析响应结果（成功 / 失败 / 连续签到天数）。
{{
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
        }}}
        
        
个人中心流程:
1.显示用户昵称（从 Cookie 中解析Pnick字段）。
{{{
// 从Cookie中解析用户昵称
        let Pnick = cookie.parse(c)["Pnick"] || "未知";
        Pnick = Pnick === "0" ? "未知" : Pnick;
}}}
2.提供多个功能选项：
(1)我的收藏：获取并展示用户收藏的游戏列表。
(2)猜你喜欢：获取推荐游戏。
(3)我玩过的：获取历史记录。
(4)签到：触发签到功能。
(5)退出登录：清除 Cookie 并登出。
{{
// 显示个人中心菜单
        const value = await vscode.window.showQuickPick([
            "🆔 昵称: " + Pnick,
            "❤️ 我的收藏盒",
            "✨ 猜你喜欢",
            "🕒 我玩过的",
            "🖊 签到",
            "🚪 退出登录",
        ]);
    }}
关键技术细节:
1.字符编码处理
使用iconv-lite解码 GBK 编码的响应内容（4399 网站使用 GBK 编码）。
2.错误处理
(1)通过err()函数显示错误信息，并记录日志。
(2)处理网络请求失败、Cookie 无效等异常情况。
3.异步操作
使用Promise处理异步请求，确保代码流程正确。
4.安全措施
(1)使用 VSCode 的secrets存储敏感信息（Cookie）。
(2)对用户输入进行验证（如检查 Cookie 是否包含Pauth字段）
