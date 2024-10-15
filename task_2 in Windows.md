# Windows 下从源码编译
在Windows下提供一种编译方式：  

- **在本机编译**

 **| 环境准备**
 - Windows 7/8/10 专业版/企业版 (64bit)
- Python 版本 3.8/3.9/3.10/3.11/3.12 (64 bit)
- Visual Studio 2017/2019 社区版/专业版/企业版

**| 选择CPU**
- 如果你的计算机硬件没有 NVIDIA® GPU，请编译 CPU 版本的 PaddlePaddle

**| 本机编译过程**
1. 安装必要的工具 cmake, git, python, Visual studio 2019：           
    > cmake：建议安装 CMake3.17 版本，官网下载[链接](https://cmake.org/files/v3.17/cmake-3.17.0-win64-x64.msi)。安装时注意勾选 `Add CMake to the system PATH for all users` ，将 CMake 添加到环境变量中。

    > git：官网下载[链接](https://github.com/git-for-windows/git/releases/download/v2.35.1.windows.2/Git-2.35.1.2-64-bit.exe)，使用默认选项安装。

    > python：官网[链接](https://www.python.org/downloads/windows/)，可选择 3.8/3.9/3.10/3.11/3.12 中任一版本的 Windows installer(64-bit)安装。安装时注意勾选 `Add Python 3.x to PATH` ，将 Python 添加到环境变量中。

    > Visual studio：VS2017 仅用于 CPU 版编译，建议安装 VS2019。官网[链接](https://visualstudio.microsoft.com/zh-hans/vs/older-downloads/)，需要登录后下载，建议下载 Community 社区版。在安装时需要在工作负荷一栏中勾选 使用 `C++的桌面开发` 和 `通用 Windows 平台开发`，并在语言包一栏中选择 `英语`。
    >>TODO：2019安装包，斜体

2. 打开 Visual studio 终端：在 Windows 桌面下方的搜索栏中搜索终端，若安装的是 VS2019 版本，则搜索 `x64 Native Tools Command Prompt for VS 2019` 或 适用于 `VS 2019 的 x64 本机工具命令提示符`，然后右键以管理员身份打开终端。后续的命令将在该终端执行。

3. 使用 `pip` 命令安装 Python 依赖：
    - 通过 `python --version` 检查默认 python 版本是否是预期版本，因为你的计算机可能安装有多个 python，可通过修改系统环境变量的顺序来修改默认 Python 版本。
    - 安装 numpy, protobuf, wheel, ninja
        ```bash
        pip install numpy protobuf wheel ninja
        ```

4. 创建编译 Paddle 的文件夹（例如 D:\workspace），进入该目录并下载源码：
    ```bash
    mkdir D:\workspace 
    cd /d D:\workspace
    git clone https://github.com/PaddlePaddle/Paddle.git
    cd Paddle
    ```
    ***注意：`git clone`克隆仓库时常常会出现网络问题，此时应：***
    - 询问大语言模型，如gpt。
    - 使用镜像源，如`https://ghp.ci/`，其它镜像源自行询问gpt。


5. 创建名为 build 的目录并进入：
    ```bash
    mkdir build
    cd build
    ```

6. 执行 cmake：
编译 CPU 版本的 Paddle：
    ```bash
    cmake .. -GNinja -DWITH_GPU=OFF -DWITH_UNITY_BUILD=ON
    ```

