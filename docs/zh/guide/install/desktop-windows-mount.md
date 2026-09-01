---
title: Windows 挂载教程
icon: fa-brands fa-windows
order: 6.2
category:
  - Guide
tag:
  - Install
  - Desktop
  - Windows
comment: false
---

# AList 在 Windows 上挂载硬盘

本教程介绍如何在 Windows 上安装 WinFsp、使用 AList Desktop 创建挂载任务，并把 Windows 本地文件夹添加到 AList 后台。完成后，可以在 AList 网页中访问 Windows 文件夹中的内容，也可以通过 AList Desktop 管理挂载任务。

## 开始前先理解两个方向

教程中包含两组容易混淆的配置：

| 操作位置 | 作用 | 关键字段 |
| --- | --- | --- |
| AList Desktop → 挂载 | 把 AList 中的目录挂载到 Windows | 源路径、目标路径 |
| AList 后台 → 本机存储 | 把 Windows 本地文件夹显示到 AList 网页 | 挂载路径、根文件夹路径 |

本文使用以下内容作为示例：

~~~text
AList 网页路径：/test
Windows 本地文件夹：C:\wwtest
~~~

> 建议先使用 C 盘中的独立文件夹进行测试，例如 C:\wwtest，不要直接把整个系统盘根目录暴露到 AList。

## 操作总览

1. 打开 AList Desktop 的挂载页面。
2. 下载并安装 WinFsp。
3. 新建 AList Desktop 挂载配置。
4. 保存配置并启动挂载任务。
5. 登录 AList 管理后台。
6. 添加“本机存储”。
7. 填写 AList 挂载路径和 Windows 根文件夹路径。
8. 保存存储并检查状态。
9. 创建测试文件，确认网页和本地内容一致。

---

## 1. 打开 AList Desktop 挂载页面

1. 启动 AList Desktop。
2. 等待程序完成初始化。
3. 点击顶部导航中的“挂载”。
4. 确认页面中可以看到“新建”按钮和 WinFsp 安装提示。

![AList Desktop 的 Windows 挂载页面](/img/desktop/mount/windows/01.jpg)

WinFsp 是 Windows 上的文件系统组件。AList Desktop 需要通过它将 AList 目录挂载为 Windows 可以访问的磁盘或路径。

## 2. 下载并安装 WinFsp

1. 优先从 [WinFsp 官方发布页](https://github.com/winfsp/winfsp/releases) 下载当前稳定版安装程序，也可以使用 AList Desktop 提供的可信下载入口。
2. 下载完成后，双击 `.msi` 安装程序。
3. 在安装向导中点击“Next”。
4. 按照默认选项完成安装。
5. 如果安装程序提示重启 Windows，请先重启，再继续后面的步骤。

![安装 WinFsp](/img/desktop/mount/windows/02.jpg)

截图中的 WinFsp 2023 仅用于演示安装界面，实际安装时应优先选择官方当前稳定版本。安装完成后，建议关闭并重新打开 AList Desktop，让程序重新检测 WinFsp。

## 3. 新建 AList Desktop 挂载配置

1. 返回 AList Desktop 的“挂载”页面。
2. 点击“新建”。
3. 在“源路径”中填写 AList 里的目录，例如 /test。
4. 在“目标路径”中填写 Windows 目标位置。本教程截图中填写的是 C。
5. 根据需要选择是否启用“启动时自动挂载”。
6. 缓存模式和自定义参数不确定时，可以先保留默认值。

![填写 AList Desktop 挂载配置](/img/desktop/mount/windows/03.jpg)

### 3.1 源路径是什么

源路径指 AList 网页里的目录，而不是 Windows 本地路径。

例如：

~~~text
/test
~~~

### 3.2 目标路径是什么

目标路径是 Windows 端的挂载位置或目标盘符设置。请按照 AList Desktop 当前界面的格式填写，并确认目标位置没有与现有挂载冲突。

### 3.3 填写时需要注意

1. 不要把 C:\wwtest 填到“源路径”。
2. 不要把 /test 当作 Windows 文件夹路径。
3. 源路径必须在 AList 中真实存在。
4. 如果使用盘符作为目标，请确认该盘符符合当前客户端要求。
5. 第一次测试时，建议不要开启复杂缓存或自定义 rclone 参数。

## 4. 保存并启动挂载任务

1. 再次检查源路径和目标路径。
2. 点击“提交”保存配置。
3. 返回挂载列表。
4. 找到刚刚创建的任务。
5. 点击“挂载”。
6. 等待按钮变为“卸载”，或等待任务显示正在运行。

![AList Desktop 挂载任务已启动](/img/desktop/mount/windows/04.jpg)

如果按钮已经显示“卸载”，通常表示当前任务已经进入挂载状态。如果挂载失败，可以点击“日志”查看具体原因。

## 5. 登录 AList 管理后台

接下来把 Windows 本地文件夹添加为 AList 的“本机存储”。

1. 打开浏览器。
2. 进入 AList 管理页面。默认本机地址通常为：

~~~text
http://127.0.0.1:5244
~~~

3. 使用管理员账户登录。
4. 确认已经进入 AList 管理后台。
5. 点击左侧菜单中的“存储”。

![进入 AList 管理后台](/img/desktop/mount/windows/05.jpg)

## 6. 添加本机存储

1. 在“存储”页面点击右上角的“添加”。
2. 打开驱动下拉列表。
3. 找到并选择“本机存储”。
4. 等待本机存储配置表单加载完成。

![选择本机存储驱动](/img/desktop/mount/windows/06.jpg)

## 7. 填写挂载路径和根文件夹路径

这一步最容易填错。请先区分两个字段：

| 字段 | 示例 | 用户在哪里看到它 |
| --- | --- | --- |
| 挂载路径 | /test | AList 网页首页 |
| 根文件夹路径 | C:\wwtest | Windows 资源管理器 |

### 7.1 填写挂载路径

1. 在“挂载路径”中填写 /test。
2. 该路径会成为 AList 网页中的访问入口。
3. 挂载路径必须唯一，不能和已有存储使用相同路径。

![填写 AList 挂载路径](/img/desktop/mount/windows/07.jpg)

### 7.2 填写根文件夹路径

1. 确认 Windows 中已经存在 C:\wwtest 文件夹。
2. 如果文件夹不存在，可以先在资源管理器中创建。
3. 在“根文件夹路径”中填写：

~~~text
C:\wwtest
~~~

4. 也可以点击右侧的“选择”按钮定位文件夹。
5. 确认运行 AList 的 Windows 用户拥有读取该文件夹的权限。
6. 如果需要通过网页上传、删除或修改文件，还需要对应的写入权限。

![填写 Windows 根文件夹路径](/img/desktop/mount/windows/08.png)

可以这样理解：

~~~text
Windows：C:\wwtest
          ↓
AList 本机存储
          ↓
AList 网页：http://127.0.0.1:5244/test
~~~

## 8. 保存存储并检查状态

1. 检查“挂载路径”是否为 /test。
2. 检查“根文件夹路径”是否为 C:\wwtest。
3. 检查其他必填项是否已经完成。
4. 点击页面底部的“添加”或“保存”。
5. 返回存储列表。
6. 确认新建存储的状态显示为 work 或正常工作状态。

![本机存储添加成功](/img/desktop/mount/windows/09.jpg)

如果状态不是 work，可以点击“编辑”重新检查路径，也可以查看 AList 日志确认是否存在权限或目录错误。

## 9. 创建测试文件并验证

最后通过一个测试文件确认 Windows 本地文件夹与 AList 网页目录已经对应。

### 9.1 在 Windows 中创建测试文件

方法一：使用资源管理器。

1. 打开 C:\wwtest。
2. 在空白处点击右键。
3. 新建一个文本文件。
4. 将文件命名为 test1.txt。

方法二：使用 PowerShell。

~~~powershell
New-Item -Path "C:\wwtest\test1.txt" -ItemType File
~~~

### 9.2 在 AList 网页中检查

1. 返回 AList 网页首页。
2. 打开 /test 目录。
3. 刷新页面。
4. 确认页面中可以看到 test1.txt。
5. 尝试打开或下载该文件，确认读取正常。

![Windows 本地文件与 AList 网页显示一致](/img/desktop/mount/windows/10.jpg)

如果 Windows 和 AList 网页中都能看到同一个测试文件，说明本机存储配置已经生效。

## 10. 常见问题

### 10.1 AList Desktop 提示没有安装 WinFsp

1. 确认 WinFsp 安装程序已经执行完成。
2. 关闭并重新打开 AList Desktop。
3. 如果安装程序要求重启，请重启 Windows。
4. 重启后再次进入“挂载”页面检查。

### 10.2 点击挂载后没有反应

1. 点击任务右侧的“日志”。
2. 检查源路径是否在 AList 中存在。
3. 检查目标位置或盘符是否填写正确。
4. 检查 WinFsp 是否正常安装。
5. 尝试退出 AList Desktop 后重新打开。

### 10.3 本机存储状态异常

请重点检查：

1. C:\wwtest 是否真实存在。
2. “根文件夹路径”是否拼写正确。
3. AList 进程是否拥有该文件夹的读取权限。
4. “挂载路径”是否与其他存储重复。
5. Windows 安全软件是否阻止了 AList 访问文件夹。

### 10.4 网页看不到刚创建的文件

1. 确认文件创建在 C:\wwtest，而不是其他同名目录。
2. 刷新 AList 网页。
3. 检查 /test 存储状态是否为 work。
4. 进入存储编辑页面，重新核对根文件夹路径。
5. 查看 AList 日志是否存在读取失败或权限错误。

### 10.5 能查看但不能上传或删除

这通常是 Windows 文件夹权限不足导致的。

1. 检查运行 AList 的账户。
2. 确认该账户对 C:\wwtest 拥有写入和修改权限。
3. 修改权限后重新加载存储。
4. 再创建一个测试文件验证写入功能。

## 11. 完成检查

完成全部步骤后，应满足以下条件：

- WinFsp 已经安装；
- AList Desktop 挂载任务可以正常启动；
- AList 后台已经添加 /test 本机存储；
- 根文件夹路径指向 C:\wwtest；
- 存储状态显示正常；
- Windows 本地创建的 test1.txt 能在 AList 网页中看到；
- AList 网页中的文件可以正常打开或下载。
