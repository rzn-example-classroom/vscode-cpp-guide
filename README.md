# Getting started with C++ and Visual Studio Code
## A cross-platform installation guide for students
### Introduction
This tutorial will guide you to setup Visual Studio Code for C++ development using GCC for compilation and GDB for debugging (or Clang for MacOS users). This is an adaptation of Microsoft's guides to using [GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw), [GCC on Linux](https://code.visualstudio.com/docs/cpp/config-linux), and [Clang on MacOS](https://code.visualstudio.com/docs/cpp/config-clang-mac), and also this tutorial for [GCC on Windows](https://www.freecodecamp.org/news/how-to-install-c-and-cpp-compiler-on-windows/).

You will be guided through using the terminal, shell, or command line for some operations in this tutorial. If you are not familiar with terminal operations, do not worry - there are only a few commands you need to know and execute.

Once you have successfully installed Visual Studio Code, GCC, and GDB (or Clang), you will write, compile, and debug a basic Hello World program in C++ to get used to working with these tools.

## Installation
### For all operating systems
#### Do this first, then proceed to your OS
1. Download, install, and launch [Visual Studio Code](https://code.visualstudio.com/download).
	- Additional guidance for: [Windows](https://code.visualstudio.com/docs/setup/windows), [MacOS](https://code.visualstudio.com/docs/setup/mac), [Linux](https://code.visualstudio.com/docs/setup/linux).
	- If you are on Windows, on the "Select Additional Tasks" page of the installer, you should **enable the following settings**:
		- `Add "Open with Code" action to Windows Explorer file context menu`
		- `Add "Open with Code" action to Windows Explorer directory context menu`
		- `Register Code as an editor for supported file types`
		- `Add to PATH (requires shell restart)`
		- You may also wish to create a desktop shortcut.
2. Install [Microsoft's C/C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) for VS Code. You can find this by opening the `Extensions` tab in the left sidebar (the four blocks) and searching for "C++". ![MS C++ Extension](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/cd9fe686-a9a8-4535-9649-c7e221c8ea39)

3. Proceed to the appropriate subsection below corresponding to your device's operating system.
	- If using Windows Subsystem for Linux (WSL), you may elect to follow the Linux path of this guide. However, note that your files will be created within the WSL environment, and may be difficult to access outside of the terminal.

### Windows
This guide assumes you are running a recent version of Windows (10 or 11), 64-bit. To check this, go to your Start menu, then Settings, System, and About.

You should also have at least 2 GB of free space on the drive you intend to install to (most likely `C:\`). 

#### Installing MSYS2
MSYS2 is a group of command line utilities including a package manager (`pacman`), which is how we will install GCC and its prerequisites on Windows.

- Follow Steps 1-5 of [the official installation instructions](https://www.msys2.org). 
	- The installer download link is provided in Step 1. 
		- Alternatively, you can download the latest version of the installer from [the GitHub releases page](https://github.com/msys2/msys2-installer/releases) (click the arrow next to "Assets" and download `msys2-x86_64-YYYYMMDD.exe`).
	- You may leave the installation folder at the default path (`C:\msys64`). If you change this, record the new path so you can refer to it later.
	- At the end of the installer, ensure the "Run MSYS2 Now" box is checked so that MSYS2 launches immediately.

#### Installing MinGW
With the MSYS2 terminal open, run the following command to update packages:

```shell
pacman -Syu
```

If prompted, press `Y` and `Enter` to proceed with the installation. You may need to repeat this more than once. Once everything has been downloaded, you will need to enter `Y` once more, then the terminal will close to finish updating.

To re-open MSYS2, open your Start Menu using the Windows key, then search for `MSYS2 MSYS`. Note that there may be multiple programs beginning with `MSYS2` - ensure you launch the `MSYS2 MSYS` one specifically.

To finish updates, run the following command:

```shell
pacman -Su
```

Again, if prompted, enter `Y` to continue. This may take some time.

Once you see a new shell prompt (like the one below), the updates are complete, so you can close the terminal.

```shell
user@desktop MSYS ~
$ 
```

Now, open a new `MSY2 MSYS` terminal using the same method as before. Then, enter the following command to install the MinGW toolchain, which includes GCC and GDB:

```shell
pacman -S --needed base-devel mingw-w64-x86_64-toolchain
```

Press `Enter` to accept the default number of packages. 

Note the `Total Installed Size` reported. Then enter `Y` to continue the installation. This may take some time.

Again, close the terminal once you see the new shell prompt.

#### Adding MinGW to your `PATH`
Next, you'll need to add the MinGW-w64 `bin` folder to your `PATH` environment variable. 

1. Press the Start button, and in the Start menu, search for "environment".
2. You should see `Edit the system environment variables` - click this.
3. At the bottom of the window that appears, click `Environment variables...`
4. In the `System variables` section, look for `Path`.
5. Select the row for `Path` and click `Edit...` below.
	- If this button is disabled (greyed out), you can instead edit the `Path` under `User variables`.
6. On the right, click `New`.
7. Now you will need to add the correct directory to `Path`. Assuming you installed MSYS2 to the folder `C:\msys64`, add the following path to your `Path` variable. Otherwise, replace `C:\msys64` with the directory to which you installed MSYS2.

```
C:\msys64\mingw64\bin
```

8. Select `OK` to save the updated `Path`. Close any active terminal, shell, or command prompt windows.

#### Verifying the installation
Next, open the Command Prompt by searching "cmd" in your Start menu. Then, run the following three commands:

```
gcc --version
g++ --version
gdb --version
```

The resultant output for each command should state the installed version of GCC, g++, and GDB, respectively. It should look similar to the following:

```
C:\Users\user>gcc --version
gcc (Rev7, Built by MSYS2 project) 13.1.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


C:\Users\user>g++ --version
g++ (Rev7, Built by MSYS2 project) 13.1.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


C:\Users\user>gdb --version
GNU gdb (GDB) 13.2
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

If this is the case, you're done! Continue to the next major section, "Creating a Hello World program".

If this is not the case, follow these instructions from Microsoft to resolve the issue:

1. Make sure your `PATH` variable entry matches the MinGW-w64 binary location where the toolchain was installed. If the compilers do not exist at that `PATH` entry, make sure you followed the previous instructions.
2. If `gcc` has the correct output but not `gdb`, then you need to install the packages you are missing from the MinGW-w64 toolset.
    - If on compilation you are getting the "The value of miDebuggerPath is invalid." message, one cause can be you are missing the `mingw-w64-gdb` package.
3. If you had a previous version of MSYS2 or MinGW installed, make sure to run the package manager update commands in the "Installing MinGW" section above, then remove the older version of MinGW from your system `PATH` environment variable, similar to how it is added in the "Adding MinGW to your `PATH`" section.

If this does not resolve your issue, ask your instructor for assistance.

### MacOS
MacOS is slightly different from Windows and Linux because `gdb` is incompatible with Apple Silicon Mac devices.[^2] This guide assumes you have administrator privileges on the device you wish to install on. 

[^2]: To check if you have an Intel or Apple CPU, click the Apple icon in the top left, then `About This Mac`. The `Chip` field should list either "Intel" or "Apple" as appropriate. This guide is designed to be compatible with either, but Intel Macs may be able to install GCC and g++ using Homebrew and GDB using [this method](https://dev.to/jasonelwood/setup-gdb-on-macos-in-2020-489k).

1. Open the `Terminal`.
	- From Finder, `Go -> Utilities` and scroll down.
	- Alternatively, use the spotlight search to search for `Terminal`.
2. Check if `clang` is already installed on your device by running the command `clang --version`. 
3. If running `clang --version` results in a message like "command not found", run `xcode-select --install` to install `clang`. Otherwise, continue to the next section on "Creating a Hello World program".

### Linux
This guide assumes that you are at least somewhat familiar with your distribution of Linux and the terminal. Instructions are provided for Ubuntu and Arch Linux, but should be similar for other distributions. 

Check whether GCC, g++, and gdb are already installed using the following commands. Some distributions of Linux may have these packages preinstalled.

```
gcc --version
g++ --version
gdb --version
```

If these packages are not installed (i.e., a version is not provided), you will need to install them from your package manager. Your terminal may suggest a command for you to install the packages. Otherwise, the general process is to perform a package list update, then install the requisite packages with the following commands. 

Once installed, make sure to re-run the three commands above to verify that the packages are working.

#### Ubuntu
```shell
sudo apt-get update
sudo apt-get install build-essential gdb
```

#### Arch Linux
```shell
sudo pacman -Syu
sudo pacman -S base-devel
```

## Creating a Hello World program
### Project Setup
First, you'll need to make a folder to store your program in. If you already have an existing folder for programming assignments, make a new folder called `helloworld` there. Otherwise, you can make a new folder on your desktop called `projects` (or similar). 

You may elect to create the folder(s) via your operating system's visual interface, or through the terminal/command prompt. This guide assumes you are familiar enough with your OS to do this visually. Alternatively, the next section provides instructions for the terminal method.

#### Navigation and folder creation in the terminal
If you did not open the terminal directly in the desired folder, you'll need to change the working directory to that location using `cd`. To change directory to your desktop, run a command like the one below. Note that `#` denotes a comment line in shell scripts.

```shell
# Windows
cd C:\Users\user\Desktop
# Replace 'user' with your username

# MacOS or Linux
cd ~
# The character above is a 'tilde' character (found next to '1' on most keyboards). It is a shortcut for 'home' on Linux-based OSes.
```

Then, create `projects/helloworld` as follows:

```shell
# All OSes (Windows/MacOS/Linux)
mkdir projects
cd projects
mkdir helloworld
cd helloworld
```

### Opening a VS Code workspace in your new folder
Again, you may decide to open VS Code visually, or via the terminal.

#### Visual Method
If you are on Windows, and you selected the `Add "Open with Code" action to Windows Explorer directory context menu` option in the VS Code installer, you can simply navigate to the directory you just created (if not already there), right-click, and click `Open with Code`. 

For any OS, you may also launch VS Code, click `File`, then `Open Folder`. Your OS's file browser should appear to prompt you to select a location.

#### Terminal Method
Once navigated to your newly created directory using the instructions in the previous section, you should be able to run the command below to launch VS Code in the working directory (current folder):

```shell
# '.' is the current directory
code .
```

If this command does not launch VS Code, it is most likely that VS Code is not yet on your system `PATH` environment variable. If this is the case, try these instructions:

##### Windows
Follow the instructions for "Adding MinGW to your `PATH`" above, but add the following path instead (replace `user` with your account username): ```
```
C:\Users\user\AppData\Local\Programs\Microsoft VS Code\bin
```

##### MacOS
[Per these instructions](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line), launch VS Code, press `Cmd+Shift+P`, and type `Shell Command`. Then, run `Shell Command: Install 'code' command in PATH`. Provide permission if prompted.

##### Linux
[Per these instructions](https://stackoverflow.com/a/33831403/15080675), determine the location to which VS Code has been installed. Replace `/path/to/vscode/Code` below with the absolute path to the VS Code executable, and run the command:

```shell
sudo ln -s /path/to/vscode/Code /usr/local/bin/code
```

### Creating a source file[^1]
In the File Explorer title bar, select the **New File** button and name the file `helloworld.cpp`.

![VSCode Create File](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/637fa2cf-c0a3-468c-ade6-d5b56b0805ba)


[^1]: The remainder of this tutorial directly follows Microsoft's guide.

### Adding source code
Copy and paste the following code into your new `cpp` file:

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
    vector<string> msg {"Hello", "C++", "World", "from", "VS Code", "and the C++ extension!"};

    for (const string& word : msg)
    {
        cout << word << " ";
    }
    cout << endl;
}
```

Now, press `Ctrl+S` (on Mac, this may be `Cmd+S`) to save the file. Notice your new file now appears in the `File Explorer` view of the VS Code sidebar:

![VSCode File](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/4363c27c-0246-4352-8350-dedcaaa65fce)


You can also enable [Auto Save](https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save) to automatically save your file changes, by selecting `File > Auto Save`.

### Using IntelliSense
[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) is a set of features that help you code faster and more efficiently, including code completion, documentation on hover, parameter info, and member lists.

Try it out by hovering over `vector` or `string` to see their type information. If you type `msg.` in line 10, you can see a complete list of recommended member functions to call, all generated by IntelliSense:

![VSCode IntelliSense](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/eabb91c0-5b66-4e25-8584-66df9a2557a6)

You can press the Tab key to insert a selected member. If you then add open parenthesis, IntelliSense will show information on which arguments are required.

#### Troubleshooting
If you are using Windows, and IntelliSense is not already configured or is not working for you, try opening the Command Palette (`Ctrl+Shift+P`) and enter `Select IntelliSense Configuration`. Select the appropriate compiler (GCC) from the list of available compilers.

If you are on MacOS or Linux, or the fix above did not work for you, ensure you have installed the C++ Extension pack by Microsoft for VS Code.

### Running your program
The C++ extension uses the C++ compiler and debugger installed on your machine to build and run your program, respectively. Ensure that you have properly installed `g++` and `gdb` (or `clang++`) before proceeding (see the subsection of "Installation" corresponding to your OS).

1. Open `helloworld.cpp` so that it is the active file.
2. From the drop-down menu next to the "play" button in the top right corner of the editor, select `Run C/C++ File`. ![VS Code Run File](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/bb5a075f-6aaf-4b62-8f19-77b1ad340aaf)

3. If prompted to select a compiler, select `g++` from the list of detected compilers.
	- You'll only be asked to choose a compiler the first time you run `helloworld.cpp`. This compiler will be set as the "default" compiler in the `tasks.json` file.
  - **If you are on MacOS, select `clang++`**. On Mac, `g++` is set as an alias of `clang++`, which can be confusing and is not recommended for use. ![MacOS VSCode Debug](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/233cb101-54c4-452b-acff-6eef971e85fa)
4. After the build succeeds, your program's output will appear in the integrated **Terminal**.
	- The VS Code Terminal appears at the bottom of the screen when you run a program. You can toggle its visibility using `Ctrl+J`.

Congratulations! You've just run your first C++ program in VS Code!

If you are unable to run the program, try deleting the `tasks.json` file located under the `.vscode` subdirectory. Then, continue from Step 2 again, and try a different compiler.

##### `tasks.json`
If you'd like to further understand the `tasks.json` file generated by VS Code, visit the resources below. However, this isn't necessary for the scope of this tutorial.

- [Windows](https://code.visualstudio.com/docs/cpp/config-mingw#_understanding-tasksjson)
- [MacOS](https://code.visualstudio.com/docs/cpp/config-clang-mac#_run-helloworldcpp) (Scroll down)
- [Linux](https://code.visualstudio.com/docs/cpp/config-linux) (Scroll down)

### Debugging your program
To debug your code, follow these instructions:

1. Go back to `helloworld.cpp` so that it is the active file.
2. Set a breakpoint by clicking to the left of a line number (a red dot should appear), or by pressing `F9` on the current line. ![VS Code Breakpoint](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/54358fc3-5994-49bb-b5ab-588fac29313f)

3. From the drop-down next to the play button, select `Debug C/C++ File`.
	- This button has two modes: Run and Debug. It defaults to the last used mode, and the icon changes accordingly. If you'd like to repeat your last action, just click the button instead of using the drop-down menu.
4. As with running the file, choose the appropriate debugger (`gdb` or `clang++`) from the list of compilers installed on your system.

#### Exploring the debugger
Before you start stepping through the code, let's take a moment to notice several changes in the user interface:

- The Integrated Terminal appears at the bottom of the source code editor. In the **Debug Output** tab, you see output that indicates the debugger is up and running.
- The editor highlights the line where you set a breakpoint before starting the debugger: ![VS Code Debugger](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/bfdd28de-837a-41f9-a5ab-9ddca7928299)

- The **Run and Debug** view on the left shows debugging information. You'll see an example later in the tutorial.    
- At the top of the code editor, a debugging control panel appears. You can move this around the screen by grabbing the dots on the left side.

![VS Code Debugger 2](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/e4ea60f3-6430-4b06-a9c1-b144e105f6c0)


#### Step through the code
Now you're ready to start stepping through the code.

1. Click or press the **Step over** icon in the debugging control panel. 

    ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/86e6644b-2317-415d-808f-bbe5394e3f7b)

   - This will advance program execution to the first line of the for loop, and skip over all the internal function calls within the `vector` and `string` classes that are invoked when the `msg` variable is created and initialized. Notice the change in the **Variables** window on the left. 
     
    ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/7c9b7fc2-b376-47c5-b8fc-76094135e980)
    - In this case, the errors shown are expected because the statement has not executed yet, even though the variable names are known to the debugger. Thus, there is currently nothing to read. The contents of `msg` are visible, however, because that statement has completed.
2. Press **Step over** again to advance to the next statement in this program, skipping over all the internal code that is executed to initialize the loop. Now, the **Variables** window shows information about the loop variables.
3. Press **Step over** again to execute the `cout` statement. Note that the C++ extension does not print any output to the **Debug Console** until the loop exits.
4. If you like, you can keep pressing **Step over** until all the words in the vector have been printed to the console. But if you are curious, try pressing the **Step Into** button to step through source code in the C++ standard library!
   ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/6a505d09-55ef-4b8b-b1be-296ac868d569)
    
5. To return to your own code, one way is to keep pressing **Step over**. Another way is to set a breakpoint in your code by switching to the `helloworld.cpp` tab in the code editor, putting the insertion point somewhere on the `cout` statement inside the loop, and pressing `F9`. A red dot appears in the gutter on the left to indicate that a breakpoint has been set on this line.
   ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/53aa7186-3c3b-4d43-8198-7306ced76aa3)

6. Press `F5` to start execution from the current line in the standard library header. Execution will break on `cout`. If you like, you can press `F9` again to toggle off the breakpoint.
7. When the loop has completed, you can see the output in the Integrated Terminal, along with some other diagnostic information from the debugger. The output may vary by OS, but it should look similar to this:

![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/74b92cec-f920-48e9-81f8-bb2e0c74a0a7)

#### Watching a variable
Sometimes you might want to keep track of the value of a variable as your program executes. You can do this by setting a **watch** on the variable.

1. Place the insertion point inside the loop. In the **Watch** window, click the plus sign, and in the text box, type `word`, which is the name of the loop variable. Now view the Watch window as you step through the loop.
   
    ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/b8a70cff-c008-451b-97b6-3913bc8a611e)
2. Add another watch by adding this statement before the loop: `int i = 0;`. Then, inside the loop, add this statement: `++i;`. Now add a watch for `i` as you did in the previous step.    
3. To quickly view the value of any variable while execution is paused on a breakpoint, you can hover over it with the mouse pointer.
     ![image](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/d28b159e-f4f2-4d89-a259-7005f1a09dac)

#### Custom debugging with `launch.json`
When you debug with the play button or `F5`, the C++ extension creates a dynamic debug configuration on the fly. 

If you want to specify arguments to pass to your program at runtime, for instance, you may want to define a custom debug configuration.

To create one, choose **Add Debug Configuration** from the drop-down menu next to the play button. 

![VS Code Debug Config](https://github.com/rzn-example-classroom/vscode-cpp-guide/assets/16062019/25cbf856-3df1-4bb0-a9c2-9b8ad9844ef2)

Choose the appropriate debugger from the list of those installed on your machine.

If you open `launch.json`, you will see something that looks like this (for Windows, but other OSes will be similar outside of the filepaths):

```json
{
  "configurations": [
    {
      "name": "C/C++: g++.exe build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [], // Add arguments here!
      "stopAtEntry": false,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\msys64\\mingw64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        },
        {
          "description": "Set Disassembly Flavor to Intel",
          "text": "-gdb-set disassembly-flavor intel",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe build active file"
    }
  ],
  "version": "2.0.0"
}
```

To add arguments, modify the array associated with the key `"args"`. Entries of this array should be strings, even if your arguments are numeric.

To learn more about `launch.json`, see these resources:

- [Windows](https://code.visualstudio.com/docs/cpp/config-mingw#_customize-debugging-with-launchjson)
- [MacOS](https://code.visualstudio.com/docs/cpp/config-clang-mac#_customize-debugging-with-launchjson)
- [Linux](https://code.visualstudio.com/docs/cpp/config-linux#_customize-debugging-with-launchjson)

## Conclusion
Congratulations! In this tutorial, you've set up a C++ development environment with Visual Studio Code on your machine and gained experience with the VS Code workflow for C++, including debugging.

If your instructor has provided any next steps or another assignment, follow their instructions now. Otherwise, you are encouraged to continue experimenting with C++ in VS Code and using the debugger on your own.
