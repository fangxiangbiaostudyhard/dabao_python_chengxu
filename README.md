# YOLO 打包程序教程（Windows + PyInstaller + Conda 环境）

本文档详细介绍了如何在 Windows 上使用 **PyInstaller** 对基于 **YOLO/Ultralytics** 的 Python 项目进行打包，确保程序在独立环境下可以运行。

---

## 1. 环境准备

1. 安装 **Miniconda** 或 **Anaconda**，并创建项目专用环境（假设名为 `yolo`）：

```bash
conda create -n yolo python=3.10
conda activate yolo
```

2. 安装项目所需依赖，例如：

```bash
pip install ultralytics opencv-python matplotlib torch torchvision zipp ...
```

> 注意：在打包前请确保在该环境下可以正常运行 Python 脚本。

3. 检查环境中已安装的库：

```bash
pip list
```

---

## 2. 获取库路径

为了 PyInstaller 正确打包，需要指定依赖库的路径。获取方法如下：

1. 获取 Conda 环境路径（第一个路径）：

```bash
pip show zipp
```

* 输出示例：

```
Location: C:\miniconda3\envs\yolo\lib\site-packages
```

* 该路径就是环境中库的路径，**用于 --paths 参数**。

2. 获取项目自定义库路径（第二个路径，无法直接使用的库路径）：

```bash
pip show ultralytics
```

* 输出示例：

```
Location: D:\dianlan_project\yolo11-up\yolo11-up
```

* 该路径也需要通过 `--paths` 参数传给 PyInstaller。

---

## 3. 打包命令

在 **项目目录下**打开 CMD（命令行），执行以下命令进行打包。

### 示例 1：多行命令（可读性强）

```cmd
pyinstaller --noconfirm --clean --windowed --onedir --noupx ^
--name DIANLANQUEXIANJIANCE ^
--paths "C:\miniconda3\envs\yolo\lib\site-packages" ^
--paths "D:\dianlan_project\yolo11-up\yolo11-up" ^
AAA_jietu_jiemain_xin2.py
```

### 示例 2：单行命令（直接复制运行）

```cmd
pyinstaller --noconfirm --clean --windowed --onedir --noupx --name DIANLANQUEXIANJIANCE --paths "C:\miniconda3\envs\yolo\lib\site-packages" --paths "D:\dianlan_project\yolo11-up\yolo11-up" AAA_lingmindu.py
```

---

## 4. 参数说明

| 参数            | 说明                                    |
| ------------- | ------------------------------------- |
| `--noconfirm` | 自动确认覆盖现有文件，无需手动确认                     |
| `--clean`     | 打包前清理临时文件，避免缓存问题                      |
| `--windowed`  | 打包为无控制台窗口的 GUI 程序（适用于 Tkinter、PyQt 等） |
| `--onedir`    | 打包为单个文件夹（目录）形式，便于调试                   |
| `--noupx`     | 禁用 UPX 压缩，避免部分防病毒误报                   |
| `--name`      | 打包后的程序名称                              |
| `--paths`     | 指定额外库的路径，确保 PyInstaller 能找到依赖         |
| `<入口文件>`      | 项目的主脚本，打包程序从这里启动                      |

---

## 5. 打包注意事项

1. **CMD 执行目录**
   必须在项目目录下执行命令，确保所有依赖文件（模型、配置文件等）在相对路径下可用。

2. **路径顺序**

   * 第一个路径：Conda 环境库路径
   * 第二个路径：自定义库或无法直接引用的库路径

3. **验证打包结果**

   * 打包完成后，检查 `dist/DIANLANQUEXIANJIANCE` 文件夹
   * 直接运行 `AAA_jietu_jiemain_xin2.exe` 或 `AAA_lingmindu.exe`，确保程序正常启动

4. **常见问题**

   * **库找不到**：确认 `--paths` 参数中路径正确
   * **缺少 DLL**：检查 PyTorch/CUDA 是否正确安装
   * **程序无法启动**：在 `--onedir` 模式下，可先尝试 `--console` 查看错误输出

---

## 6. 总结流程

1. 创建并激活 Conda 环境
2. 安装项目依赖
3. 获取 Conda 库路径和自定义库路径
4. 打开 CMD，在项目目录执行 PyInstaller 命令
5. 检查 `dist` 文件夹中的打包结果，运行程序验证

