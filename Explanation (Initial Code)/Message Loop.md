The **message loop** is the core of any Windows application—it keeps the program running and responsive.
#### **Your Message Loop (from `run()`)**

```cpp
int windowApp::run(int show_command) {
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
#### **How it works:**

1. `GetMessageW(&msg, nullptr, 0, 0)` waits for a message (e.g., keyboard input, mouse clicks).
2. If `result == -1`, an error occurred, and the app exits.
3. `TranslateMessage(&msg)` translates keyboard input (e.g., converts raw key presses into character codes).
4. `DispatchMessageW(&msg)` sends the message to `window_proc()`, where it's handled.
#### **Why is this necessary?**

- Without a **message loop**, Windows wouldn't know how to process events.
- It ensures that your application **stays responsive** (e.g., handles input, redraws the window).
- It runs **continuously** until `PostQuitMessage()` is called.