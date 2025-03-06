### Handling Dynamic Window Resizing (`WM_SIZE`)

When the user resizes the window, the application should adjust its content dynamically.
```cpp
LRESULT windowApp::window_proc(HWND window, UINT message, WPARAM wparam, LPARAM lparam) {
    switch (message) {
        case WM_SIZE:
            {
                int width = LOWORD(lparam);   // New width
                int height = HIWORD(lparam);  // New height
                // Handle resizing logic here (e.g., adjust UI elements)
                InvalidateRect(window, nullptr, TRUE); // Request repaint
            }
            return 0;

        case WM_PAINT:
            on_paint(window);
            return 0;

        case WM_CLOSE:
            DestroyWindow(window);
            return 0;

        case WM_DESTROY:
            PostQuitMessage(0);
            return 0;
    }
    return DefWindowProcW(window, message, wparam, lparam);
}
```

- `LOWORD(lparam)` and `HIWORD(lparam)` get the new **width** and **height**.
- You can adjust UI elements based on the new size.
- `InvalidateRect(window, nullptr, TRUE);` forces the window to repaint after resizing.
--- 

### GDI-Based Drawing (`WM_PAINT`)

This example **draws a red rectangle** in the window using **GDI (Graphics Device Interface)**.
```cpp
void windowApp::on_paint(HWND window) {
    PAINTSTRUCT ps;
    HDC hdc = BeginPaint(window, &ps);

    // Create a red brush
    HBRUSH redBrush = CreateSolidBrush(RGB(255, 0, 0));
    RECT rect = { 50, 50, 200, 200 }; // Define a rectangle area

    // Fill the rectangle with the red brush
    FillRect(hdc, &rect, redBrush);

    // Cleanup
    DeleteObject(redBrush);
    EndPaint(window, &ps);
}
```

- `BeginPaint()` and `EndPaint()` manage the **drawing context**.
- `CreateSolidBrush(RGB(255, 0, 0))` creates a **red color brush**.
- `FillRect(hdc, &rect, redBrush)` draws a **filled red rectangle** at `(50,50)` with a width/height of `150x150`.
- `DeleteObject(redBrush)` ensures proper resource cleanup.

---
### **How These Work Together**

1. **When the window is resized (`WM_SIZE`)**, it triggers a repaint.
2. **The `WM_PAINT` message calls `on_paint()`**, where GDI draws the red rectangle.
3. The rectangle stays in place, but you can modify `WM_SIZE` logic to reposition it dynamically.