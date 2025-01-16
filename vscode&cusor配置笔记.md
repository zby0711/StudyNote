# 一.一些编译相关内容的解释

## 1. vscode的配置文件



### tasks.json

command：需要在命令行执行的命令

args：命令的参数，一个list，里面的选项都会添加到命令中

task是任务

```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: gcc 生成活动文件",
			"command": "/usr/bin/gcc",
			"args": [
				"-fdiagnostics-color=always",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"detail": "编译器: /usr/bin/gcc"
		}
	]
}

```

### launch.json

启动可执行文件（首先需要使用tasks.json生成可执行文件）

下面面是默认生成的launch文件，上面是默认生成的，没法直接用，也可以从运行->添加配置里面生成配置文件

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": []
}


```



### settings.json

### c_cpp_properties.json

#### 作用

1. **编译器路径**：指定使用的编译器路径。
2. **包含路径**：指定头文件的搜索路径。
3. **宏定义**：定义预处理宏。
4. **C/C++ 标准**：指定使用的 C/C++ 标准版本。
5. **IntelliSense 配置**：配置 IntelliSense 的行为，如自动补全、代码导航等。

#### 配置方法

1. **自动生成**：在 VSCode 中打开 C/C++ 文件时，C/C++ 扩展会自动生成一个默认的 `c_cpp_properties.json` 文件。
2. **手动编辑**：可以通过以下步骤手动编辑该文件：
   - 打开命令面板 (`Ctrl+Shift+P` 或 `Cmd+Shift+P`)。
   - 输入并选择 `C/C++: Edit Configurations (UI)`，通过 UI 界面配置。
   - 或者直接在工作区的 `.vscode` 文件夹中找到并编辑 `c_cpp_properties.json` 文件。

#### 示例

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/include",
                "/usr/local/include"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        },
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/local/include"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        },
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/MinGW/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "C:/MinGW/bin/gcc.exe",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```

## 2.vscode配置相关

### 1.任务类型

### **1. `shell`**

- **作用**：在终端中运行命令。

- **适用场景**：需要执行命令行工具或脚本的任务（如编译、构建、运行脚本等）。

- **特点**：

  - 命令会在 VSCode 的集成终端中运行。
  - 支持传递参数和环境变量。
  - 可以运行系统命令（如 `mkdir`、`gcc`、`npm` 等）。

- **示例**：

  json

  复制

  ```json
  {
      "label": "build",
      "type": "shell",
      "command": "g++",
      "args": ["-o", "output", "input.cpp"]
  }
  ```

------

### **2. `process`**

- **作用**：直接运行一个可执行文件或程序。

- **适用场景**：运行编译后的可执行文件或不需要终端的任务。

- **特点**：

  - 不会在终端中运行，而是直接启动一个进程。
  - 适用于运行 GUI 程序或不需要终端输入/输出的任务。
  - 无法传递复杂的命令行参数（如管道、重定向等）。

- **示例**：

  json

  复制

  ```json
  {
      "label": "run",
      "type": "process",
      "command": "./output.exe"
  }
  ```

------

### **3. `npm`**

- **作用**：运行 npm 脚本。

- **适用场景**：Node.js 项目中的 npm 脚本（如 `npm run build`）。

- **特点**：

  - 自动检测项目中的 `package.json` 文件。
  - 支持运行 `npm` 命令（如 `install`、`run` 等）。
  - 无需手动指定 `command`，直接通过 `script` 指定 npm 脚本。

- **示例**：

  json

  复制

  ```json
  {
      "label": "build",
      "type": "npm",
      "script": "build"
  }
  ```

------

### **4. `typescript`**

- **作用**：运行 TypeScript 编译器 (`tsc`)。

- **适用场景**：编译 TypeScript 项目。

- **特点**：

  - 自动检测项目中的 `tsconfig.json` 文件。
  - 支持 TypeScript 的编译选项。
  - 无需手动指定 `command`，直接通过 `tsconfig` 指定配置文件。

- **示例**：

  json

  复制

  ```json
  {
      "label": "compile",
      "type": "typescript",
      "tsconfig": "tsconfig.json"
  }
  ```

------

### **5. `gulp`**

- **作用**：运行 Gulp 任务。

- **适用场景**：使用 Gulp 作为构建工具的项目。

- **特点**：

  - 自动检测项目中的 `gulpfile.js` 文件。
  - 支持运行 Gulp 任务。
  - 无需手动指定 `command`，直接通过 `task` 指定 Gulp 任务。

- **示例**：

  json

  复制

  ```json
  {
      "label": "build",
      "type": "gulp",
      "task": "build"
  }
  ```

------

### **6. `grunt`**

- **作用**：运行 Grunt 任务。

- **适用场景**：使用 Grunt 作为构建工具的项目。

- **特点**：

  - 自动检测项目中的 `Gruntfile.js` 文件。
  - 支持运行 Grunt 任务。
  - 无需手动指定 `command`，直接通过 `task` 指定 Grunt 任务。

- **示例**：

  json

  复制

  ```json
  {
      "label": "build",
      "type": "grunt",
      "task": "build"
  }
  ```

------

### **7. `docker-build`**

- **作用**：构建 Docker 镜像。

- **适用场景**：使用 Docker 进行容器化开发的项目。

- **特点**：

  - 自动检测项目中的 `Dockerfile` 文件。
  - 支持构建 Docker 镜像。
  - 无需手动指定 `command`，直接通过 `dockerBuild` 指定构建选项。

- **示例**：

  json

  复制

  ```json
  {
      "label": "docker-build",
      "type": "docker-build",
      "dockerBuild": {
          "tag": "my-image:latest"
      }
  }
  ```

------

### **8. `docker-run`**

- **作用**：运行 Docker 容器。

- **适用场景**：使用 Docker 进行容器化开发的项目。

- **特点**：

  - 支持运行 Docker 容器。
  - 无需手动指定 `command`，直接通过 `dockerRun` 指定运行选项。

- **示例**：

  json

  复制

  ```json
  {
      "label": "docker-run",
      "type": "docker-run",
      "dockerRun": {
          "image": "my-image:latest"
      }
  }
  ```

------

### **9. `custom`**

- **作用**：自定义任务类型。

- **适用场景**：需要自定义任务行为的场景。

- **特点**：

  - 允许用户完全自定义任务的行为。
  - 需要编写自定义的扩展或脚本来支持。

- **示例**：

  json

  复制

  ```json
  {
      "label": "custom-task",
      "type": "custom",
      "command": "my-custom-script.sh"
  }
  ```

## 3. 编译知识总结

### （1）gcc和g++

- **`gcc`**：
  - 默认只链接 **C 标准库**（如 `libc`）。
  - 如果编译 C++ 代码，需要手动链接 C++ 标准库（如 `libstdc++`）。
- **`g++`**：
  - 默认链接 **C++ 标准库**（如 `libstdc++`）。
  - 在编译 C++ 代码时，`g++` 会自动处理 C++ 标准库的链接。

### （2）cl.exe

`cl.exe` 是 Microsoft Visual C++ (MSVC) 编译器的核心组件，用于编译 C 和 C++ 代码。它是 Microsoft Visual Studio 工具链的一部分，主要用于 Windows 平台上的 C/C++ 开发。

### （3）clang

`Clang` 是一个开源的 C、C++ 和 Objective-C 编译器前端，属于 **LLVM 项目** 的一部分。它被设计为高性能、模块化且易于扩展的编译器，广泛用于开发工具链、静态分析和代码生成等领域。

### （4）qt+minGw编译

### （5）qt安装目录下的预编译库和编译工具链

#### 1. **`Qt\5.15.2\mingw_64`**

- **用途**：这是Qt框架的预编译库和工具链，专门用于使用MinGW 64位编译器开发和构建Qt应用程序。

#### 2.**`Qt\Tools\mingw_64`**

这是独立的MinGW 64位编译器工具链，用于编译和链接Qt应用程序。

#### 3.使用msvc的预编译库时，需要使用visual studio提供的编译工具链来编译和构建代码

#### 1. **为什么需要Visual Studio的编译器工具链？**

- **MSVC预编译库** 是针对MSVC编译器生成的，因此必须使用MSVC编译器（如`cl.exe`）来编译和链接代码。
- **Visual Studio** 是微软的官方开发环境，内置了MSVC编译器工具链（包括`cl.exe`、`link.exe`等），因此需要安装Visual Studio来获取这些工具。

------

#### 2. **具体步骤**

##### (1) **安装Visual Studio**

- 下载并安装 **Visual Studio**（建议使用2017、2019或2022版本）。
- 在安装时，选择 **C++开发工具**（包括MSVC编译器、Windows SDK等）。
- 确保安装的MSVC版本与Qt的MSVC预编译库版本匹配（例如，Qt的`msvc2019_64`需要Visual Studio 2019的MSVC工具链）。

##### (2) **配置Qt项目**

- 打开Qt Creator或Visual Studio。
- 在Qt Creator中：
  - 进入 **Kits** 设置，确保选择了正确的MSVC工具链（如`Desktop Qt 5.15.2 MSVC2019 64bit`）。
  - 确保Qt Creator能够检测到Visual Studio的MSVC编译器（通常会自动检测）。
- 在Visual Studio中：
  - 使用 **Qt VS Tools** 插件来配置Qt项目。
  - 确保项目设置中选择了正确的Qt版本和MSVC工具链。

##### (3) **编译和构建**

- 使用MSVC编译器（`cl.exe`）编译代码。
- 使用MSVC链接器（`link.exe`）链接Qt预编译库。
- 生成的应用程序将依赖MSVC运行时库（如`MSVCRT`）。

------

#### 3. **注意事项**

- **MSVC运行时库**：使用MSVC编译的应用程序需要依赖MSVC运行时库（如`vcruntime140.dll`）。这些库可以通过Visual Studio安装，也可以单独分发。
- **Qt版本匹配**：确保Qt的MSVC预编译库版本与Visual Studio的MSVC工具链版本一致（例如，`msvc2019_64`对应Visual Studio 2019）。
- **环境变量**：在使用命令行构建时，需要正确配置MSVC的环境变量（可以通过Visual Studio的`Developer Command Prompt`自动设置）。

------

#### 4. **替代方案**

如果你不想安装完整的Visual Studio，可以尝试以下方法：

- **独立MSVC工具链**：安装Visual Studio Build Tools，它只包含MSVC编译器工具链，而不包含完整的IDE。
- **MinGW**：如果你不想使用MSVC，可以选择Qt的MinGW版本，它使用GCC编译器，无需Visual Studio。

**编译过程详解**

1. **生成 Makefile**：

   - `qmake` 根据 `.pro` 文件生成 `Makefile`。

   - `.pro` 文件示例：

     plaintext

     复制

     ```
     QT += core gui widgets
     SOURCES += main.cpp
     TARGET = QtDemo
     ```

2. **编译源代码**：

   - `mingw32-make` 调用 MinGW 的 `g++` 编译器编译源代码。
   - 生成目标文件（`.o`）和最终的可执行文件（`.exe`）。

3. **链接 Qt 库**：

   - 在链接阶段，`g++` 将目标文件与 Qt 库（如 `QtCore`、`QtGui`）链接。

------

**1. 作用：**

- **编译 C/C++ 代码**：将 C/C++ 源代码文件（如 `.c` 和 `.cpp`）编译为目标文件（`.obj`）。
- **链接目标文件**：将目标文件链接为可执行文件（`.exe`）或动态链接库（`.dll`）。
- **支持多种编译选项**：提供丰富的编译选项，用于优化、调试和多平台支持。



# 经验记录

## 1.头文件引入报错

1.在配置qt开发环境时，需要进行配置，这一步主要是进行头文件的引入和编译器的指定(点击配置会自动生成c_cpp_properties.json文件)

![image-20250116091802352](D:\work\studyProj\StudyNote\vscode&cusor配置笔记.assets\image-20250116091802352.png)

c_cpp_properties.json

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/software/Qt/5.15.2/mingw81_64/include/**",
                "D:/software/Qt/5.15.2/mingw81_64/include/QtWidgets"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "D:/software/Qt/Tools/mingw810_64/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "gnu++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```

## 2.vscode+qt+qmake+mingw配置示例

### tasks.json

#### task1

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "mkdir",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "command": "mkdir",
            "args": [
                "-Force",
                "build"
            ]
        },
        {
            "label": "qmake-debug",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "command": "D:/software/Qt/5.15.2/mingw81_64/bin/qmake.exe",
            "args": [
                "../${workspaceFolderBasename}.pro",
                "-spec",
                "win32-g++",
                "\"CONFIG+=debug\"",
                "\"CONFIG+=console\""
            ],
            "dependsOn": [
                "mkdir"
            ],
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "make-debug",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "command": "D:/software/Qt/Tools/mingw810_64/bin/mingw32-make.exe",
            "args": [
                "-f",
                "Makefile.Debug",
                "-j7"
            ],
            "dependsOn": [
                "qmake-debug"
            ]
        },
        {
            "label": "run-debug",
            "type": "process",
            "options": {
                "cwd": "${workspaceFolder}/build/debug"
            },
            "command": "${workspaceFolderBasename}.exe",
            "dependsOn": [
                "make-debug"
            ]
        },
        {
            "label": "qmake-release",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "command": "qmake",
            "args": [
                "../${workspaceFolderBasename}.pro",
                "-spec",
                "win32-g++",
                "\"CONFIG+=qtquickcompiler\""
            ],
            "dependsOn": []
        },
        {
            "label": "make-release",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "command": "mingw32-make",
            "args": [
                "-f",
                "Makefile.Release",
                "-j7"
            ],
            "dependsOn": [
                "qmake-release"
            ]
        },
        {
            "label": "run-release",
            "type": "process",
            "options": {
                "cwd": "${workspaceFolder}/build/release"
            },
            "command": "${workspaceFolderBasename}.exe",
            "dependsOn": [
                "make-release"
            ]
        },
        {
            "label": "clean",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/build"
            },
            "command": "mingw32-make",
            "args": [
                "clean"
            ]
        }
    ]
}

```

#### task2

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "mkdir",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}"
            },
            "command": "mkdir",
            "args": [
                "-Force",
                "../build"
            ]
        },
        {
            "label": "qmake-debug",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/../build"
            },
            "command": "D:/software/Qt/5.15.2/mingw81_64/bin/qmake.exe",
            "args": [
                "../${workspaceFolderBasename}/${workspaceFolderBasename}.pro",
                "-spec",
                "win32-g++",
                "\"CONFIG+=debug\"",
                "\"CONFIG+=console\""
            ],
            "dependsOn": [
                "mkdir"
            ],
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
        {
            "label": "make-debug",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/../build"
            },
            "command": "D:/software/Qt/Tools/mingw810_64/bin/mingw32-make.exe",
            "args": [
                "-f",
                "Makefile.Debug",
                "-j7"
            ],
            "dependsOn": [
                "qmake-debug"
            ]
        },
        {
            "label": "run-debug",
            "type": "process",
            "options": {
                "cwd": "${workspaceFolder}/../build/debug"
            },
            "command": "${workspaceFolderBasename}.exe",
            "dependsOn": [
                "make-debug"
            ]
        },
        {
            "label": "qmake-release",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/../build"
            },
            "command": "qmake",
            "args": [
                "../${workspaceFolderBasename}/${workspaceFolderBasename}.pro",
                "-spec",
                "win32-g++",
                "\"CONFIG+=qtquickcompiler\""
            ],
            "dependsOn": []
        },
        {
            "label": "make-release",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/../build"
            },
            "command": "mingw32-make",
            "args": [
                "-f",
                "Makefile.Release",
                "-j7"
            ],
            "dependsOn": [
                "qmake-release"
            ]
        },
        {
            "label": "run-release",
            "type": "process",
            "options": {
                "cwd": "${workspaceFolder}/../build/release"
            },
            "command": "${workspaceFolderBasename}.exe",
            "dependsOn": [
                "make-release"
            ]
        },
        {
            "label": "clean",
            "type": "shell",
            "options": {
                "cwd": "${workspaceFolder}/../build"
            },
            "command": "mingw32-make",
            "args": [
                "clean"
            ]
        }
    ]
}
```



### c_cpp_properties.json

```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/software/Qt/5.15.2/mingw81_64/include/**",
                "D:/software/Qt/5.15.2/mingw81_64/include/QtWidgets"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "D:/software/Qt/Tools/mingw810_64/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "gnu++17",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```

