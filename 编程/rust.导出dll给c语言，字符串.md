#rust #动态链接库 #dll #字符串 #c语言 

---
###### 1. 在终端中运行以下命令以创建新的 Rust 项目：
```
cargo new rust_c_lib --lib
```
	src/lib.rs 不可以是main.rs
###### 2. 修改 `Cargo.toml`
```
[lib] 
crate-type = ["cdylib"] 
#crate-type是一个数组，可以一次性指定多个库类型，同时生成多个dll
#name = "库名字" 可以指定dll的名字，不特别指定name,dll就是项目的名字
```
###### 3.修改src/lib.rs
```
use std::ffi::{CStr, CString};

use std::os::raw::c_char;

use encoding_rs::GBK;

  

#[unsafe(no_mangle)]  //使用extern "stdcall"是适配易语言，如果是c语言，用extern "C"

pub extern "stdcall" fn free_string(ptr: *mut c_char) {

    if ptr.is_null() { return; }

    unsafe {

        drop(CString::from_raw(ptr));

    }

}

  
  
  
  
  

    // 将输入的 c字符串参数 转换成 rust字符串，rust的函数处理好后，转换回 c字符串

    //`input: *const c_char` 是一个 **只读的 C 风格字符串**

    // 它等价于 `const char*`（C 语言里）

    // 它是一个指向以 `\0` 结尾的 UTF-8 字符串的 **指针**，对应于 C/C++ 或易语言中的 **ANSI 字符串指针**

    // 要在 Rust 里用 `CStr::from_ptr(input)` 把它转成 `&str` 来用

#[unsafe(no_mangle)]

pub extern "stdcall" fn utf8_convert_to_yiyuyan(input: *const c_char) -> *mut c_char {

    //返回的是一个“拥有所有权”的指针”，必须由调用者手动释放

    //能被 CString::from_raw(ptr) 正确释放内存

    //如果返回 *const c_char，那表示“不可修改 + 不可释放”，Rust 无法安全释放 → 会内存泄漏或报错

    //易语言使用DLL时，传入文本型，接收整数型

  

    if input.is_null() {

        return std::ptr::null_mut();

    }

  

    // C 字符串 -> Rust 字符串

    let c_str = unsafe { CStr::from_ptr(input) };

    //CStr：C 的字符串借用（const char*）

    //c_str的内存是易语言分配，rust不要多管闲事，易语言擦不擦屁股，和rust无关

    //from_ptr() 从 C 的 char* 开始向后找 第一个 \0（null terminator）

    //然后把这块内存当作 &CStr（只读字节片段）

    //它相信你传入的指针是合法的，查找了\0的位置，然后视作&CStr来使用，没有复制内存

    let rust_str = match c_str.to_str() {

        Ok(s) => s,

        Err(_) => return std::ptr::null_mut(),

    }; //rust_str 并不会分配新内存，它是对原 c_str 的一个只读引用切片（&str）而已

    //CStr 是字节序列（&[u8]）

    //to_str() 会尝试将其 验证为 UTF-8，然后返回 &str

    //它是检查c字符串是不是utf-8,如果是，就能直接视作rust字符串来用；如果不是，就直接扔掉

    //如果成功，返回的是原字节序列的一个“零开销”UTF-8视图（&str）

    //没有内存复制、没分配、没转换 → 只是个安全封装

    //你有一行字（C 字符串），Rust 用手指着每个字（CStr::from_ptr）来数它的字数，确认它真是C字符串；

    //然后查词典（&CStr.to_str()），确认这段说的是人话，就叫它rust的 &str，但是你一直看的是原来的纸，没复印

  

    let result = inner_convert_to_yiyuyan(rust_str);

    //这一步发生了 新的堆内存分配。result 是一个全新的 String，它是 &str 的复制品

  

    // Rust 字符串 -> C 字符串

    CString::new(result).unwrap().into_raw()  

    //Rust 官方推荐 凡是你手动 into_raw() 的，返回 *mut 并配 free()

    //CString：Rust 创建的字符串（用 malloc 分配）

    //表示“一个由 Rust 拥有的 C 字符串”，尾部有 \0

    //用 CString::new("hello") 创建

    //用 into_raw() 返回给 C

    //必须用 CString::from_raw(ptr) 再回收

    //CString的内存是rust分配的，rust要提供一个free_string函数，能把自己的屁股擦干净的纸

    //但是所有权交给了易语言，擦屁股的动作要调用方来完成

    //CString::new(result) 拥有了字符串的堆内存->你租了一个新房，复制了你旧家的数据

    //into_raw()->你交出钥匙，不再续租，由调用方自己负责清理

    //into_raw() 把这个 CString 拆成一个裸指针，夺走了 c_string 的内存所有权，并将其作为 *mut c_char 裸指针交给易语言

    //Rust 不再自动释放它，内存交给易语言手动管理

}

  

//理解思路去看fn utf8_convert_to_yiyuyan，思路都在那里，现在的这个是处理GBK的，因为易语言的编码就是GBK

#[unsafe(no_mangle)]

pub extern "stdcall" fn convert_to_yiyuyan(input: *const c_char) -> *mut c_char {

    if input.is_null() { return std::ptr::null_mut()};

    // 原始 C 字符串 → &[u8]

    let c_str = unsafe { CStr::from_ptr(input) };

    let bytes = c_str.to_bytes(); // 不会校验编码，仅做原始切片

  

    // 尝试 UTF-8 解码

    let decoded = match std::str::from_utf8(bytes) {

        Ok(utf8_str) => utf8_str.to_string(),

        Err(_) => {

            // 若 UTF-8 解码失败，则尝试 GBK

            let (cow, _, _) = GBK.decode(bytes);

            cow.to_string()

        }

    };

  

    let result = inner_convert_to_yiyuyan(&decoded);

    // Step 3: 转换为 GBK 编码

    let (gbk_bytes, _, had_errors) = GBK.encode(&result);

    if had_errors {

        return std::ptr::null_mut();

    }

  

    // Step 4: 包装为 C 字符串（CString 会自动添加 \0）

    CString::new(gbk_bytes.into_owned())

        .ok()

        .map(|s| s.into_raw())

        .unwrap_or(std::ptr::null_mut())

}

  
  
  

fn inner_convert_to_yiyuyan(input: &str) -> String {

    if input.is_empty() {

        return "".to_string();

    }

  

    let mut result = String::new();

    let mut segment = String::new();

  

    for ch in input.trim_end().chars() {

        if ch != '"' {

            //非引号字符，临时保存到segment

            segment.push(ch);

        } else {

            // 遇到引号,先判断segment里有没有临时保存的内容

            //如果有，先处理segment（引号 segment 引号 加号）

            //清空segment,用来保存后面的临时内容

            if !segment.is_empty() {

                result.push('"');

                result.push_str(&segment);

                result.push_str("\"+");

                segment.clear();

            }

            // 如果遇到引号了，并且segment是空的，说明第一个字符就是引号，直接添加 #引号

            result.push_str("#引号+");

        }

    }

  

    // 处理结尾

    //如果segment还有剩余内容,(引号 segment 引号)，不需要加号了

    if !segment.is_empty() {

        result.push('"');

        result.push_str(&segment);

        result.push('"');

    } else {

        // 如果segment是空的，说明最后一个字符是引号，移除末尾多余的 "+"

        if result.ends_with('+') {

            result.pop();

        }

    }

  

    result

}

  

#[cfg(test)]

mod tests {

    use super::*;

  

    #[test]

    fn test_basic_conversion() {

        let input = r#"Hello "World""#;

        let expected = r#""Hello "+#引号+"World"+#引号"#;

        assert_eq!(inner_convert_to_yiyuyan(input), expected);

    }

  

    #[test]

    fn test_only_quotes() {

        let input = r#""""#;

        let expected = r#"#引号+#引号"#;

        assert_eq!(inner_convert_to_yiyuyan(input), expected);

    }

  

    #[test]

    fn test_empty_string() {

        assert_eq!(inner_convert_to_yiyuyan(""), "");

    }

  

    #[test]

    fn test_protocol_data() {

        let input = r#"<xml><value>"123"</value></xml>"#;

        let expected = r#""<xml><value>"+#引号+"123"+#引号+"</value></xml>""#;

        assert_eq!(inner_convert_to_yiyuyan(input), expected);

    }

  

    #[test]

    fn test_json_example() {

        let input = r#"{"model": "deepseek-chat","messages": [{"role": "user", "content": "Hello!"}],"stream": false}"#;

        let expected = r#""{"+#引号+"model"+#引号+": "+#引号+"deepseek-chat"+#引号+","+#引号+"messages"+#引号+": [{"+#引号+"role"+#引号+": "+#引号+"user"+#引号+", "+#引号+"content"+#引号+": "+#引号+"Hello!"+#引号+"}],"+#引号+"stream"+#引号+": false}""#;

        assert_eq!(inner_convert_to_yiyuyan(input), expected);

    }

}
```
###### 4.编译 DLL
```
cargo build --release --target=i686-pc-windows-gnu

```
生成的 DLL 在：target/release/yiyuyan_convert.dll

Rust 编译需使用 `i686-pc-windows-gnu`，与易语言一致（32位）。

#### 如何切换编译目标

1. **安装 32 位目标**
	```
	rustup target add i686-pc-windows-gnu
	```
2. **用正确的目标编译dll**
	```
	cargo build --release --target=i686-pc-windows-gnu
	```
3. **生成的DLL在**
	```
	target/i686-pc-windows-gnu/release/yourname.dll
	```


#### error: linker `i686-w64-mingw32-gcc` not found:

- 这个错误说明：已经正确安装了 32 位 Rust 编译目标 `i686-pc-windows-gnu`，但 **缺少 32 位 MinGW 工具链的 linker（`i686-w64-mingw32-gcc`）**，Rust 无法完成链接操作

- 解决方案：安装 MinGW-w64（32位版本）
	- 安装 MSYS2
	- 打开 MSYS2 的终端，运行：
	- pacman -Syu # 先更新系统 
	- pacman -Su # 再次更新（确保完全更新）
	- `pacman -S mingw-w64-i686-gcc`
	- 然后确保 `i686-w64-mingw32-gcc` 可在 `PATH` 中访问
	- 回到项目目录，重新运行：
	- `cargo build --release --target=i686-pc-windows-gnu`
	- 如果你不需要 MinGW 生成的 DLL（比如不需要跨平台兼容性），可以改用 ​**​MSVC（Microsoft Visual C++）​**​ 目标：
	```
	rustup target add x86_64-pc-windows-msvc  # 64位
	# 或
	rustup target add i686-pc-windows-msvc    # 32位
	cargo build --release --target=i686-pc-windows-msvc
	```
	
	
    





