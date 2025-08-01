# QLaunch – Windows 快捷命令助手

> 双击 `Ctrl` 呼出面板，输入几个字符即可启动程序、打开目录、调用工具、粘贴模板……  
> 支持拼音搜索、使用频度排序、剪贴板交互、Everything 集成、Total Commander 集成等。

---

## 1. 快速上手

| 场景            | 示例                                                         |
| --------------- | ------------------------------------------------------------ |
| 打开软件        | 输入 `chrome` → Enter                                        |
| 拼音搜索        | 输入 `:py:huoq` 可匹配「获取」开头的命令                     |
| Everything 搜索 | 输入 `:es:readme.md`                                         |
| 粘贴固定文本    | 输入 `:kv:ipGMS` → Enter（将 `172.16.46.73` 写进剪贴板并自动粘贴） |
| 计算器          | 输入 `#3*7+2` → Enter，结果弹窗并自动复制                    |

---

## 2. 面板激活 / 隐藏

| 方式          | 说明                                          |
| ------------- | --------------------------------------------- |
| **双击 Ctrl** | 显示 / 隐藏主面板（默认热键，可在源码中调整） |
| 托盘左键      | 显示面板                                      |
| 托盘右键      | 弹出菜单                                      |
| 失焦自动隐藏  | 点击主窗口以外区域自动隐藏                    |

---

## 3. 内部命令大全

| 命令                          | 说明                                    |
| ----------------------------- | --------------------------------------- |
| `:quit`                       | 退出 QLaunch                            |
| `:help`                       | 帮助                                    |
| `:reload`                     | 重新加载所有配置                        |
| `:py:` *关键词*               | 拼音搜索                                |
| `:kv:` *关键词*               | 搜索 key-value 条目                     |
| `:es:` *关键词*               | Everything 搜索文件                     |
| `:tcl` / `:tcr`               | Total Commander 左/右窗格打开剪贴板路径 |
| `:esFolder`                   | Everything 仅搜索文件夹                 |
| `:esExe`                      | Everything 仅搜索可执行文件             |
| `:esZip`                      | Everything 仅搜索压缩文件               |
| `:esDoc`                      | Everything 仅搜索文档                   |
| `:esImg`                      | Everything 仅搜索图片                   |
| `:esVideo`                    | Everything 仅搜索视频                   |
| `:esAudio`                    | Everything 仅搜索音频                   |
| `:encodeBase64`               | 对剪贴板文本进行 Base64 编码            |
| `:decodeBase64`               | 对剪贴板文本进行 Base64 解码            |
| `:urlEncode`                  | 对剪贴板文本进行 URL 编码               |
| `:urlDecode`                  | 对剪贴板文本进行 URL 解码               |
| `:formatJson`                 | 格式化剪贴板中的 JSON 字符串            |
| `:2txt`                       | 将剪贴板内容转为纯文本并自动粘贴        |
| `:setclip` *文本*             | 将给定文本写入剪贴板并自动粘贴          |
| `:fileclip` *文件路径*        | 将文件完整内容读入剪贴板                |
| `:clearclip`                  | 清空剪贴板                              |
| `:showfile` *文件路径*        | 弹出窗口显示文本文件内容                |
| `:mdview` *markdown 文件路径* | 打开 Markdown 预览窗口                  |
| `:msgbox` *文本*              | 弹出信息提示框                          |
| `:open` *路径*                | 使用系统默认方式打开文件/目录/URL       |
| `:execmd` *命令行*            | 执行任意命令行并回显结果                |
| `#表达式`                     | 计算器（示例 `#(2+3)*4`）               |

---

## 4. 配置文件

所有配置使用 UTF-8 纯文本，支持 `#` 或 `;` 注释，支持 `!include` 嵌套。

### 4.1 命令文件（`cmds.conf` 及其子文件）

**格式**  

```
关键字[,别名…]	描述	要执行的命令
```

在一些内部命令或外部工具软件前加上`@`字符，则会实现后台执行，不现实信息窗口。

**示例**  

```
chrome	Google Chrome	C:\Program Files\Google\Chrome\Application\chrome.exe
notepad++	编辑器	"C:\Program Files\Notepad++\notepad++.exe" "%*"
exampleExePs	exeps命令举例	:@exeps "c:\example.ps1"
```

**特殊占位符**  

| 占位符         | 运行时替换为         |
| -------------- | -------------------- |
| `%TOTALCMD%`   | Total Commander 路径 |
| `%EVERYTHING%` | Everything 路径      |
| `{%c}`         | 当前剪贴板文本       |
| `{%wd}`        | 最近一次活动窗口句柄 |

### 4.2 key-value 文件（`kv.conf` 及其子文件）

**格式**  
```
键[,别名…]	描述(可选)	要写入剪贴板的文本
```

**示例**  
```
ipGMS	云主机-GMS	172.16.46.73
pathGMSLog	GMS日志目录	/usr/local/df/gmsg/log/
```

使用时在面板输入 `:kv:ipGMS` 即可把 `172.16.46.73` 写进剪贴板并自动粘贴到当前窗口。

---

## 5. 文件结构

```
QLaunch\
├─ QLaunch.exe          主程序
├─ QLaunch.ini          主配置文件（窗口大小、路径等）
├─ cmds.conf            命令入口文件
│   ├─ command.conf     常用软件
│   ├─ info.conf        剪贴板模板、代码片段
│   └─ work.conf        工作相关
├─ kv.conf              k-v 入口文件
│   ├─ devops.conf      主机、路径信息
│   └─ password.conf    口令（建议加密存储）
├─ QLaunch.data         命令使用频度统计
├─ QLaunch.log          运行日志（可选）
└─ README.md            说明文件
```

---

## 6. 主配置 (QLaunch.ini)

| Section | Key        | 说明                   |
| ------- | ---------- | ---------------------- |
| Config  | CmdFile    | 主命令文件路径         |
| Config  | KvFile     | 主 k-v 文件路径        |
| Config  | MaxRows    | 面板最大显示行数       |
| Config  | RowHeight  | 每行高度（DPI 自适应） |
| Config  | AutoRun    | 是否开机启动 (0/1)     |
| Tools   | TotalCmd   | Total Commander 路径   |
| Tools   | Everything | Everything 路径        |
| Tools   | nircmd     | NirCmd 路径            |
| Tools   | WinCtl     | WinCtl 路径            |

---

## 7. 高级技巧

1. **多别名**  

   ```
   specChRight,specChDuiHao	对号✔	:setclip ✔
   ```
   输入 `right`、`duihao` 均可触发。

2. **Everything 过滤器**  
   内置 `:esExe`、`:esFolder`、`:esDoc` 等命令，分别对应 Everything 的「可执行文件」「文件夹」「文档」过滤器。

3. **剪贴板二次加工**  
   命令里出现 `{%c}` 时，QLaunch 会把当前剪贴板内容嵌入，实现「复制 → 处理 → 粘贴」一条龙。

4. **计算器结果**  
   计算结果弹窗同时已写入剪贴板，可直接粘贴。

5. **在 TOTALCMD 中快速定位命令中的文件**
   在命令选择界面选中某条命令，按 `Ctrl + Enter` / `Ctrl + L` 或 `Ctrl + Shift + Enter` / `Ctrl + R` 则在 TOTALCMD 的左右面板中快速打开文件所在位置

6. **查看命令配置**
   在命令选择界面选中某条命令，按 `F1` 键可以快速查看当前命令的配置

7. **复制命令配置**
   在命令选择界面选中某条命令，按 `Ctrl + 0` / `Ctrl + 1` / `Ctrl + 2` / `Ctrl + 3` 键，则可以分别将当前命令的完整配置、命令名称、命令描述或命令复制到剪贴板

---

## 8. 构建 / 二次开发

- [项目主页：softice70/QuickLaunch](https://github.com/softice70/QuickLaunch)
- 使用 **aardio** 10 以上版本打开 `main.aardio` → F7 即可生成独立 EXE。  
- 所有主逻辑均位于 `main.aardio`，纯脚本无需额外依赖（除调用的外部工具外）。

---

## 9. 常见问题

| 现象                 | 解决                                           |
| -------------------- | ---------------------------------------------- |
| 双击 Ctrl 无响应     | 检查是否被安全软件拦截热键                     |
| 中文输入法抢占焦点   | 程序已自动关闭中文输入法，如仍有问题可手动切换 |
| 配置文件修改后不生效 | 输入 `:reload` 或重启程序                      |

---

## 10. License

MIT – 可自由修改、分发，保留原作者信息即可。