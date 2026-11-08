# rang [![CI](https://github.com/agauniyal/rang/actions/workflows/ci.yml/badge.svg)](https://github.com/agauniyal/rang/actions/workflows/ci.yml) [![codecov](https://codecov.io/gh/agauniyal/rang/branch/master/graph/badge.svg)](https://codecov.io/gh/agauniyal/rang)

##### 为你的终端增添色彩。

![rang-demo](https://cloud.githubusercontent.com/assets/7630575/13501282/0bd00074-e18c-11e5-9848-5bd1f20566d9.gif)
<details>
 <summary>Windows 演示</summary>

![rang-windows-demo](https://cloud.githubusercontent.com/assets/11349690/19836886/8134975e-9ebe-11e6-9ee4-c4657784ff3b.gif)
</details>


用法示例
-------------

```c++
#include "rang.hpp"

using namespace std;
using namespace rang;

int main()
{
    cout << "普通旧文本"
         << style::bold << "Rang 样式文本！！"
         << style::reset << endl;
}
```

依赖项
------------
*rang* 仅依赖 [C++ 标准库](http://en.cppreference.com/w/cpp/header)，在 Unix 系统上依赖 `unistd.h` 系统头文件，在 Windows 系统上依赖 `windows.h` 和 `io.h` 系统头文件。换句话说，你不需要任何第三方依赖。


安装
------------

*rang* 是一个单头文件库。将 `rang.hpp` 放入 [include](include) 文件夹，直接放入项目的源码树中，或放在项目可访问的某个位置即可。

或者，如果你使用 [conan 包管理器](https://www.conan.io/)，请按照以下步骤操作：

1. 在项目的 `conanfile.txt` 文件的 *requires* 部分添加对 *rang* 的引用：

        [requires]
        rang/3.1.0@rang/stable

2. 运行 conan 的安装命令：

        conan install


## 如何使用

*Rang* 使用 iostream 对象——`cout`/`clog`/`cerr`——来为输出文本应用属性。由于 *rang* 旨在同时支持 Windows 和类 Unix 系统，它会处理操作系统相关的细节，并尝试提供统一的接口。由于不同操作系统版本之间的不兼容性，并非所有类型的属性在每个系统上都受支持，因此 rang 会尝试跳过那些可能产生乱码的属性（而不是向你的流中推入随机的 ANSI 转义码）。tty 的检测也在内部处理，因此你无需检查应用程序用户是否可能将输出重定向到文件。

> **需要支持非 ANSI 终端？请查看 [Termdb](https://github.com/agauniyal/termdb)，它几乎支持所有终端及其能力。**

除了设置文本属性之外，你还可以通过以下方法要求 rang 覆盖其默认行为——
```cpp
void rang::setControlMode(rang::control);
```
其中 `rang::control` 可取值为
- `control::Auto` - 自动检测终端是否支持颜色（**默认**）
- `control::Off` - 完全关闭颜色
- `control::Force` - 即使终端不支持颜色或输出被重定向到非终端，也强制使用颜色

```cpp
void rang::setWinTermMode(rang::winTerm);
```
其中 `rang::winTerm` 可取值为
- `winTerm::Auto` - 检查较新的 Windows 版本并选择 Ansi，否则回退到 Native（**默认**）
- `winTerm::Native` - 此方法在所有 Windows 版本上都受支持，但支持的属性较少
- `winTerm::Ansi` - 此方法在较新的 Windows 版本上受支持，并支持丰富多样的属性


支持的属性及其兼容性列于下方——

**文本样式**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::style::bold`      | 是   | 是   |
| `rang::style::dim`       | 是   | 否    |
| `rang::style::italic`    | 是   | 否    |
| `rang::style::underline` | 是   | 否    |
| `rang::style::blink`     | 否    | 否    |
| `rang::style::rblink`    | 否    | 否    |
| `rang::style::reversed`  | 是   | 是    |
| `rang::style::conceal`   | 可能 | 是    |
| `rang::style::crossed`   | 是   | 否    |

**文本颜色**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::fg::black`     | 是 | 是 |
| `rang::fg::red`       | 是 | 是 |
| `rang::fg::green`     | 是 | 是 |
| `rang::fg::yellow`    | 是 | 是 |
| `rang::fg::blue`      | 是 | 是 |
| `rang::fg::magenta`   | 是 | 是 |
| `rang::fg::cyan`      | 是 | 是 |
| `rang::fg::gray`      | 是 | 是 |

**背景颜色**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::bg::black`     | 是 | 是 |
| `rang::bg::red`       | 是 | 是 |
| `rang::bg::green`     | 是 | 是 |
| `rang::bg::yellow`    | 是 | 是 |
| `rang::bg::blue`      | 是 | 是 |
| `rang::bg::magenta`   | 是 | 是 |
| `rang::bg::cyan`      | 是 | 是 |
| `rang::bg::gray`      | 是 | 是 |

**明亮前景色**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::fgB::black`     | 是 | 是 |
| `rang::fgB::red`       | 是 | 是 |
| `rang::fgB::green`     | 是 | 是 |
| `rang::fgB::yellow`    | 是 | 是 |
| `rang::fgB::blue`      | 是 | 是 |
| `rang::fgB::magenta`   | 是 | 是 |
| `rang::fgB::cyan`      | 是 | 是 |
| `rang::fgB::gray`      | 是 | 是 |

**明亮背景色**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::bgB::black`     | 是 | 是 |
| `rang::bgB::red`       | 是 | 是 |
| `rang::bgB::green`     | 是 | 是 |
| `rang::bgB::yellow`    | 是 | 是 |
| `rang::bgB::blue`      | 是 | 是 |
| `rang::bgB::magenta`   | 是 | 是 |
| `rang::bgB::cyan`      | 是 | 是 |
| `rang::bgB::gray`      | 是 | 是 |

**重置样式/颜色**：

| 代码 | Linux/Win/其他 | 旧版 Win
| ---- | --------- | ------ |
| `rang::style::reset`  | 是   | 是 |
| `rang::fg::reset`     | 是   | 是 |
| `rang::bg::reset`     | 是   | 是 |

-----
## 我的终端未被检测到/输出乱码！

检查你的环境变量 `TERM` 的值。然后在[此处](https://github.com/agauniyal/rang/issues/new)提交一个 issue，并确保注明 `TERM` 的值以及你的终端名称。

## 重定向 `cout`/`cerr`/`clog` 的 rdbuf？

如果你尝试将 `cout`/`cerr`/`clog` 重定向到其他地方，Rang 不会进行干预，而是将决定权留给库用户。请确保你已阅读[此对话](https://github.com/agauniyal/rang/pull/77#issuecomment-360991652)，并查看[此处](https://gist.github.com/kingseva/a918ec66079a9475f19642ec31276a21)的示例代码。