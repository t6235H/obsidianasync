#c语言 #基础 #windowsApi #编码 


`#include <Windows.h>`

`MessageBox(NULL,"打印操作完成","提示",MB_OK | MB_ICONWARNING);`

MessageBox()会根据字符集配置，自动使用不同的函数：

**使用 Unicode 字符集**：会使用MessageBox==W==()  
	在 Unicode 项目中，直接使用 `LPCSTR` 传递中文等非 ASCII 字符可能导致乱码，需使用 `LPCWSTR` 并添加 `L` ==前缀==（如 `L"中文"`）。
	如果系统默认语言是中文，此时上面的代码会报错，因为中文系统的ANSI是GBK编码
	[[编码]]
	`MessageBoxW(NULL,L"打印操作完成",L"提示",MB_OK | MB_ICONWARNING); `
	如果是英文系统，ANSI是ASCII
**使用多字节字符集**：会使用MessageBox==A==()  
`MessageBoxA(NULL,"打印操作完成","提示",MB_OK | MB_ICONWARNING); `

`C++ int __stdcall MessageBoxW(HWND hWnd, LPCWSTR lpText, LPCWSTR lpCaption, UINT uType)`

`C++ int __stdcall MessageBoxA(HWND hWnd, LPCSTR lpText, LPCSTR lpCaption, UINT uType)`

里面有有四个常用的类型别名：
- HWND Handle to a Window 窗口句柄                   ==指针或整数==      [[HWND]]
- LPCSTR Long Pointer to Constant STR ing              ==const char*==      [[C.const指针]]
- LPCWSTR Long Pointer to Constant Wide STRing  ==const wchar_t*==
- UINT 无符号整数  typedef unsigned int UINT;        ==uint32_t== stdint.h->C || cstdint.h->C++                                       
- **`LPWSTR`**：Long Pointer to Wide STRing                 ==wchar_t==  宽字符字符串
- **`LPCTSTR`**：条件性字符串指针，根据项目字符集配置自动映射为 `LPCSTR` 或 `LPCWSTR`。

在 Visual Studio 2022 中设置 C 语言项目的字符集，可以按照以下步骤操作：

### 步骤 1：打开项目属性页

1. 在 “解决方案资源管理器” 中右键点击你的项目名称
2. 选择 **属性**（Properties）

### 步骤 2：定位字符集设置

1. 在左侧面板中展开：
    - **配置属性**（Configuration Properties）
    - **高级**
2. 在右侧找到 **字符集**（Character Set）选项

### 步骤 3：选择字符集

你可以选择以下两种之一：

  

- **使用 Unicode 字符集**（对应 `MessageBoxW`）
- **使用多字节字符集**（对应 `MessageBoxA`）

### 步骤 4：应用设置

点击 **确定** 保存更改。

### 示例截图位置

如果需要可视化帮助，可以参考以下位置：

```plaintext
项目属性 > 配置属性 > 高级 > 字符集
```

  
### 代码适配建议

无论选择哪种字符集，建议使用通用的 `MessageBox` 宏，并确保字符串类型匹配：

- 对于 Unicode：`L"宽字符串"`
- 对于多字节：`"普通字符串"` 
这样可以避免字符集不匹配的编译警告或错误。