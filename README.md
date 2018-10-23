# ffmpeg-libs
The following outlines the compilation of ffmpeg DLL's and executables using the
Microsoft Visual Studio compiler.

1. Download Microsoft Visual Studio (Community edition) from 
[Microsft Website](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
and install it.

2. Download MSYS2 x68_64 from [link](http://msys2.github.io/) and install it into `C:\` folder

3. Install build tools in msys64
    ```bash
        cd C:\msys64
        Execute msys2_shell.cmd
    ```
    Execute in the msys2 shell
    ```bash
        pacman -S make gcc diffutils
    ```
    Close the shell.
    To avoid a name conflict rename `C:\msys64\usr\bin\link.exe` into
    `C:/msys64/usr/bin/link_old.exe`. This ensures that your compile environment will 
    use the linker from the Microsoft Visual Studio.

4. Install YASM

    Download `Win64.exe` from [link](http://yasm.tortall.net/Download.html) and rename
    the downloaded executable to `yasm.exe`. Create the folder `C:\yasm` and place the renamed file into 
    this latter. Add this folder to your path.

5. Setup the environment variables for the 64 bit build
 
    Run the following batch script in the 
    windows command line tool:
    ```bash
        cd C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC
        vcvarsall.bat x64
    ```
    The flag x64 sets the environment up for compilation of 64 bit versions.

6. Switch to the MSYS shell

    In the windows command line from the prior step navigate to C:\msys64.
    Run the command: msys2_shell.cmd -msys -use-full-path
    This ensures that the environment variables PATH and LIB carry over into the 
    msys shell. This opens a new msys shell in a separate window.

7. In the msys shell CHECK the compiler and linker:
    ```bash
        $ which cl
        /c/Program Files (x86)/Microsoft Visual Studio 14.0/VC/BIN/amd64/cl
        $ which link
        /c/Program Files (x86)/Microsoft Visual Studio 14.0/VC/BIN/amd64/link
    ```
    This ensures that your MVS compiler and linker are used.

8. Download FFmpeg source code

    Download the ZIP file of the latest FFmpeg source code 
    from [the Github Repository](https://github.com/FFmpeg/FFmpeg/releases)
    and unzip the file into the folder FFmpeg-release-x.x.x.

9. Configure for compilation of 64 bit DLLs
    In the msys shell: 
    ```bash
       cd /c/PATH/TO/FFMPEG_SOURCE/
    ``` 
    Run the following command to configure the build for FFmpeg:
    ```bash
    ./configure --toolchain=msvc 
                   --enable-swscale 
                   --enable-asm 
                   --enable-yasm 
                   --target-os=win64 
                   --arch=x86_64 
                   --enable-shared 
                   --disable-static 
                   --disable-ffserver 
                   --disable-avdevice 
                   --disable-doc 
                   --disable-ffplay 
                   --disable-ffprobe 
                   --disable-ffmpeg 
                   --enable-w32threads    
    ``` 
10. A successful configuration ends with the lines

    License: LGPL version 2.1 or later
    Creating config.mak, config.h, and doc/config.texi...
    config.asm is unchanged
    libavutil/avconfig.h is unchanged
    WARNING: pkg-config not found, library detection may fail.
    libavcodec/bsf_list.c is unchanged
    libavformat/protocol_list.c is unchanged

    You can ignore the pkg-config not found issue. It will not affect compilation.

11. Compile the source code to produce DLLs

    ```bash
        make -j4
    ```
    The -j4 flag indicates to use 4 threads for compilation. This 
    speeds up the compilation process. You may want to set the number 
    of threads even higher depending on your hardware.

12. The successful compilation ends with.

    ```bash
        LD      libavdevice/avdevice-57.dll
           Creating library libavdevice/avdevice.lib and object libavdevice/avdevice.exp
        LD      ffmpeg_g.exe
        LD      ffprobe_g.exe
        CP      ffprobe.exe
        STRIP   ffprobe.exe
        skipping strip ffprobe.exe
        CP      ffmpeg.exe
        STRIP   ffmpeg.exe
        skipping strip ffmpeg.exe
    ```

## Resources
Useful installation guides:
- https://pracucci.com/compile-ffmpeg-on-windows-with-visual-studio-compiler.html
- https://trac.ffmpeg.org/wiki/CompilationGuide/MSVC
- http://ffmpeg.org/platform.html
