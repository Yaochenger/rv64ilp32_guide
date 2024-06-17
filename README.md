# RV64ilp32 Note

## 概述

Todo

参考链接：

- [ ]  [nuttx support rv64ilp32 ABI](https://github.com/apache/nuttx/pull/12475)
- [ ] [新32位 nuttx ](https://yf13.github.io/nuttx/nuttx-rv64ilp32)
- [ ] [新32位 nuttx 笔记](https://yf13.github.io/nuttx/nuttx-rv64ilp32)



## 一 数据类型

RISC-V GCC通过-mabi选项指定数据模型和浮点参数传递规则。有效的选项值包括ilp32、ilp32f、ilp32d、lp64、lp64f 和 lp64d。前半部分指定数据模型，后半部分指定浮点参数传递规则。

> `i`指`int`，`l`指`long`，`p`指`pointer`即指针，`32/64`指前面给出的类型是`32/64`位的；`f`指`float`，指`float`型浮点数参数通过浮点数寄存器传递；`d`指`double`，指`floa`t型和`double`型浮点数参数通过浮点数寄存器传递。

数据模型：

|                     | int字长 | long字长 | 指针字长 |
| :-----------------: | :-----: | :------: | :------: |
| ilp32/ilp32f/ilp32d | 32bits  |  32bits  |  32bits  |
|  lp64/lp64f/lp64d   | 32bits  |  64bits  |  64bits  |

浮点参数传递规则：

|              | 需要浮点扩展指令？ |          float参数          |         double参数          |
| :----------: | :----------------: | :-------------------------: | :-------------------------: |
|  ilp32/lp64  |       不需要       | 通过整数寄存器（a0-a1）传递 | 通过整数寄存器（a0-a3）传递 |
| ilp32f/lp64f |     需要F扩展      | 过浮点寄存器（fa0-fa1）传递 | 通过整数寄存器（a0-a3）传递 |
| ilp32d/lp64d |  需要F扩展和D扩展  | 过浮点寄存器（fa0-fa1）传递 | 过浮点寄存器（fa0-fa1）传递 |

浮点参数传递规则只跟`-mabi`选项有关，和`-march`选项没有直接关系，但是部分`-mabi`选项需要浮点寄存器，浮点寄存器是通过浮点扩展指令引入的，这就需要在`-march`选项中指定浮点扩展。

## 二 Nuttx RV64ilp32

### 1.数据类型修改

uintptr_uintreg_t

1. **定义来源**：
   - `uintptr_t` 是 C 标准库的一部分，定义在 `<stdint.h>` 中。
   - `uintreg_t` 通常是特定平台或编译器扩展定义的类型，不是标准的一部分。
2. **用途**：
   - `uintptr_t` 主要用于存储指针值的无符号整数类型。
   - `uintreg_t` 主要用于表示与机器寄存器大小相同的无符号整数类型。
3. **大小**：
   - `uintptr_t` 的大小保证足够存储任何指针的值，通常与平台的指针大小一致（例如 32 位或 64 位）。
   - `uintreg_t` 的大小通常与目标平台的寄存器宽度一致。例如，在 32 位平台上，`uintreg_t` 可能是 32 位无符号整数；在 64 位平台上，`uintreg_t` 可能是 64 位无符号整数。。`uintreg_t` 主要用于与寄存器相关的操作，例如在嵌入式系统或操作系统内核中，与硬件寄存器直接交互时使用。

### 2.Nuttx RV64ilp32 & qemu

TODO

