#cheatsheet
Window styles define how a window looks and behaves. You specify them in `CreateWindowExW()`.

#### **Basic Window Styles**

|Style|Description|
|---|---|
|`WS_OVERLAPPED`|A standard window with a title bar.|
|`WS_POPUP`|A window without a border (used for popups).|
|`WS_CHILD`|A child window (used inside parent windows).|
|`WS_VISIBLE`|The window is visible after creation.|
|`WS_DISABLED`|The window is disabled (cannot receive input).|
|`WS_CLIPSIBLINGS`|Prevents overlapping sibling windows from drawing over each other.|
|`WS_CLIPCHILDREN`|Excludes child windows from being drawn in the parent’s background.|

#### **Caption and Border Styles**

|Style|Description|
|---|---|
|`WS_CAPTION`|The window has a title bar.|
|`WS_BORDER`|The window has a thin border.|
|`WS_DLGFRAME`|The window has a border without a title bar.|

#### **Buttons on the Title Bar**

|Style|Description|
|---|---|
|`WS_SYSMENU`|Enables the system menu (right-click title bar).|
|`WS_MINIMIZEBOX`|Enables the minimize button.|
|`WS_MAXIMIZEBOX`|Enables the maximize button.|

#### **Sizing and Positioning**

|Style|Description|
|---|---|
|`WS_THICKFRAME`|Allows resizing the window.|
|`WS_OVERLAPPEDWINDOW`|Standard window style (`WS_OVERLAPPED|

#### **Extended Window Styles (Used in `CreateWindowExW`)**

| Style              | Description                             |
| ------------------ | --------------------------------------- |
| `WS_EX_TOPMOST`    | Keeps the window on top of all others.  |
| `WS_EX_LAYERED`    | Enables transparency effects.           |
| `WS_EX_TOOLWINDOW` | Creates a small toolbar-like window.    |
| `WS_EX_NOACTIVATE` | Prevents the window from gaining focus. |
