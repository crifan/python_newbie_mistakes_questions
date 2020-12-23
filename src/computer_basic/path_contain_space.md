# 路径中包含空格

很多初学者在使用Python期间，尤其是`Windows`环境下，常会遇到：

给命令行或代码参数中传递路径时，**路径中包含了空格**

其不知道路径中的空格，会导致实际上传递的参数，已经被空格分开为多个部分，因而出现找不到子路径等异常情况。

举例：

## pyinstaller打包时路径带空格导致异常

[python中pyinstaller打包时出现的问题，跪求大神帮助-CSDN论坛](https://bbs.csdn.net/topics/398452775)

某人用`PyInstaller`去打包python程序，用命令：

```bash
C:\Users\Administrator>pyinstaller -F D:\python VIP\chap16\stusystem
```

结果出错：

```bash
39 INFO: PyInstaller: 4.1
39 INFO: Python: 3.9.0
39 INFO: Platform: Windows-10-10.0.18362-SP0
40 INFO: wrote C:\Users\Administrator\python.spec
41 INFO: UPX is not available.
script 'C:\Users\Administrator\VIP\chap16\stusystem' not found
```

其中很明显就是：

`-F`参数所传入的路径`D:\python VIP\chap16\stusystem`中间有空格

导致实际结果相当于：

```bash
C:\Users\Administrator>pyinstaller -F D:\python
```

而此处很明显Windows中只存在目录`D:\python VIP`，而（估计）不存在`D:\python`

所以导致最后报错找不到相关目录：

```bash
script 'C:\Users\Administrator\VIP\chap16\stusystem' not found
```

**根本原因**：

各种系统（Windows、Linux、Mac等）中的路径，往往是通过空格去区分参数的

```bash
your_command parameter1 parameter2
```

不论是：

* 命令行环境
* 代码运行环境

中，所以，如果路径中有空格，往往会导致路径被空格区分开，变成多个参数，导致传入的路径本身不对，且后续其他参数也不正常了，导致结果异常

对于此处的`pyinstaller`的[命令行参数语法](https://pyinstaller.readthedocs.io/en/stable/man/pyinstaller.html)是：

```bash
~  pyinstaller --help
usage: pyinstaller [-h] [-v] [-D] [-F] [--specpath DIR] [-n NAME]
                   [--add-data <SRC;DEST or SRC:DEST>]
                   [--add-binary <SRC;DEST or SRC:DEST>] [-p DIR]
                   [--hidden-import MODULENAME]
                   [--additional-hooks-dir HOOKSPATH]
                   [--runtime-hook RUNTIME_HOOKS] [--exclude-module EXCLUDES]
                   [--key KEY] [-d {all,imports,bootloader,noarchive}] [-s]
                   [--noupx] [--upx-exclude FILE] [-c] [-w]
                   [-i <FILE.ico or FILE.exe,ID or FILE.icns>]
                   [--version-file FILE] [-m <FILE or XML>] [-r RESOURCE]
                   [--uac-admin] [--uac-uiaccess] [--win-private-assemblies]
                   [--win-no-prefer-redirects]
                   [--osx-bundle-identifier BUNDLE_IDENTIFIER]
                   [--runtime-tmpdir PATH] [--bootloader-ignore-signals]
                   [--distpath DIR] [--workpath WORKPATH] [-y]
                   [--upx-dir UPX_DIR] [-a] [--clean] [--log-level LEVEL]
                   scriptname [scriptname ...]
...
```

此处如果输入：

```bash
pyinstaller -F D:\python VIP\chap16\stusystem
```

其实变成了：

* 参数1：`-F D:\python`
* 参数2：`VIP\chap16\stusystem`

对应着：

* `-F`参数的值是：`D:\python`
* `scriptname`参数的值是：`VIP\chap16\stusystem`

很明显，不是我们希望的结果了，就会导致异常报错了。

**解决办法**：尤其是命令行操作时，或者代码调用传入的路径时，要确保传入的路径中不能包含空格

如果路径中包含空格，则可以**用（双）引号括起来**：

```bash
pyinstaller -F "D:\python VIP\chap16\stusystem"
```

这样就是我们希望的效果了：

* 参数1：`-F "D:\python VIP\chap16\stusystem"`

即：

* `-F`参数的值是：`D:\python VIP\chap16\stusystem`

即可正常运行。
