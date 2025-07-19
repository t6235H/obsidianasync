#rust #基础
[[rust.创建项目]]

---
# 安装RUST环境

##### 官方工具链管理器 **rustup**
	
1. **确定安装位置**（可以不设置，直接默认安装）
	环境变量添加：
	RUSTUP_HOME = 欲安装的位置
	 CARGO_HOME = 欲安装的位置
	
2. **终端运行该命令**会下载并运行 ==rustup== 安装脚本
	```
	curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
	```
	
3. **安装选项**，推荐按1，并回车
	```
	1) Proceed with installation (default) 
	2) Customize installation 
	3) Cancel installation
	```
	
4. **验证是否成功安装**
	```
	rustc --version 
	# 应输出类似：rustc 1.72.0 (5680fa18f 2023-08-23) 
	cargo --version 
	# 应输出类似：cargo 1.72.0 (103a7ff2e 2023-08-15)
	```
	 ==rustc==: 是rust编译器
	 ==cargo==: 是rust包管理工具
	 
5. 会自动将 Rust 工具添加到系统 PATH 中。若遇到命令找不到的问题，若遇到命令找不到的问题，手动添加环境变量
	```
	Windows：`C:\Users\你的用户名\.cargo\bin`
	macOS/Linux：`~/.cargo/bin`
	```
	如果在安装rustup前，就创建了RUSTUP_HOME,CARGO_HOME,
	那么添加'%CARGO_HOME%\bin'
	
6. **windows额外依赖**
	Windows 用户需安装 **Visual Studio Build Tools**，包含 C++ 构建工具。可通过以下方式安装：
	- 安装 [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/)，并勾选 “C++ 生成工具”。
	- 或直接安装 [Build Tools for Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)。**Visual Studio 2022 生成工具（Build Tools）**
	
7. **更新rustup**
	```
	rustup update stable
	```
	
8. **卸载rustup**
	```
	rustup self uninstall
	```
	
9. **镜像加速**（可选）
	
	==github可以用steam++加速，改名后叫watt toolkit,加速模式hosts要用443端口，vm虚拟机程序会占用443；加速模式system会让科学上网出现问题，加速模式DNSintercept(速度快)和PAC可以用==
	
	如果rustup和cargo出现网络问题，可以更换国内的镜像
	
	位置：.cargo\config.toml
	```
	[source.crates-io]
	registry = "https://github.com/rust-lang/crates.io-index"
	replace-with = 'ustc' # 指定使用下面哪个源，修改为source.后面的内容即可
	
	#阿里云
	[source.aliyun]
	registry = "sparse+https://mirrors.aliyun.com/crates.io-index/"
	# 中国科学技术大学
	[source.ustc]
	registry = "https://mirrors.ustc.edu.cn/crates.io-index"
	# 上海交通大学
	[source.sjtu]
	registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index/"
	# 清华大学
	[source.tuna]
	registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
	#本地crate.io
	[source.local]
	directory = "D:/Program Files/cargo/registry/index/crates.io-index-master"
	
	```
---


