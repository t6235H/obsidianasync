#rust #基础
[[rust.基础语法]]

---
# 一、创建RUST项目
	
	
1. 创建一个**不包含 Git 版本控制**的文件夹
	在 Rust 中，当你使用`cargo new`命令创建新项目时，默认会自动初始化一个 Git 仓库。如果你想，可以使用`--vcs none`参数来禁用这个行为。
	
	在终端中执行以下命令：
	```bash
	cargo new --vcs none 项目名称
	```
	参数解析:
	`--vcs none`：  
	`vcs` 即 **Version Control System**（版本控制系统），`none` 表示不使用任何版本控制。
	
	-其他可选的`vcs`值：
		- `git`（默认值）：初始化 Git 仓库。
		- `hg`：初始化 Mercurial 仓库。
		- `pijul`：初始化 Pijul 仓库。
		- `fossil`：初始化 Fossil 仓库。  

2. **基本语法**
	
	cargo new [选项] **项目名称** [路径]
	
	- **项目名称**：必须参数，指定项目文件夹和包名（如`my-project`）。
	- **路径**：可选参数，指定项目创建位置（默认当前目录）。
	
3. **常用选项**

| 选项               | 作用                                   |
| ---------------- | ------------------------------------ |
| `--bin`          | 创建可执行程序项目（默认选项，生成`src/main.rs`作为入口）。 |
| `--lib`          | 创建库项目（生成`src/lib.rs`作为库入口），也可导出dll   |
| `--vcs none`     | 不初始化 Git 仓库（默认会创建 Git 仓库）。           |
| `--edition 2024` | 指定 Rust 版本（如 2024 版，默认根据当前工具链确定）。    |
| `--name 自定义名称`   | 指定 Cargo.toml 中的包名（与文件夹名不同）。         |
	```bash
	cargo new my-app
	# 等价于
	cargo new --bin my-app
	```
	项目结构：
	```plaintext
	my-app/
	├── Cargo.toml      # 项目配置文件
	└── src/
	    └── main.rs     # 程序入口点（包含fn main()函数）
	```
	```bash
	cargo new --lib my-lib
	``` 
	```plaintext
	my-lib/
	├── Cargo.toml      # 项目配置文件
	└── src/
	    └── lib.rs      # 库入口点（包含pub mod等模块定义）
	```
	
	```bash
	cargo new my-project /path/to/directory
	```
	



# 二、初始文件
	
	
1. **Cargo.toml**：项目配置文件，包含元数据和依赖： 
    ```toml
    [package]
    name = "my-project"
    version = "0.1.0"
    edition = "2024"  # Rust版本
	resolver = "3"  # 手动显式指定resolver 3, Cargo 推荐的现代依赖解析策略，能有效减少依赖冲突和编译时间。新项目建议默认使用
    
    # 依赖项（示例）
    [dependencies]
    rand = "0.8.5"
    ```
	
2. **src/main.rs**（可执行项目）：
    ```rust
    fn main() {
        println!("Hello, world!");
    }
    ```  
    
3. **src/lib.rs**（库项目）：
    ```rust
    #[cfg(test)]
    mod tests {
        #[test]
        fn it_works() {
            assert_eq!(2 + 2, 4);
        }
    }
    ```
# 三、后续操作

1. **编译项目**：
    ```bash
    cargo build       # 编译（debug模式）
    cargo build --release  # 编译（release模式）
    ```
    
2. **运行项目**：
    ```bash
    cargo run         # 编译并运行可执行程序
    ```
    
3. **添加依赖**：
    ```bash
    cargo add rand    # 添加rand库依赖
    ```
    

# 四、进阶技巧

1. **自定义模板**：  
    使用`cargo-generate`工具基于模板创建项目（需先安装）：
    ```bash
    cargo install cargo-generate
    cargo generate --git https://github.com/rust-lang/cargo-generate-template
    ```
    
2. **配置默认选项**：  
    修改`~/.cargo/config.toml`文件，设置默认选项：
    ```toml
    [new]
    vcs = "none"      # 默认不创建Git仓库
    edition = "2021"  # 默认使用Rust 2021版
    ```
 ---


















