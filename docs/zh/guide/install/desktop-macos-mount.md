---
title: macOS 挂载教程
icon: fa-brands fa-apple
order: 6.1
category:
  - Guide
tag:
  - Install
  - Desktop
  - macOS
comment: false
---

# AList 在 macOS 上挂载卷

本教程介绍如何使用 AList Desktop，将 AList 中的目录挂载到 Mac 本地文件夹。完成后，你可以直接在 Finder 中浏览和管理文件，使用体验与普通本地磁盘类似。

## 先分清楚：这是两个不同方向

教程中包含两种容易混淆的操作，它们不是同一件事：

| 操作位置 | 数据方向 | 需要填写的字段 |
| --- | --- | --- |
| AList Desktop 的“挂载”页面 | AList 目录 → Mac Finder | 源路径、目标路径 |
| AList 后台的“本机存储” | Mac 本地文件夹 → AList 网页 | 挂载路径、根文件夹路径 |

可以这样理解：

~~~text
AList Desktop 挂载：
AList 的 /test → /Users/testerbugo/wwtest → Finder 中查看

AList 后台本机存储：
Mac 的某个真实文件夹 → AList 的 /test → 浏览器中查看
~~~

> 如果你只想把已有的 AList 目录挂载到 Finder，完成第 1～6 步即可，不需要再添加“本机存储”。第 7 步只适用于还需要把 Mac 本地文件夹开放到 AList 网页的情况。

## 操作总览

整个过程可以分为 7 步：

1. 准备 AList Desktop、macFUSE 和本地文件夹。
2. 打开 AList Desktop 的挂载页面。
3. 新建挂载配置，填写源路径和目标路径。
4. 保存配置并启动挂载。
5. 根据 macOS 提示允许 macFUSE 系统扩展。
6. 检查挂载状态，确认目录可以访问。
7. 可选：在 AList 后台添加本机存储，把 Mac 文件夹显示到网页中。

---

## 1. 完成准备工作

开始前，请依次确认：

1. AList Desktop 已经安装并能正常打开。
2. AList 服务正在运行，管理页面可以正常访问。
3. Mac 已经安装 macFUSE；如果尚未安装，可稍后根据 AList Desktop 页面提示完成安装。
4. 当前 macOS 账户拥有管理员权限，能够批准系统扩展。
5. 已确定要挂载的 AList 目录和 Mac 本地目标文件夹。

本文使用以下路径作为示例：

~~~text
AList 源路径：/test
Mac 目标路径：/Users/testerbugo/wwtest
~~~

> 示例中的用户名和目录仅用于演示。实际操作时，请替换成你自己的用户目录和挂载路径。

## 2. 进入 AList Desktop 挂载页面

1. 打开 AList Desktop。
2. 等待 AList Desktop 完成启动。
3. 点击顶部导航中的“挂载”。
4. 确认页面中可以看到“新建”按钮和 macFUSE 提示。

![AList Desktop 挂载页面](/img/desktop/mount/macos/01.png)

首次使用时，请根据页面提示安装 macFUSE。macFUSE 负责把 AList 目录转换为 macOS 可以识别的文件系统挂载卷。

如果已经安装过 macFUSE，可以直接进入下一步。

## 3. 新建挂载配置

1. 在挂载页面点击“新建”。
2. 在“源路径”中填写 AList 网页里的目录。
3. 在“目标路径”中填写 Mac 本地文件夹。
4. 根据需要选择是否启用“启动时自动挂载”。
5. 缓存模式和自定义参数不确定时，可以先保持默认设置。

![新建挂载配置](/img/desktop/mount/macos/02.jpg)

### 3.1 区分源路径和目标路径

| 配置项 | 示例 | 实际含义 |
| --- | --- | --- |
| 源路径 | /test | AList 网页中已经存在的目录 |
| 目标路径 | /Users/testerbugo/wwtest | Mac 本地用于显示挂载内容的文件夹 |

可以简单理解为：

~~~text
AList 中的 /test
        ↓
挂载到 Mac
        ↓
/Users/testerbugo/wwtest
~~~

### 3.2 创建目标文件夹

“目标路径”必须是 Mac 上已经存在的文件夹。如果文件夹不存在，请先创建。

1. 打开 Finder，进入自己的用户目录。
2. 新建一个用于挂载的文件夹，例如 wwtest。
3. 或者打开终端，执行：

~~~bash
mkdir -p "$HOME/wwtest"
~~~

4. 创建后，可以执行下面的命令确认目录存在：

~~~bash
ls -ld "$HOME/wwtest"
~~~

如果目标路径不存在，挂载时通常会出现以下错误：

~~~text
failed to retrieve mount path information:
stat /Users/testerbugo/wwtest: no such file or directory
~~~

### 3.3 填写路径时的注意事项

1. 源路径填写 AList 中的目录，例如 /test。
2. 目标路径填写 Mac 本地文件夹，例如 /Users/testerbugo/wwtest。
3. 不要把 Mac 本地路径填写到“源路径”。
4. 不要把 /test 直接填写到“目标路径”。
5. 目标文件夹最好为空，避免原有文件在挂载期间被临时遮挡。

## 4. 保存配置并启动挂载

1. 再次检查源路径和目标路径。
2. 确认目标文件夹已经存在。
3. 点击“提交”保存挂载配置。
4. 返回挂载列表，找到刚刚创建的任务。
5. 点击“挂载”启动任务。

![保存挂载配置](/img/desktop/mount/macos/03.jpg)

如果点击后立即显示成功，可以直接跳到第 6 步。如果 macOS 弹出系统扩展或安全权限提示，请继续执行第 5 步。

## 5. 允许 macFUSE 系统扩展

首次挂载时，macOS 可能阻止 macFUSE 系统扩展加载。这属于 macOS 的安全保护机制，需要手动允许一次。

![macFUSE 系统扩展权限提示](/img/desktop/mount/macos/04.jpg)

请依次操作：

1. 打开“系统设置”（System Settings）。
2. 点击“隐私与安全性”（Privacy & Security）。
3. 向下滚动到“安全性”区域。
4. 找到来自开发者 Benjamin Fleischer 的系统软件被阻止提示。
5. 点击“允许”（Allow）。
6. 使用 Mac 登录密码或 Touch ID 确认。
7. 如果系统提示需要重启，请先保存其他工作，然后重启 Mac。
8. 重启后重新打开 AList Desktop。
9. 返回“挂载”页面，再次启动刚才创建的挂载任务。

> 只有从可信来源安装的 macFUSE 才应被允许。如果页面显示的开发者名称与教程不一致，请先停止操作并检查安装来源。

## 6. 确认挂载是否成功

完成挂载后，建议从 AList Desktop、终端和 Finder 三个位置进行确认。

### 6.1 检查 AList Desktop 状态

1. 返回 AList Desktop 的挂载列表。
2. 确认任务没有持续显示加载状态。
3. 检查页面是否出现挂载成功或正在运行的状态。

### 6.2 使用终端检查

1. 打开终端。
2. 执行：

~~~bash
mount | grep alist
~~~

3. 如果挂载成功，可能看到类似结果：

~~~text
alist:test on /Users/testerbugo/wwtest
~~~

4. 也可以执行：

~~~bash
df -h /Users/testerbugo/wwtest
~~~

终端可能显示：

~~~text
Filesystem    Size    Used   Avail   Mounted on
alist:test    16Ti    0Bi    16Ti    /Users/testerbugo/wwtest
~~~

这表示 AList 中的 /test 已经挂载到 /Users/testerbugo/wwtest。

### 6.3 使用 Finder 检查

1. 打开 Finder。
2. 前往设置的目标文件夹。
3. 检查是否能够看到 AList 中的目录和文件。
4. 尝试打开一个文件夹，确认目录可以正常读取。

## 7. 可选：在 AList 后台添加本机存储

如果还需要把 Mac 本地文件夹显示到 AList 网页中，可以继续配置“本机存储”。如果你的目的只是把 AList 挂载到 Finder，可以跳过本节。

### 7.1 进入存储添加页面

1. 打开 AList 管理页面。
2. 使用管理员账户登录。
3. 点击左侧菜单中的“存储”。
4. 点击页面顶部的“添加”。
5. 在驱动列表中选择“本机存储”。

![选择本机存储](/img/desktop/mount/macos/05.jpg)

### 7.2 填写挂载路径

1. 确认驱动已经选择为“本机存储”。
2. 在“挂载路径”中填写：

~~~text
/test
~~~

3. 确保该路径没有和现有存储重复。
4. 其他选项没有特殊要求时，可以先保留默认值。

![填写本机存储挂载路径](/img/desktop/mount/macos/06.jpg)

### 7.3 填写根文件夹路径

继续向下滚动，找到“根文件夹路径”。这个字段表示 AList 实际读取的 Mac 本地目录。

“挂载路径”和“根文件夹路径”的关系如下：

| 字段 | 示例 | 作用 |
| --- | --- | --- |
| 挂载路径 | `/test` | 用户在 AList 网页中看到的入口 |
| 根文件夹路径 | `/` | 把整个 Mac 系统根目录显示到 `/test` |
| 根文件夹路径 | `/Users/testerbugo/AListShare` | 只把指定文件夹显示到 `/test` |

如果希望得到当前教程截图中的效果，即在 AList `/test` 下看到 `Applications`、`Library`、`System`、`Users` 等目录，可以填写：

~~~text
挂载路径：/test
根文件夹路径：/
~~~

不过，这会让 AList 尝试访问整个 Mac 文件系统，范围较大。更安全的做法是只共享一个专用文件夹：

~~~bash
mkdir -p "$HOME/AListShare"
~~~

然后填写：

~~~text
挂载路径：/test
根文件夹路径：/Users/你的用户名/AListShare
~~~

例如当前用户名为 `testerbugo`，则根文件夹路径为：

~~~text
/Users/testerbugo/AListShare
~~~

> **重要：**不要把“根文件夹路径”设置为同一个 AList Desktop 挂载任务的“目标路径”。例如，AList Desktop 已经把 `/test` 挂载到 `/Users/testerbugo/wwtest` 时，不要再把 `/Users/testerbugo/wwtest` 作为 `/test` 本机存储的根文件夹，否则可能形成循环引用。

### 7.4 检查文件夹权限

1. 确认根文件夹路径真实存在。
2. 确认运行 AList 的 macOS 账户可以读取该目录。
3. 如果需要通过网页上传、删除或修改文件，还需要写入权限。
4. 如果选择 `/` 作为根文件夹，macOS 仍可能限制部分系统目录，这是正常的系统安全行为。

### 7.5 添加并检查存储状态

1. 向下检查页面中的必填项。
2. 再次确认“挂载路径”和“根文件夹路径”没有填反。
3. 点击页面底部的“添加”。
4. 等待页面返回存储列表。
5. 在存储列表中找到新建的 `/test` 存储。
6. 确认状态显示为 `work` 或正常工作状态。

![确认本机存储创建成功](/img/desktop/mount/macos/07.jpg)

### 7.6 检查网页和 Finder

回到 AList 网页首页，打开 `/test`。如果根文件夹路径填写的是 `/`，网页中会显示 Mac 系统根目录；如果填写的是专用文件夹，则只会显示该文件夹中的内容。

![AList 网页显示 Mac 本地目录](/img/desktop/mount/macos/08.jpg)

最后回到 Mac 的 Finder，确认 AList Desktop 创建的挂载卷仍然可以正常访问。

![在 Finder 中查看挂载目录](/img/desktop/mount/macos/09.jpg)

## 8. 常见问题

### 8.1 目标路径不存在

**现象：**

~~~text
stat /Users/testerbugo/wwtest: no such file or directory
~~~

**解决方法：**

1. 创建目标文件夹。
2. 确认路径拼写正确。
3. 返回 AList Desktop 重新挂载。

~~~bash
mkdir -p "$HOME/wwtest"
~~~

### 8.2 macFUSE 权限未开启

**现象：**点击挂载后出现系统扩展被阻止或无法加载的提示。

**解决方法：**

1. 进入“系统设置”→“隐私与安全性”。
2. 允许来自 Benjamin Fleischer 的系统软件。
3. 根据系统提示重启 Mac。
4. 重启后再次执行挂载。

### 8.3 源路径和目标路径填反

请记住：

~~~text
AList 网页目录 → 源路径
Mac 本地文件夹 → 目标路径
~~~

### 8.4 挂载后 Finder 中没有内容

请依次检查：

1. AList 中的源路径是否真实存在。
2. AList Desktop 中的挂载任务是否正在运行。
3. macFUSE 是否已经获得系统权限。
4. Finder 打开的是否为正确的目标路径。
5. AList 服务是否仍在运行。

### 8.5 AList 网页显示了整个 Mac 系统目录

这通常不是程序故障，而是“根文件夹路径”填写了 `/`。

如果只想开放一个文件夹：

1. 新建一个专用目录，例如 `/Users/你的用户名/AListShare`。
2. 编辑 `/test` 本机存储。
3. 把“根文件夹路径”从 `/` 改为专用目录。
4. 保存后重新加载存储。

## 9. 完成检查

完成全部步骤后，应满足以下条件：

- AList Desktop 中的挂载任务处于运行状态；
- 终端能够查询到 alist:test 挂载记录；
- Finder 可以打开目标目录；
- 如果配置了本机存储，AList 后台能够看到 `/test`；
- “根文件夹路径”指向你真正希望开放的 Mac 目录；
- 本机存储状态正常，目录内容可以读取。
