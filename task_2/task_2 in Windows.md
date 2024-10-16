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
    >>[VS2019安装包](https://github.com/ChengNanFlower/PP-warm-up/tree/main/task_2/vs_community__2019.exe   )已放入当前目录下

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
    - 使用镜像源，如 `https://ghp.ci/` ，其它镜像源及使用方式自行询问gpt。


5. 创建名为 build 的目录并进入：
    ```bash
    mkdir build
    cd build
    ```

    ### ! Warning !：因未知原因， `cmake` 与 `ninja` 很可能出现报错并且无法解决。此时，应删除 `build` 下所有文件，并从 `git clone` 重新开始。😡

6. 创建timecmd.txt，将[脚本](https://github.com/PaddlePaddle/Paddle/issues/45347#issuecomment-1320810399)复制并粘贴到txt文件，并将文件格式改为bat：
    ```bash
    echo. > timecmd.txt
    start timecmd.txt
    # 将脚本复制上去并保存
    ren timecmd.txt timecmd.bat
    ```

7. 执行 cmake：
    编译 CPU 版本的 Paddle：
    ```
    timecmd.bat cmake .. -GNinja -DWITH_GPU=OFF -DWITH_UNITY_BUILD=ON 
    ```
    ***注意：请在 `cmake`前禁用你之前的编译器，如 `mingw`，否则可能导致编译错误。***

8. 执行编译：
    ```bash
    # 将pip3.X改为你的python版本号，如使用python3.10，即改为pip3.10

    pip3.X install -r ../python/requirements.txt
    timecmd.bat ninja 
    ```
    *若编译错误，请 `ninja clean`*

9. 编译成功后进入 `python\dist` 目录下找到生成的 `.whl` 包：
    ```bash
    cd python\dist
    dir
    ```

10. 安装编译好的 `.whl` 包：
    ```bash
    # 请去掉[]
    pip install [whl包的名字] --force-reinstall
    ```
    恭喜，至此你已完成 PaddlePaddle 的编译安装!😊

**| 验证安装**

- 安装完成后你可以使用 `python` 进入 python 解释器，输入:   

    ```bash
    python
    ```
    ```python
    >>> import paddle
    ```
    ```python
    >>> paddle.utils.run_check()
    ```
    如果出现PaddlePaddle is installed successfully!，说明你已成功安装。👍

**| 如何卸载**
- 请使用以下命令卸载 PaddlePaddle：
    ```bash
    # CPU 版本的 PaddlePaddle:
    pip uninstall paddlepaddle
    ```
