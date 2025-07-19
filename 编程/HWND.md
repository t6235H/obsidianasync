#c语言 #基础 #windowsApi 



**`HWND` 的大小始终与系统指针宽度一致，确保能表示系统中所有可能的内存地址或对象索引。**

| 系统架构 | HWND 大小 | 等价类型                 |
| ---- | ------- | -------------------- |
| 32 位 | 4 字节    | `int`, `void*`       |
| 64 位 | 8 字节    | `long long`, `void*` |

`HWND` 是 Windows API 中的一个重要数据类型，其英文全称为：

**`Handle to a Window`**  
（窗口句柄）

### 一、基本定义

- **本质**：`HWND` 是一个指向窗口对象的句柄（整数标识符），用于唯一标识一个窗口。
- **用途**：在 Windows 编程中，所有与窗口相关的操作（如移动、调整大小、发送消息、设置属性等）都需要通过 `HWND` 来指定目标窗口。

### 二、相关概念

1. **窗口（Window）**
    
    - 在 Windows 系统中，窗口是用户界面的基本单元，包括应用程序窗口、对话框、按钮、文本框等控件。
    - 每个窗口都由一个 `HWND` 标识，通过该句柄可以操作窗口的外观和行为。
2. **句柄（Handle）**
    
    - Windows 系统使用句柄来引用各种资源（如窗口、图标、菜单、设备上下文等）。
    - 句柄本质是一个无符号整数，由系统分配和管理，应用程序通过句柄间接访问资源。

### 三、常见用途

1. **创建窗口**
    
    c
    
    运行
    
    ```c
    // 创建窗口并返回 HWND
    HWND hwnd = CreateWindow(
        lpClassName,      // 窗口类名
        lpWindowName,     // 窗口标题
        dwStyle,          // 窗口样式
        x, y,             // 位置
        nWidth, nHeight,  // 大小
        hWndParent,       // 父窗口句柄
        hMenu,            // 菜单句柄
        hInstance,        // 应用程序实例句柄
        lpParam           // 创建参数
    );
    ```
    
      
    
2. **操作窗口**
    
    c
    
    运行
    
    ```c
    // 移动窗口
    MoveWindow(hwnd, x, y, width, height, bRepaint);
    
    // 显示/隐藏窗口
    ShowWindow(hwnd, nCmdShow);
    
    // 发送消息到窗口
    SendMessage(hwnd, WM_COMMAND, wParam, lParam);
    ```
    
      
    
3. **获取窗口句柄**
    
    c
    
    运行
    
    ```c
    // 获取当前活动窗口
    HWND hwndActive = GetActiveWindow();
    
    // 通过窗口标题查找窗口
    HWND hwndTarget = FindWindow(NULL, "窗口标题");
    ```
    
      
    

### 四、注意事项

1. **有效性检查**
    
    - 创建窗口或查找窗口后，需检查返回的 `HWND` 是否为 `NULL`（表示失败）。
2. **线程安全**
    
    - 原则上，窗口句柄只能由创建它的线程操作。跨线程操作窗口需使用 `PostMessage` 或 `SendMessage` 等机制。
3. **资源释放**
    
    - 窗口销毁后，对应的 `HWND` 会失效，继续使用可能导致程序崩溃。

### 五、相关类型

|类型|英文全称|说明|
|---|---|---|
|`HWND`|Handle to a Window|窗口句柄|
|`HINSTANCE`|Handle to an Instance|应用程序实例句柄|
|`HMENU`|Handle to a Menu|菜单句柄|
|`HDC`|Handle to a Device Context|设备上下文句柄（绘图用）|
|`HICON`|Handle to an Icon|图标句柄|
|`HCURSOR`|Handle to a Cursor|光标句柄|

### 六、示例代码

以下是一个简单的窗口创建和消息循环示例：

  

c

运行

```c
#include <windows.h>

// 窗口过程函数
LRESULT CALLBACK WindowProc(HWND hwnd, UINT uMsg, WPARAM wParam, LPARAM lParam) {
    switch (uMsg) {
        case WM_DESTROY:
            PostQuitMessage(0);
            return 0;
    }
    return DefWindowProc(hwnd, uMsg, wParam, lParam);
}

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nCmdShow) {
    // 注册窗口类
    WNDCLASS wc = {0};
    wc.lpfnWndProc = WindowProc;
    wc.hInstance = hInstance;
    wc.lpszClassName = "MyWindowClass";
    RegisterClass(&wc);

    // 创建窗口
    HWND hwnd = CreateWindow(
        "MyWindowClass", "我的窗口", WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, CW_USEDEFAULT, 800, 600,
        NULL, NULL, hInstance, NULL
    );

    // 显示窗口
    ShowWindow(hwnd, nCmdShow);

    // 消息循环
    MSG msg = {0};
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return (int)msg.wParam;
}
```

  

  

  

  

  

  

  

![](data:image/png;base64,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)

  

在这个示例中，`hwnd` 变量就是创建窗口后返回的句柄，后续所有窗口操作都基于该句柄进行。