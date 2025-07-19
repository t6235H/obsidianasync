#cmd #基础 

---
- **`cmd.exe /s /k`** 是用于启动 Windows 命令提示符（Command Prompt）的命令行参数组合，常用于**开发环境配置、快捷方式创建或批处理脚本**中。通过搭配不同命令，可实现环境变量设置、路径切换、界面美化等功能，提升命令行使用效率
- /c 是执行完命令后，**关闭**命令提示符窗口 不加相关参数，**默认关闭**
- /k 是执行完命令后，**保持**命令提示符窗口
- /s 不是单独使用的，而是配合其它参数。作用是**正确处理字符串**。如果字符串中有一些具有特殊作用的字符，它可以正确解析，而不是当成普通的字符
---
### 例子：
1. **转义特殊命令中的字符**
	当命令包含 `&`, `|` 等批处理命令符时，`/s` 可避免解析错误：
	```
	cmd.exe /s /k "echo 命令执行成功 & date" # 正确执行两条命令
	```
2. **启动时执行自定义命令并保持窗口打开**
	```
	cmd.exe /k "cd C:\projects\my-app && echo 欢迎进入项目目录"
	```
3.  **创建带预设环境的快捷方式**
	在桌面上创建快捷方式，目标为：
	```plaintext
	C:\Windows\System32\cmd.exe /s /k "set PATH=%PATH%;C:\tools\my-utils && prompt $P$G"
	```
	- 将 `C:\tools\my-utils` 添加到环境变量 `PATH`。
	- 修改命令提示符前缀为当前路径（`$P`）和大于号（`$G`）。
4.  **启动时设置颜色和标题**
	```
	cmd.exe /s /k "color 0A && title 我的自定义终端 && echo 按Ctrl+C退出"
	```
5. **加载自定义配置文件**
	若有批处理脚本 `config.bat`，内容为：
	```batch
	@echo off
	set PATH=%PATH%;C:\Python311
	prompt [管理员] $P$G
	```
	则启动命令为：
	```bash
	cmd.exe /s /k "C:\config.bat"
	```
> [!note] 注意
> 1. **命令解析顺序**
> - `/s` 会先处理命令字符串，再执行 `/k` 或 `/c` 的行为。
> - 若命令中包含引号，`/s` 会移除首尾引号，但中间的引号保留。


```
```

1. 如果需要更强大的功能，可以使用**powershell**
```
powershell -NoExit -Command "cd C:\projects; Write-Host '欢迎'"
```
2. **管理员权限**：若需执行管理员命令（如修改注册表），需右键点击命令提示符并选择 “以管理员身份运行”，或在参数中结合 **`runas` 命令**：
```
runas /user:Administrator "cmd.exe /s /k netsh firewall show state"
```


























