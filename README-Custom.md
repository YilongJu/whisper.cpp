# SDL2 Setup for whisper.cpp on Windows (using Pre-built VC Libraries)

This document outlines the steps taken to configure and use SDL2 with `whisper.cpp` (specifically for the `whisper-stream` example) on Windows, using the pre-compiled development libraries provided by the SDL team (e.g., `SDL2-devel-2.32.4-VC.zip`).

It was found that the Visual C++ development libraries contain the necessary CMake configuration files, allowing for a simpler setup using `CMAKE_PREFIX_PATH`.

1.  **Download and Extract SDL2:**
    *   Download the SDL2 Development Library for Visual C++ (e.g., `SDL2-devel-2.32.4-VC.zip`) from the [SDL releases page](https://github.com/libsdl-org/SDL/releases).
    *   Extract the contents of the zip file to a known location on your system (e.g., `C:\SDL2-2.32.4`).

2.  **Clean Previous CMake Configuration (Important):**
    *   If you previously tried to configure the project with incorrect or incomplete SDL2 paths, CMake might have cached this information. Remove the existing build directory to ensure a clean start:
        ```bash
        # In Git Bash or similar
        rm -rf build
        ```

3.  **Configure `whisper.cpp` using `CMAKE_PREFIX_PATH`:**
    *   Run CMake from the `whisper.cpp` root directory, enabling SDL2 support (`-DWHISPER_SDL2=ON`) and telling CMake where to find the extracted SDL2 library using `CMAKE_PREFIX_PATH`.
    *   Point `CMAKE_PREFIX_PATH` to the *root* of the extracted SDL2 directory (e.g., `C:/SDL2-2.32.4`). CMake's `find_package` command will look for the configuration files in standard subdirectories (like `lib/cmake/SDL2`).
    *   Use forward slashes for paths in the command line, even on Windows.
        ```bash
        # Example command (adjust path as needed):
        cmake -B build -DWHISPER_SDL2=ON -DCMAKE_PREFIX_PATH="C:/SDL2-2.32.4"
        # Add other necessary whisper.cpp CMake flags (e.g., -DWHISPER_COREML=1)
        ```

4.  **Build the Project:**
    *   Compile the project (specifically the target needing SDL2, like `whisper-stream`):
        ```bash
        cmake --build build --config Release 
        # Or --config Debug if you configured whisper.cpp for Debug
        ```

5.  **Make `SDL2.dll` Available at Runtime:**
    *   The compiled executable (e.g., `whisper-stream.exe`) needs access to the `SDL2.dll` file when it runs.
    *   Locate `SDL2.dll` within your extracted SDL2 development library directory (usually in `lib/x64` or sometimes `bin/x64`).
    *   Copy this `SDL2.dll` file into the same directory as the built executable. For a standard Release build, this is typically:
        `./build/bin/Release/`
        ```bash
        # Example using cp in Git Bash (adjust source path):
        cp /c/SDL2-2.32.4/lib/x64/SDL2.dll ./build/bin/Release/
        ```

After completing these steps, you should be able to run the SDL2-dependent executable (e.g., `./build/bin/Release/whisper-stream.exe`) from the `whisper.cpp` root directory.

---