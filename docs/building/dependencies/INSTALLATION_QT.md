# ⚙️ Building Qt Guide
CaptureMoment requires Qt 6.10.2 with the qtdeclarative and qtshadertools modules. You can install it using the official graphical installer or via the command-line tool aqtinstall.

## List of modules for CaptureMoment
* Core
* Quick
* Gui
* GuiPrivate
* ShaderTools

## Method 1: Qt Maintenance Tool (Official Installer, recommended for Windows & macOS)

This is the easiest method if you are setting up a development environment on your own machine.

* Download the Qt Online Installer from the official website Qt for your operating system.
> Recommended for Windows & macOS

## Method 2: aqtinstall (Command Line, recommended for Linux)
Recommended for Linux

### Install aqtinstall with pip

```bash
pip install aqtinstall
```

>💡 Linux note: on recent distributions, pip refuses to install outside a virtual environment, you can use venv or force install.
```bash
pip install --break-system-packages aqtinstall
```

To get familiar with aqtinstall, please refer to the [Website](https://aqtinstall.readthedocs.io/en/latest/getting_started.html)

### Install Qt with aqt

aqt installs into the current directory if no output is specified, which makes `CMAKE_PREFIX_PATH` unpredictable. Prefer an explicit output directory:
```bash
aqt install-qt linux desktop 6.10.2 linux_gcc_64 --outputdir /opt/qt -m qtdeclarative -m qtshadertools
```

Or install into the current directory if you prefer to manage the path yourself:
```bash
aqt install-qt linux desktop 6.10.2 linux_gcc_64 -m qtdeclarative -m qtshadertools
```

### Environment Configuration (recommended)
If you have multiple version of Qt, it is better to define a single path with `CMAKE_PREFIX_PATH`

```bash
export CMAKE_PREFIX_PATH="/opt/qt/6.10.2/gcc_64:${CMAKE_PREFIX_PATH}"
# Or replace by your path
```
