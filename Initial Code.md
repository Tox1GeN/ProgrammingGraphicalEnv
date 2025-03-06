#firstSteps
A common way to set up a Windows application in Visual Studio using the Windows API:

1. **Define an application class** that encapsulates the main logic of the application.
2. **Register a window class** using `RegisterClassExW()`.
3. **Create the main window** using `CreateWindowExW()`.
4. **Implement a [[Message Loop|message loop]]** to handle system events (`WM_CLOSE`, `WM_DESTROY`, etc.).
5. **Use a static window procedure** ([[Message Handler]]) to forward messages to an instance-based handler.

This approach follows a standard pattern used in most native Windows applications:
### **Header file**
- windowApp.h:
```cpp
#pragma once
#include <windows.h>
#include <string>
class windowApp {
private:
	bool register_class();
	static std::wstring const s_class_name;
	static LRESULT CALLBACK window_proc_static(
		HWND window, UINT message, WPARAM wparam, LPARAM lparam
	);

	LRESULT window_proc(
		HWND window, UINT message, WPARAM wparam, LPARAM lparam
	);

	HWND create_window();
	HINSTANCE m_instance;
	HWND m_main;
public:
	windowApp(HINSTANCE instance);
	int run(int show_command);
};
```
### **Source Files**
- windowApp.cpp:
```cpp
#include "windowApp.h"
#include <stdexcept>

std::wstring const windowApp::s_class_name{ L"SimpleApp" };

bool windowApp::register_class() {
	WNDCLASSEXW desc{};
	if (GetClassInfoExW(m_instance, s_class_name.c_str(),
		&desc) != 0) return true;

	desc = { .cbSize = sizeof(WNDCLASSEXW),
		.lpfnWndProc = window_proc_static,
		.hInstance = m_instance,
		.hCursor = LoadCursorW(nullptr, L"IDC_ARROW"),
		.lpszClassName = s_class_name.c_str()
	};

	return RegisterClassExW(&desc) != 0;
}

HWND windowApp::create_window() {
	return CreateWindowExW(
	
		0 /*no extended styles*/,
		s_class_name.c_str(),
		L"SimpleApp",
		WS_OVERLAPPED | WS_SYSMENU | WS_CAPTION |
			WS_BORDER | WS_MINIMIZEBOX,
		CW_USEDEFAULT, 0, /*default position*/
		CW_USEDEFAULT, 0, /*default size*/
		nullptr,
		nullptr,
		m_instance,
		this

	);
}

LRESULT windowApp::window_proc_static(
	HWND window,
	UINT message,
	WPARAM wparam,
	LPARAM lparam
) {
	windowApp* app = nullptr;
	if (message == WM_NCCREATE) {
		auto p = reinterpret_cast<LPCREATESTRUCTW>(lparam);
		app = static_cast<windowApp*>(p->lpCreateParams);
		SetWindowLongPtrW(window, GWLP_USERDATA,
			reinterpret_cast<LONG_PTR>(app));
	} else {
		app = reinterpret_cast<windowApp*>(GetWindowLongPtrW(window, GWLP_USERDATA));
	}

	if (app != nullptr)
		return app->window_proc(window, message, wparam, lparam);

	return DefWindowProcW(window, message, wparam, lparam);
}

LRESULT windowApp::window_proc(HWND window, UINT message, WPARAM wparam, LPARAM lparam) {
	switch (message) {
		case WM_CLOSE:
			DestroyWindow(window);
			return 0;
		case WM_DESTROY:
			if (window == m_main)
				PostQuitMessage(EXIT_SUCCESS);
			return 0;
	}
	return DefWindowProcW(window, message, wparam, lparam);
}

windowApp::windowApp(HINSTANCE instance) : m_instance{ instance }, m_main{}
{
	register_class();
		m_main = create_window();
}

int  windowApp::run(int show_command) {
	ShowWindow(m_main, show_command);
	MSG msg{};
	BOOL result = TRUE;
	while ((result = GetMessageW(&msg, nullptr, 0, 0)) != 0) {
		if (result == -1)
			return EXIT_FAILURE;
		TranslateMessage(&msg);
		DispatchMessageW(&msg);
	}
	return EXIT_SUCCESS;
}
```
### **Main**
- main.cpp:
```cpp
#include <windows.h>
#include "windowApp.h"

int WINAPI wWinMain(HINSTANCE instance,
	HINSTANCE/*prevInstance*/,
	LPWSTR /*command_line*/,
	int show_command) {
	windowApp app{ instance };
	return app.run(show_command);
}
```

 You can **absolutely** reuse this code as a starting point for future Windows applications. This setup provides a **solid foundation** that you can modify and extend depending on the requirements of each specific lab task. Remember that, you can:

- Change the **class name** and window title.
- Modify the [[Window Styles|window styles]] (e.g., `WS_OVERLAPPED`, `WS_MINIMIZEBOX`, etc.).
- Extend the [[Message Handler|message handler]] to handle **keyboard input**, **timers**, or **graphics**.
- Add **child windows** or **custom [[Drawing Logic|drawing logic]]** (as done later in the document).

### Things to keep in mind:

- Your **future projects may require different features** (e.g., additional windows, timers, custom drawing).
- You might need to **modify** how messages are handled in `window_proc()`.
- If you work on a **multi-window** application (like in later sections), you may need to manage **multiple window handles** (`HWND`).
- Proper **resource management** (e.g., freeing allocated objects) will be important in larger projects.