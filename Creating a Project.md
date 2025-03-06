#firstSteps
First we need to create a Visual Studio project that we will use to implement our solution.

1. Start Visual Studio and click _Create a new project_.
    
2. Select **Windows Desktop Wizard** template and name it something.
    
3. In the wizard window select **Desktop Application (.exe)** from the drop-down under **Application type** and check **Empty project** in **Additional options**.
    
4. Once the project is created, add a new source file **main.cpp**. Initially, you should only add an empty `wWinMain`:
    
    ```cpp
    #include <windows.h>
    
    int WINAPI wWinMain(HINSTANCE instance,
                        HINSTANCE /*prevInstance*/,
                        LPWSTR /*command_line*/,
                        int show_command)
    { }
    ```
    
5. Lastly, we need to adjust project configuration. In the project properties, under **Configuration Properties** > **General**, set the **C++ Language Standard** to at least **ISO C++20 Standard** (`/std:c++20`).
    
6. Under **Configuration Properties** > **C/C++** > **Preprocessor**, expand the drop-down for **Preprocessor Definitions**, select `<Edit...>` and add the following:
    
    ```
    WIN32_LEAN_AND_MEAN
    NOMINMAX
    ```
    
    - The first one **excludes rarely used parts** in `<windows.h>` header.
    - The second **disables `min` and `max` macros**, which tend to mess with the C++ standard library.

The project should now **compile and run**, but the program **immediately exits**. 

Continue the project building in [[Initial Code]].