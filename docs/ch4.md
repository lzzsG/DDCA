---
layout: page
title: Chapter 4 Hardware Description Languages
permalink: /ch4-hardware-description-languages/
nav_order: 4
has_children: true
description: Chapter 4 Hardware Description Languages (HDL) 硬件描述语言（HDL）是数字系统设计的关键工具。本章深入介绍HDL的基础知识与实际应用，通过高层次代码定义硬件电路功能，并利用仿真工具验证设计的正确性。本章为读者理解复杂硬件系统设计提供了全面支持。verilog systemverilog
---

# Chapter 4: Hardware Description Languages

《Digital Design and Computer Architecture: RISC-V Edition》

硬件描述语言（HDL）在数字设计与计算机架构中扮演着重要角色，特别是在描述和模拟数字电路的行为方面。本章将深入探讨硬件描述语言的基础知识，并介绍其主要应用领域。硬件描述语言是设计复杂数字系统（如处理器、存储设备等）不可或缺的工具。通过这些语言，设计者能够使用高层次的代码来定义硬件电路的功能，并通过仿真工具验证设计的正确性，确保它们能够在实际硬件中高效运行。

## Chapter 4 :: Topics

这一章的主要内容包括硬件描述语言在不同领域中的应用，涵盖组合逻辑、时延、时序逻辑等基础概念，同时介绍一些更为复杂的主题，如组合逻辑中使用的 `always` 语句、阻塞与非阻塞赋值的区别，以及有限状态机的设计等。这些主题的核心在于帮助设计者深入理解如何用 HDL 来描述数字电路的行为，并借助参数化模块和测试平台（testbenches）来优化设计。

本章探讨的主要主题如下：

- **Introduction**：介绍硬件描述语言的基本概念和重要性。
- **Combinational Logic**：探讨组合逻辑电路的设计与实现。
- **Delays**：时延在电路仿真中的影响及其建模方式。
- **Sequential Logic**：时序逻辑电路的工作原理和设计方法。
- **Combinational Logic w/ Always**：使用 `always` 语句处理组合逻辑的高级技巧。
- **Blocking & Nonblocking Assignments**：阻塞与非阻塞赋值在电路描述中的应用场景与区别。
- **Finite State Machines**：有限状态机的设计原理与实现。
- **Parameterized Modules**：参数化模块的设计方法，提升模块的灵活性与可重用性。
- **Testbenches**：测试平台在验证电路设计正确性中的应用。

# Introduction

硬件描述语言（HDL）是用于定义和描述数字电路行为的专门编程语言。它的作用不仅限于简单描述逻辑功能，还通过计算机辅助设计（CAD）工具来生成或综合优化后的门级电路。HDL 是当前几乎所有商用数字系统设计的基础，帮助设计者有效地管理复杂系统。

### 硬件描述语言的特点

- **逻辑功能定义**：HDL 主要用于描述电路的逻辑功能，而不是物理实现。这意味着设计者可以专注于电路的行为，而不必直接考虑底层的物理结构。
- **计算机辅助设计工具的支持**：通过 CAD 工具，设计者能够对硬件设计进行仿真、优化和综合，从而生成实际可以制造的硬件电路。CAD 工具的自动化功能大大提高了设计效率，降低了人为错误的可能性。

### 常见的硬件描述语言

当前主流的硬件描述语言（HDL）主要有以下几种：

1. **Verilog**：Verilog 是一种广泛使用的硬件描述语言，主要用于设计和模拟数字电路。它的语法和C语言相似，广泛应用于集成电路设计、FPGA开发等领域。Verilog 支持并行处理，能够描述电路的结构和行为。

2. **VHDL**：VHDL（VHSIC Hardware Description Language）是另一种常见的硬件描述语言。它起源于美国国防部的VHSIC项目，语法比较严谨，类似于Ada语言。VHDL 在欧洲和航空航天、军事等领域使用较多，适用于大规模和复杂系统的建模。

3. **SystemVerilog**：SystemVerilog 是 Verilog 的扩展，它结合了硬件描述和验证功能，增加了许多高级功能，如类、随机化、断言等，尤其在验证领域非常强大。它适合现代的复杂芯片设计，已经成为集成电路设计和验证的主流工具之一。

4. **Chisel**：Chisel（Constructing Hardware in a Scala Embedded Language）是一种基于Scala的硬件描述语言，主要用于生成灵活的硬件模块。它允许设计者以更高的抽象层次来描述硬件，支持参数化设计，适合复杂和可配置的硬件系统。

5. **Bluespec**：Bluespec HDL 是基于Haskell的硬件描述语言，使用规则的方式来描述电路行为。它允许在更高层次上建模硬件设计，具有强大的抽象能力。

这些语言在集成电路设计、FPGA开发和硬件验证中扮演着重要角色，各自有不同的优点和应用场景。Verilog、SystemVerilog和VHDL是当前应用最为广泛的HDL。

## HDL to Gates

硬件描述语言（HDL）设计的核心目标之一是将高层次的电路描述转换为硬件门级实现。这一过程通常通过**仿真**和**综合**两个主要步骤来完成。

### Simulation（仿真）

仿真是设计过程中的重要阶段，用于在设计制造之前验证电路的功能。通过仿真，输入信号被应用于设计的电路，输出结果会被检查以确保设计行为的正确性。通过这一过程，设计者能够在不使用实际硬件的情况下调试电路，从而避免了昂贵的硬件制造和测试。仿真的核心优势包括：

- **输入应用于电路**：仿真允许设计者通过不同的输入条件来测试电路的响应。
- **输出检查**：输出结果与预期结果进行比对，确保设计的正确性。
- **节省成本**：仿真阶段能够在硬件制造前发现和修复问题，避免后期的昂贵错误，节省数百万美元。

### Synthesis（综合）

综合是将 HDL 代码转换为硬件门级表示的过程。综合工具会将 HDL 描述的行为转换为一个网表（netlist），即一系列逻辑门及其相互连接的线。这一步骤是实现从高层次代码到实际硬件的关键，最终生成的网表可以直接用于硬件的物理实现。

## HDL: Hardware Description Language

在使用硬件描述语言时，设计者应始终从硬件的角度思考，确保代码所表达的硬件功能与实际硬件一致。关键在于：

- **硬件优先**：在编写 HDL 时，设计者应首先明确所需硬件的功能，再编写能够表现该功能的代码。HDL 代码本质上是对硬件的抽象描述，代码的编写应反映底层硬件的行为。
- **避免软件化思维**：尽管 HDL 看似与编写软件代码相似，但实际上其目的是描述硬件。因此，设计者需要避免将 HDL 当作软件来处理，必须时刻意识到自己在描述硬件而非编写计算机软件。

## SystemVerilog Modules

<img src="{{ site.baseurl }}/docs/assets/image-20241017111327469.png" alt="image-20241017111327469" style="zoom:80%;" />

在硬件设计中，模块是设计的基本单元。SystemVerilog 模块是用来定义电路的输入、输出及其内部逻辑结构。模块的设计允许设计者对电路的特定功能进行封装，并在更大的设计中重复使用。模块的类型主要有两种：

1. **Behavioral Modules（行为描述模块）**：这些模块侧重于描述模块应该做什么，即描述电路的功能行为。这类模块在设计的早期阶段用于定义系统的逻辑行为。

2. **Structural Modules（结构描述模块）**：这些模块描述了电路是如何通过其他简单模块构建的，专注于模块内部的硬件连接和组成。

图示例展示了一个简单的 SystemVerilog 模块，其中模块有三个输入（a、b、c）和一个输出（y）。

## Module Declaration（模块声明）

SystemVerilog 中模块的声明格式如下所示：

```verilog
module example(input logic a, b, c, 
               output logic y);
  // module body goes here
endmodule
```

### 关键点

- **module/endmodule**：所有模块的定义都必须以 `module` 开头并以 `endmodule` 结束。这是 HDL 中定义模块的标准结构。
- **example**：这是模块的名称，设计者可以根据需要为每个模块定义独特的名称。
  
此处的示例模块具有三个输入信号（a、b、c）和一个输出信号（y）。

## Behavioral SystemVerilog

SystemVerilog 中的行为描述是通过高级语言结构来表达硬件的功能。这种方式不关心具体的硬件实现，而是侧重于描述电路应该如何反应，类似于软件编程中的算法描述。在下面的示例中，定义了一个简单的行为模块：

```verilog
module example(input logic a, b, c, 
               output logic y);
  assign y = ~a & ~b & ~c | a & ~b & ~c | a & ~b & c;
endmodule
```

这个模块的输入为三个逻辑信号 `a`, `b`, `c`，输出为一个逻辑信号 `y`。`assign` 语句用于直接分配输出值给 `y`，根据输入信号的组合来计算输出。这段代码表示一个布尔表达式，其中使用了逻辑运算符：

- `~`: 表示 **NOT** 操作符，取反。
- `&`: 表示 **AND** 操作符，用于按位与操作。
- `|`: 表示 **OR** 操作符，用于按位或操作。

模块使用组合逻辑运算来确定输出 `y` 的值，这是一种行为描述的方式，告诉综合工具如何在仿真中表现电路的功能。

## HDL Simulation

仿真是硬件设计中不可或缺的步骤，它用于验证电路的功能是否按照预期工作。仿真工具会根据给定的输入信号模拟电路的响应，输出的波形可以帮助设计者检查电路的行为。

<img src="{{ site.baseurl }}/docs/assets/image-20241017111927743.png" alt="image-20241017111927743" style="zoom:80%;" />

在示例中的代码基础上，仿真会将不同时间点的输入信号应用于电路，并监视输出 `y` 随着输入变化的波形。图中的时间轴显示了信号随时间变化的情况，横坐标代表时间（单位为纳秒），纵坐标表示信号的状态（高或低电平）。

举例：

- 当 `a`, `b`, `c` 全部为 0 时，输出 `y` 为 1。
- 当信号 `a` 和 `c` 发生变化时，`y` 的值会随着逻辑运算结果发生变化。

通过这种仿真分析，设计者能够确认在不同条件下电路的行为是否正确。

## HDL Synthesis

综合是从行为描述到实际硬件实现的重要步骤。综合工具会将 SystemVerilog 代码转换为门级实现，即用逻辑门和连线来实现所描述的行为电路。

<img src="{{ site.baseurl }}/docs/assets/image-20241017112119122.png" alt="image-20241017112119122" style="zoom:80%;" />

在合成后的电路图中，可以看到输入信号 `a`, `b`, `c` 通过逻辑与门（AND gate）、或门（OR gate）和非门（NOT gate）进行组合，形成输出 `y`。这是一种直接映射的硬件结构，基于代码中的布尔表达式生成。

### 主要步骤

1. **表达式分解**：首先，将布尔表达式解析为简单的逻辑操作。
2. **逻辑门映射**：然后根据每个逻辑操作映射到相应的硬件组件，比如与门、或门和非门。
3. **连接生成**：最后，生成用于连接这些逻辑门的线。

通过这种方式，设计者能够看到实际硬件是如何实现代码中定义的逻辑操作的。这一步骤为后续的硬件制造和部署奠定了基础。

## SystemVerilog Syntax

SystemVerilog 语法在硬件描述中非常重要，它有一些独特的规则，需要设计者注意。这些规则确保代码的可读性和正确性，并有助于防止错误。

### 主要语法规则

1. **大小写敏感**：SystemVerilog 是大小写敏感的语言。例如，`reset` 和 `Reset` 是两个不同的信号名称。如果在代码中不小心使用了错误的大小写，可能会导致逻辑错误。
   - 例如：`reset` 与 `Reset` 是不同的信号。

2. **变量名称不能以数字开头**：在定义信号或变量时，名称不能以数字开头。例如，`2mux` 是一个无效的名称，应该使用诸如 `mux2` 这样的格式。
   - 例如：`2mux` 是无效的名称。

3. **忽略空白符**：SystemVerilog 不会关注空格或换行符，这意味着可以使用合理的格式来提高代码的可读性而不影响编译。

4. **注释**：SystemVerilog 支持两种注释形式：
   - `//` 用于单行注释。
   - `/* */` 用于多行注释。

### 代码可读性和注释

注释在代码中起着重要作用，尤其是在复杂的硬件设计中。它有助于解释设计意图，使代码更容易被其他设计者或后期维护者理解。合理使用注释有助于保持代码的可维护性。

## Structural SystemVerilog

结构化描述是硬件设计中另一种重要的描述方式，主要关注电路的物理连接方式。在 SystemVerilog 中，结构描述通过模块实例化来完成，模块之间的连接是通过信号传递的。

### 示例代码解释

1. **and3 模块**：
   - 该模块实现了一个三输入与门，输入为 `a`, `b`, `c`，输出为 `y`。
   - 通过 `assign` 语句直接将输出 `y` 设置为 `a & b & c`，即当所有输入为高时，输出为高。

   ```verilog
   module and3(input logic a, b, c, 
               output logic y);
     assign y = a & b & c;
   endmodule
   ```

2. **inv 模块**：
   - 实现了一个反相器，输入为 `a`，输出为 `y`。
   - 通过 `assign` 语句，`y` 等于 `~a`，即取输入的反。

   ```verilog
   module inv(input logic a, 
              output logic y);
     assign y = ~a;
   endmodule
   ```

3. **nand3 模块**：
   - 该模块实现了一个三输入与非门（NAND gate），并通过实例化其他模块来完成设计。
   - 这里 `nand3` 模块的内部信号 `n1` 用于连接与门和反相器，首先通过 `and3` 实例计算出 `n1 = a & b & c`，然后通过 `inv` 实例将 `n1` 反相，输出到 `y`。
   - 该模块展示了如何在结构化描述中使用已有模块来构建更复杂的逻辑电路。

   ```verilog
   module nand3(input logic a, b, c,
                output logic y);
     logic n1;                          // 定义内部信号
     and3 andgate(a, b, c, n1);         // 实例化 and3 模块
     inv inverter(n1, y);               // 实例化 inv 模块
   endmodule
   ```

通过这种结构化的描述方式，设计者能够将复杂的硬件电路分解为多个简单的子模块。每个子模块可以单独设计、调试，然后通过模块实例化来构建更复杂的系统。这种方法有助于提高设计的模块化和可维护性。

# Combinational Logic

## 组合逻辑

组合逻辑是数字电路设计中的核心概念，它决定了电路如何在没有时钟信号的情况下根据输入信号的变化产生输出。在硬件描述语言（HDL）中，组合逻辑可以通过各种逻辑运算符来实现，如与（AND）、或（OR）、非（NOT）等操作。组合逻辑电路的输出只依赖于当前的输入，而不涉及历史状态或时序。

## Bitwise Operators（按位运算符）

按位运算符用于组合逻辑中处理输入信号的每一位，常见的按位运算包括 AND、OR、XOR 等操作。这些操作允许设计者对输入的每一位独立地进行逻辑运算，从而生成输出。

### 示例代码

```verilog
module gates(input logic [3:0] a, b, 
             output logic [3:0] y1, y2, y3, y4, y5);
  /* 五种不同的二输入逻辑操作，作用于 4 位总线 */
  assign y1 = a & b;  // AND
  assign y2 = a | b;  // OR
  assign y3 = a ^ b;  // XOR
  assign y4 = ~(a & b);  // NAND
  assign y5 = ~(a | b);  // NOR
endmodule
```

这段代码定义了一个模块 `gates`，它接收两个 4 位的输入 `a` 和 `b`，并生成 5 个输出 `y1` 到 `y5`，每个输出对应一种按位运算：

- `y1` 执行按位与（AND），即在每一位上执行 `a & b`；
- `y2` 执行按位或（OR）；
- `y3` 执行按位异或（XOR）；
- `y4` 和 `y5` 分别执行按位与非（NAND）和按位或非（NOR）。

### 综合（Synthesis）结果

在综合过程中，每个按位运算会转换为相应的逻辑门电路。综合工具会根据 HDL 描述，生成与运算符对应的硬件组件，如与门、或门、异或门等。

<img src="{{ site.baseurl }}/docs/assets/image-20241017113039730.png" alt="image-20241017113039730" style="zoom:80%;" />

上图展示了综合后的电路，其中 `y1` 到 `y5` 各自通过与门、或门、异或门等组合实现。对于每一对输入 `a` 和 `b` 的位线，电路将根据相应的按位运算符生成特定的输出逻辑。

## Reduction Operators（归约运算符）

归约运算符是用于将多个输入位的值合并为一个输出的运算，常见的归约运算符包括 AND、OR 和 XOR。这种运算允许设计者在较短的代码中完成对一组输入位的逻辑运算。

### 示例代码

```verilog
module and8(input logic [7:0] a, 
            output logic y);
  assign y = &a;  // 归约 AND 运算
  // 以下代码等效于 &a 的手写展开形式
  // assign y = a[7] & a[6] & a[5] & a[4] & a[3] & a[2] & a[1] & a[0];
endmodule
```

在这个例子中，模块 `and8` 接收一个 8 位输入信号 `a`，并通过归约 AND 运算符 `&a` 生成输出 `y`。该运算符会将 `a` 的所有位执行与运算，然后将结果输出到 `y`。与手动编写 `a[7] & a[6] & ...` 这样的代码相比，使用归约运算符简化了设计，并提高了代码的可读性和维护性。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017113247946.png" alt="image-20241017113247946" style="zoom:80%;" />

在综合过程中，归约运算符会被转换为一系列逻辑与门，依次连接输入信号的各个位。示意图中展示了 8 个输入通过与门依次连接，最终生成一个输出 `y`。这种电路结构反映了 HDL 代码中使用的归约运算的逻辑实现。

## Conditional Assignment（条件赋值）

在硬件描述语言中，条件赋值允许设计者根据某个条件信号的值来选择不同的输入路径，类似于软件中的条件运算符（if-else）。SystemVerilog 提供了一个称为三元运算符（`?:`）的简洁表达方式，用于在硬件电路中进行选择操作。

### 示例代码

```verilog
module mux2(input logic [3:0] d0, d1, 
            input logic s, 
            output logic [3:0] y);
  assign y = s ? d1 : d0;
endmodule
```

在此例中，模块 `mux2` 通过三元运算符选择两个 4 位输入 `d0` 和 `d1` 中的一个，根据控制信号 `s` 的值决定输出 `y`。如果 `s` 为 1，则选择 `d1`；否则选择 `d0`。这种形式在多路选择器（MUX）等电路设计中尤为常见。

### 综合（Synthesis）结果

三元运算符在综合过程中被映射为一个多路选择器（MUX）电路，如图所示。电路根据 `s` 的值选择 `d0` 或 `d1` 作为输出 `y`。这种电路实现了条件选择的功能，避免了复杂的逻辑门组合。

<img src="{{ site.baseurl }}/docs/assets/image-20241017123530413.png" alt="image-20241017123530413" style="zoom:80%;" />

三元运算符 `?:` 之所以称为三元运算符，是因为它操作了三个输入：控制信号 `s` 和两个输入 `d0` 与 `d1`。

## Internal Variables（内部变量）

在更复杂的电路中，设计者通常需要引入内部信号来表示中间计算结果。这些信号并不直接作为模块的输入或输出，但它们在电路实现过程中起到重要作用。SystemVerilog 允许设计者使用内部变量来存储中间计算结果，并将其用于后续的逻辑运算。

### 示例代码

```verilog
module fulladder(input logic a, b, cin, 
                 output logic s, cout);
  logic p, g;  // 内部信号

  assign p = a ^ b;     // 半加器部分
  assign g = a & b;     // 生成进位

  assign s = p ^ cin;   // 输出和
  assign cout = g | (p & cin);  // 输出进位
endmodule
```

在上述全加器设计中，`p` 和 `g` 是内部信号，用于存储两个输入的部分计算结果：

- `p` 表示两输入的异或操作，表示无进位的加法结果；
- `g` 表示两输入的与操作，表示生成进位的条件。

然后，这些中间结果被用于计算最终的和 `s` 和进位输出 `cout`。这种模块化的信号管理方式使得设计更为清晰、易于调试。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017123901930.png" alt="image-20241017123901930" style="zoom:80%;" />

综合后的电路结构展示了如何通过逻辑门实现全加器的功能。内部信号 `p` 和 `g` 分别由异或门和与门生成，然后参与后续的逻辑计算。最终的输出由这些中间信号的组合操作得出。

## Precedence（优先级）

在 SystemVerilog 中，运算符的优先级决定了复杂表达式中各个操作的计算顺序。正确理解运算符的优先级对于避免代码中的意外行为至关重要。

### 运算符优先级表

| 优先级   | 运算符               | 含义                   |
| -------- | -------------------- | ---------------------- |
| **最高** | `~`                  | NOT（非）              |
|          | `*`, `/`, `%`        | 乘法，除法，取模       |
|          | `+`, `-`             | 加法，减法             |
|          | `<<`, `>>`           | 移位                   |
|          | `<<<`, `>>>`         | 算术移位               |
|          | `<`, `<=`, `>`, `>=` | 比较运算符             |
|          | `==`, `!=`           | 等于，不等于           |
|          | `&`, `~&`            | AND, NAND              |
|          | `^`, `~^`            | XOR, XNOR              |
|          | `|`,`~|`            | OR, NOR                |
| **最低** | `?:`                 | 三元运算符（条件选择） |

运算符优先级从高到低排列，较高优先级的运算符会先进行计算。理解这些优先级有助于在复杂表达式中正确使用逻辑运算，避免产生不必要的错误。例如，在表达式 `a + b * c` 中，乘法 `*` 的优先级高于加法 `+`，因此先计算 `b * c`，再将结果与 `a` 相加。

## Numbers

在 SystemVerilog 中，数值可以以不同的进制和位宽表示。数字格式的通用形式为：`N'Bvalue`，其中：

- `N` 是位数（number of bits）。
- `B` 是进制（base），例如二进制（b）、八进制（o）、十进制（d）或十六进制（h）。

尽管 `N'Bvalue` 格式是可选的，但推荐使用这种格式来增强代码的可读性。默认情况下，未指定进制的数字将视为十进制。

### 示例表格

| 数字格式     | 位数   | 进制                 | 十进制值 | 存储值     |
| ------------ | ------ | -------------------- | -------- | ---------- |
| 3'b101       | 3      | 二进制 binary        | 5        | 101        |
| 'b11         | 无大小 | 二进制 binary        | 3        | 00…0011    |
| 8'b11        | 8      | 二进制 binary        | 3        | 00000011   |
| 8'b1010_1011 | 8      | 二进制 binary        | 171      | 10101011   |
| 3'd6         | 3      | 十进制 decimal       | 6        | 110        |
| 6'o42        | 6      | 八进制 octal         | 34       | 100010     |
| 8'hAB        | 8      | 十六进制 hexadecimal | 171      | 10101011   |
| 42           | 无大小 | 十进制 decimal       | 42       | 00…0101010 |

此表格说明了如何通过不同的进制来表示数值，以及它们在存储中的具体表现。

## Bit Manipulations: Example 1

SystemVerilog 支持对信号进行**位操作**，使得设计者可以灵活地组合、拆分和重新排列位信号。以下是一个位操作的示例：

```verilog
assign y = {a[2:1], {3{b[0]}}, a[0], 6'b100_010};
```

在这个例子中，`y` 是一个 12 位信号，按如下方式生成：

- `a[2:1]`：从 `a` 中提取第 2 位和第 1 位。
- `{3{b[0]}}`：将 `b[0]` 位重复 3 次。
- `a[0]`：将 `a` 的第 0 位作为输出的一部分。
- `6'b100_010`：直接赋值为二进制常量 `100_010`。

组合这些位后，最终生成的 12 位输出为：`a[2] a[1] b[0] b[0] b[0] a[0] 1 0 0 0 1 0`。下划线用于格式化以增强可读性，但它们不会影响仿真结果，SystemVerilog 会忽略下划线。

## Bit Manipulations: Example 2

通过组合和分解位，可以实现复杂的多路选择逻辑。下面的示例演示了如何使用 8 位输入信号的不同部分来生成不同的输出。

### 示例代码

```verilog
module mux2_8(input logic [7:0] d0, d1,
              input logic s,
              output logic [7:0] y);

  mux2 lsbmux(d0[3:0], d1[3:0], s, y[3:0]);  // 低4位
  mux2 msbmux(d0[7:4], d1[7:4], s, y[7:4]);  // 高4位
endmodule
```

在该模块中，信号 `d0` 和 `d1` 是 8 位输入信号，通过两个 4 位的多路选择器 `mux2` 分别选择其高 4 位和低 4 位，然后组合成一个 8 位的输出信号 `y`。根据选择信号 `s` 的值，输出 `y` 将取自 `d0` 或 `d1`。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017125929762.png" alt="image-20241017125929762" style="zoom:70%;" />

此代码的综合结果显示，两个 4 位的多路选择器（`lsbmux` 和 `msbmux`）分别选择输入的高 4 位和低 4 位，并将它们组合在一起，生成最终的 8 位输出 `y`。

## Z: Floating Output（高阻态输出）

在某些设计中，输出信号可能需要在特定条件下处于“高阻态”（`Z`），即断开与电路的连接。这种设计通常用于三态缓冲器或总线系统中，以防止多个信号同时驱动同一个总线。

### 示例代码

```verilog
module tristate(input logic [3:0] a,
                input logic en,
                output tri [3:0] y);

  assign y = en ? a : 4'bz;
endmodule
```

在这个三态缓冲器的设计中，当 `en`（使能信号）为 1 时，输出 `y` 将等于输入 `a`；当 `en` 为 0 时，输出 `y` 将处于高阻态（`Z`），即电路断开。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017130130759.png" alt="image-20241017130130759" style="zoom:67%;" />

综合图显示了一个典型的三态缓冲电路。使能信号 `en` 控制输出 `y` 是否连接到输入 `a`，当 `en` 禁用时，输出处于高阻态，从而避免冲突。这种设计通常用于总线共享结构中，确保当一个信号未被驱动时，其他信号可以安全驱动总线。

# Delays

在硬件描述语言（HDL）中，**延迟（delays）** 是用于仿真中模拟电路信号传播时间的工具。在实际硬件中，信号从输入传播到输出需要一定时间，延迟可以在仿真中反映这一现象。然而，需要注意的是，**延迟仅用于仿真**，并不能直接决定硬件的实际延迟或性能。

### 示例代码

```verilog
module example(input logic a, b, c,
               output logic y);
  logic ab, bb, cb, n1, n2, n3;
  
  assign #1 {ab, bb, cb} = ~(a, b, c);  // 1 纳秒延迟
  assign #2 n1 = ab & bb & cb;          // 2 纳秒延迟
  assign #2 n2 = a & bb & cb;           // 2 纳秒延迟
  assign #2 n3 = a & bb & c;            // 2 纳秒延迟
  assign #4 y = n1 | n2 | n3;           // 4 纳秒延迟
endmodule
```

在这个例子中，信号传播的时间通过 `#` 语句指定：

- `#1` 表示信号需要 1 纳秒来反映变化。
- `#2` 表示操作的结果需要 2 纳秒的时间来产生。
- `#4` 表示最终的输出 `y` 将在 4 纳秒后稳定。

这些延迟的设定有助于仿真工具精确模拟信号的传播时间，尤其是在复杂电路中多个信号交互时更为重要。

### 仿真结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017130556336.png" alt="image-20241017130556336" style="zoom:67%;" />

图中的仿真波形显示了信号在时间上的变化。随着输入 `a`, `b`, `c` 的变化，信号 `ab`, `bb`, `cb` 和中间结果 `n1`, `n2`, `n3` 会依次发生变化，并且所有变化都有一个对应的延迟。最终，输出 `y` 经过所有逻辑运算的延迟后产生。

### 延迟的作用

**延迟的主要用途是仿真中的精确建模**，它帮助设计者了解信号在电路中传播所需的时间。在实际硬件设计中，综合工具会根据所使用的具体硬件来决定信号传播的真实延迟。因此，设计者不能依赖仿真中的延迟来推断实际电路的性能。

**延迟的总结：**

- 仿真中的延迟用于模拟信号传播时间，但并不影响实际硬件设计。
- 延迟设置能够帮助设计者更好地理解电路中不同信号的相互作用和时序。

# Sequential Logic

时序逻辑是硬件设计中的一种关键概念，它与组合逻辑不同，时序逻辑的输出不仅取决于当前的输入，还依赖于之前的状态。时序逻辑广泛应用于存储元件如锁存器（latches）、触发器（flip-flops）以及有限状态机（FSMs）的设计中。在 SystemVerilog 中，使用特定的编程习惯（idioms）来正确地描述这些时序逻辑电路，确保在仿真与实际硬件之间保持一致性。

### 关键点

- SystemVerilog 使用特定的编程风格（idioms）来描述锁存器、触发器和有限状态机等时序逻辑。
- 其他编码风格可能在仿真时看似正确，但生成的硬件可能会出错。因此，严格遵循规范的时序逻辑描述方法非常重要。

## `always` Statement

时序逻辑通常通过 `always` 语句来实现，`always` 语句是 Verilog 和 SystemVerilog 中最常用的结构之一，它根据**敏感列表**（sensitivity list）中的信号变化来执行语句块。`always` 块的作用是描述需要在特定条件下触发的逻辑行为，尤其在时序逻辑中，比如基于时钟的触发器设计。

### 一般结构

```verilog
always @(sensitivity list)
    statement;
```

### 解释

- `sensitivity list` 是一个触发条件，当列表中的信号发生变化时，`always` 语句块中的逻辑将被执行。这通常包括时钟信号（`posedge clock`）或其他事件信号。
- 当敏感列表中的事件发生时，`statement` 语句将会执行，完成所描述的逻辑功能。

### 应用

- **时钟触发的时序逻辑**：常见于触发器设计，保证在时钟的上升沿或下降沿进行状态更新。
- **组合逻辑敏感列表**：当一个或多个输入信号发生变化时，立即重新计算组合逻辑的输出。

## D Flip-Flop

D 触发器（D Flip-Flop）是时序逻辑电路中最常见的存储单元之一，用于在时钟信号的作用下存储和传递数据。每当时钟信号上升沿时，D 触发器将输入的数据 `d` 传递给输出 `q`，从而将数据锁存。

### 示例代码

```verilog
module flop(input logic clk,
            input logic [3:0] d,
            output logic [3:0] q);

  always_ff @(posedge clk)
    q <= d;  // 当时钟上升沿时，q 赋值为 d
endmodule
```

在这个例子中，D 触发器在时钟信号 `clk` 的上升沿（`posedge clk`）时，将输入 `d` 的值传递给输出 `q`。这是一个最基本的触发器设计，它的行为是“q gets d”，即 `q` 直接跟随 `d` 的变化。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017131039042.png" alt="image-20241017131039042" style="zoom:84%;" />

综合后的电路图显示了时钟信号 `clk` 驱动的数据传输路径。每当时钟上升沿到来时，输入信号 `d` 的值被传递到输出 `q`，实现数据锁存的功能。

## Resettable D Flip-Flop

可复位的 D 触发器在标准的 D 触发器基础上增加了一个复位信号 `reset`。当 `reset` 信号被激活时，触发器的输出 `q` 会被清零（或设置为某个预定义值），即使时钟信号继续工作，也不会更新数据。

### 示例代码

```verilog
module flopr(input logic clk,
             input logic reset,
             input logic [3:0] d,
             output logic [3:0] q);

  always_ff @(posedge clk)
    if (reset)
      q <= 4'b0;  // 复位时，将 q 置为 0
    else
      q <= d;     // 否则，将 d 的值赋给 q
endmodule
```

在这个设计中，当时钟上升沿到来时，首先检查 `reset` 信号：

- 如果 `reset` 信号为高，则输出 `q` 被重置为 0。
- 如果 `reset` 信号为低，则正常操作，将 `d` 的值赋给 `q`。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017131107455.png" alt="image-20241017131107455" style="zoom:60%;" />

综合后的电路显示了复位功能的实现。复位信号通过额外的逻辑控制器，当复位激活时，输出被置为 0；当复位未激活时，数据流正常传输。

## Resettable D Flip-Flop with Asynchronous Reset

带有异步复位的 D 触发器允许复位信号在时钟之外的任何时候立即生效。与同步复位不同，异步复位在复位信号激活时立刻将输出重置为 0，而无需等待时钟边缘。

### 示例代码

```verilog
module flopar(input logic clk,
              input logic reset,
              input logic [3:0] d,
              output logic [3:0] q);

  always_ff @(posedge clk, posedge reset)
    if (reset)
      q <= 4'b0;  // 复位信号激活时，立即重置 q
    else
      q <= d;     // 否则，将 d 的值赋给 q
endmodule
```

在这里，敏感列表包含了 `posedge clk` 和 `posedge reset`：

- 当时钟上升沿到来时，触发器行为与普通 D 触发器相同。
- 当复位信号上升沿到来时，无论时钟状态如何，触发器立即被重置。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017131107455.png" alt="image-20241017131107455" style="zoom:60%;" />

该综合电路图展示了一个支持异步复位的 D 触发器。异步复位允许电路在任何时间点都可以响应复位信号，无需等待时钟的下一个上升沿。这对于某些需要快速复位的电路设计尤其重要。

## D Flip-Flop with Enable

带使能信号的 D 触发器（D Flip-Flop with Enable）扩展了基本的 D 触发器功能，允许数据仅在使能信号 `en` 为高时被锁存。这样可以控制数据的传输，使得输出仅在需要时更新。

### 示例代码

```verilog
module flopren(input logic clk,
               input logic reset,
               input logic en,
               input logic [3:0] d,
               output logic [3:0] q);

  always_ff @(posedge clk, posedge reset)
    if (reset)
      q <= 4'b0;  // 复位时将输出置为 0
    else if (en)
      q <= d;     // 使能时将 d 的值赋给 q
endmodule
```

在这个例子中：

- 当 `reset` 信号有效时，输出 `q` 被重置为 0。
- 当 `reset` 信号无效并且 `en` 为高时，输入 `d` 的值被锁存到输出 `q` 中。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017131337147.png" alt="image-20241017131337147" style="zoom:80%;" />

综合后的电路图展示了时钟、复位和使能信号如何控制 D 触发器的行为。数据仅在使能信号 `en` 有效时被更新，而复位信号确保在复位过程中输出被清零。

## Latch

锁存器（Latch）是一种不同于 D 触发器的存储单元，它在输入信号有效时一直保持数据更新，而不是仅在时钟边缘更新。这种行为使得锁存器比触发器更难以控制，容易导致不稳定的设计，因此在设计中通常尽量避免使用锁存器。

### 示例代码

```verilog
module latch(input logic clk,
             input logic [3:0] d,
             output logic [3:0] q);

  always_latch
    if (clk) 
      q <= d;  // 在 clk 为高时，q 追踪 d
endmodule
```

在这个例子中，锁存器的输出 `q` 会在时钟信号 `clk` 为高电平时不断追踪输入 `d` 的变化。不同于触发器，锁存器不依赖于时钟的边缘触发，而是随时更新输出。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017132817108.png" alt="image-20241017132817108" style="zoom:80%;" />

综合图显示了一个锁存器的实现，锁存器根据时钟电平的状态决定是否传递输入信号 `d`。然而，使用锁存器可能会引发意外的电路行为，因此应当避免使用，除非明确需要它的特性。

### 注意

尽量避免使用锁存器（`always_latch`）。在大多数情况下，锁存器会带来不稳定性或者难以预料的时序问题。如果在硬件综合时出现了锁存器，而你并未打算创建它，则说明可能存在错误。

## Review

在时序逻辑设计中，`always` 语句的结构对于触发器（Flip-Flop）和锁存器（Latch）的实现至关重要。

### 一般结构

```verilog
always @(sensitivity list)
  statement;
```

根据敏感列表中的信号，`always` 语句将被触发，用于描述电路的行为。

### 用法

- **Flip-Flop**：使用 `always_ff` 描述触发器，在时钟边沿触发数据更新。
- **Latch**：使用 `always_latch` 描述锁存器，数据在信号电平有效时更新，但通常应避免使用。

# Combinational Logic using `always`

在硬件描述语言中，组合逻辑可以通过 `always` 语句来实现。尽管组合逻辑的直接实现方式通常是使用 `assign` 语句，但在某些情况下，例如当涉及复杂的 `if/else` 或 `case` 语句时，使用 `always` 语句会更合适。

## `if/else` 和 `case/casez`

**`if/else`** 和 **`case/casez`** 语句必须包含在 `always` 语句中，因为它们是条件语句，描述的是在不同条件下电路的行为变化。

- **`if/else`**：用于简单的条件选择。
- **`case/casez`**：用于多重条件选择，其中 `casez` 允许使用通配符来处理不确定的位。

## Combinational Logic using `always`

组合逻辑可以通过 `always_comb` 语句来实现。在下列示例中，多个组合逻辑操作通过 `always_comb` 语句组合在一起。这里 `always_comb` 被用于保证组合逻辑操作在输入信号变化时自动更新输出信号。

### 示例代码

```verilog
module gates(input logic [3:0] a, b,
             output logic [3:0] y1, y2, y3, y4, y5);

  always_comb // 使用 always_comb 表示组合逻辑
  begin
    y1 = a & b;      // AND
    y2 = a | b;      // OR
    y3 = a ^ b;      // XOR
    y4 = ~(a & b);   // NAND
    y5 = ~(a | b);   // NOR
  end
endmodule
```

### 解释

- `always_comb` 是专门用于组合逻辑的 `always` 块。在此块中的逻辑将在输入信号 `a` 或 `b` 变化时自动更新输出。
- 使用 `begin/end` 块封装多个语句。
- 这里通过 `y1` 到 `y5` 分别实现了按位与、或、异或、与非、或非等常见的逻辑运算。

### 代码优化建议

在这个例子中，虽然可以使用 `always_comb` 实现组合逻辑，但更简洁的方法是直接使用 `assign` 语句。`assign` 语句在处理简单的组合逻辑时更加高效，代码量更少。例如，以上逻辑可以通过如下方式实现：

```verilog
assign y1 = a & b;
assign y2 = a | b;
assign y3 = a ^ b;
assign y4 = ~(a & b);
assign y5 = ~(a | b);
```

- 当需要处理简单组合逻辑时，使用 `assign` 语句更为简洁和直观。
- 当逻辑复杂、涉及多个条件判断（如 `if/else` 或 `case`）时，`always_comb` 是更合适的选择。

## Combinational Logic using `case`

在组合逻辑的设计中，`case` 语句用于根据不同的输入值选择不同的输出，这通常用于实现多路选择器、解码器或状态机等逻辑。`case` 语句可以有效地处理多种输入组合，并生成相应的输出信号。

### 示例代码

```verilog
module sevenseg(input logic [3:0] data,
                output logic [6:0] segments);

  always_comb
    case (data)
      0: segments = 7'b111_1110;  // 显示数字 0
      1: segments = 7'b011_0000;  // 显示数字 1
      2: segments = 7'b110_1101;  // 显示数字 2
      3: segments = 7'b111_1001;  // 显示数字 3
      4: segments = 7'b011_0011;  // 显示数字 4
      5: segments = 7'b101_1011;  // 显示数字 5
      6: segments = 7'b101_1111;  // 显示数字 6
      7: segments = 7'b111_0000;  // 显示数字 7
      8: segments = 7'b111_1111;  // 显示数字 8
      9: segments = 7'b111_0011;  // 显示数字 9
      default: segments = 7'b000_0000;  // 默认值
    endcase
endmodule
```

### 解释

- 在此例中，`data` 是输入的 4 位二进制信号，用于选择七段显示器的显示模式。
- `case` 语句根据输入 `data` 的值，驱动输出 `segments` 控制不同的显示数字（0-9）。
- **`default`** 分支是必需的，确保在未覆盖的输入情况下有合理的输出。若未指定 `default`，未定义输入可能会导致不可预期的行为。

## Combinational Logic using `casez`

`casez` 是 `case` 语句的扩展版本，允许使用 `?` 作为通配符，表示对输入的某些位不关心。这种用法在优先级编码器或其他需要简化输入匹配的场景中非常有用。

### 示例代码

```verilog
module priority_casez(input logic [3:0] a,
                      output logic [3:0] y);

  always_comb
    casez(a)
      4'b1???: y = 4'b1000;  // 当最高位为 1 时
      4'b01??: y = 4'b0100;  // 当次高位为 1 时
      4'b001?: y = 4'b0010;  // 当次次高位为 1 时
      4'b0001: y = 4'b0001;  // 当最低位为 1 时
      default: y = 4'b0000;  // 默认输出
    endcase
endmodule
```

### 解释

- `casez` 使用 `?` 表示“don't care”位，表示在匹配时该位可以为 0 或 1。
- 在这个优先级编码器的例子中，`y` 的值根据 `a` 的最高有效位（从左至右）来决定。例如，如果 `a` 的最高位是 1，则输出 `y = 4'b1000`，即优先级最高。

### 综合（Synthesis）结果

<img src="{{ site.baseurl }}/docs/assets/image-20241017133512325.png" alt="image-20241017133512325" style="zoom:70%;" />

图中的综合电路显示了一个实现优先级的逻辑。输入 `a` 被逐位比较，直到找到一个符合条件的最高位并输出相应的 `y` 值。

### 关键点

- **`case`** 语句实现组合逻辑，当所有可能的输入都被涵盖时，产生明确的组合逻辑输出。
- **`casez`** 语句允许通过 `?` 忽略某些位，有助于处理不关心的输入位，常用于优先级逻辑设计。
- **`default`** 分支是组合逻辑中不可或缺的一部分，确保对所有输入都定义了输出，避免合成出锁存器。

# Blocking and Nonblocking Assignments

在 SystemVerilog 中，赋值语句分为**阻塞赋值（blocking assignment）**和**非阻塞赋值（nonblocking assignment）**两种。了解这两者的区别对于确保时序逻辑的正确性至关重要，尤其是在描述时序逻辑电路时。

### Blocking vs. Nonblocking Assignment

1. **非阻塞赋值 (`<=`)**：
   - 非阻塞赋值同时执行，不依赖赋值顺序。
   - 通常用于**时序逻辑**，例如触发器（flip-flop）中，用于在时钟边沿进行数据的并行更新。

2. **阻塞赋值 (`=`)**：
   - 阻塞赋值按顺序执行，赋值在代码执行的顺序上具有严格的先后关系。
   - 通常用于**组合逻辑**，例如在 `always_comb` 块中，用于在逻辑信号变化时更新值。

### 示例

#### Good Example: Nonblocking Assignments

```verilog
module syncgood(input logic clk,
                input logic d,
                output logic q);

  logic n1;
  always_ff @(posedge clk)
  begin
    n1 <= d;  // 非阻塞赋值
    q <= n1;  // 非阻塞赋值
  end
endmodule
```

- 这里 `n1` 和 `q` 同时更新，避免了信号的时序竞争问题，确保了正确的行为。
- 非阻塞赋值确保了信号的并行更新。

#### Bad Example: Blocking Assignments

```verilog
module syncbad(input logic clk,
               input logic d,
               output logic q);

  logic n1;
  always_ff @(posedge clk)
  begin
    n1 = d;  // 阻塞赋值
    q = n1;  // 阻塞赋值
  end
endmodule
```

- 在这个例子中，由于阻塞赋值是按顺序执行的，`q` 将直接获得 `d` 的值，而不是 `n1` 的延迟值，导致时序不正确。

### 综合结果

![image-20241017134111672]({{ site.baseurl }}/docs/assets/image-20241017134111672.png)

左侧的好示例中，由于使用了非阻塞赋值，生成了正确的触发器电路；右侧的坏示例则因为阻塞赋值导致了不正确的信号传递和错误的电路行为。

## Rules for Signal Assignment

在设计时序逻辑和组合逻辑时，必须遵循以下信号赋值规则：

1. **时序逻辑**（如触发器）：
   - 使用 `always_ff @(posedge clk)` 来描述时序逻辑。
   - 使用**非阻塞赋值** (`<=`)，确保所有触发器信号同步更新。

   ```verilog
   always_ff @(posedge clk)
     q <= d;  // 非阻塞赋值
   ```

2. **简单组合逻辑**：
   - 使用连续赋值语句 `assign`，例如：

   ```verilog
   assign y = a & b;  // 连续赋值，用于组合逻辑
   ```

3. **复杂组合逻辑**：
   - 使用 `always_comb` 和**阻塞赋值** (`=`)，确保组合逻辑信号按照定义顺序更新。

4. **避免多次赋值**：
   - 每个信号只能在一个 `always` 块或 `assign` 语句中进行赋值，以避免冲突。

通过遵循这些规则，设计者可以确保信号在时序逻辑和组合逻辑中的行为正确，并避免潜在的时序问题。

# Finite State Machines (FSMs)

有限状态机（FSM）是数字系统中的一种重要模型，它通过状态转移来描述系统的行为。FSM 由三大主要部分组成：

1. **Next State Logic（下一个状态逻辑）**：决定在输入信号影响下，下一个状态是什么。
2. **State Register（状态寄存器）**：用于存储当前状态，通常在时钟上升沿更新。
3. **Output Logic（输出逻辑）**：根据当前状态和输入生成输出。

FSM 有两种常见类型：

- **Moore FSM**：输出仅取决于当前状态。
- **Mealy FSM**：输出取决于当前状态和输入信号。

### FSM Example 1: Divide by 3

<img src="{{ site.baseurl }}/docs/assets/image-20241017134731564.png" alt="image-20241017134731564" style="zoom:60%;" />

此例展示了一个 FSM，它实现了除以 3 的功能。FSM 共有 3 个状态：`S0`, `S1`, `S2`，每次输入信号（时钟）到来时，FSM 会在状态之间依次转移。

- **S0** 是复位状态（用双圆圈表示）。
- 状态转移顺序为：`S0 -> S1 -> S2 -> S0`，并不断循环。

### Divide by 3 FSM in SystemVerilog

下列代码展示了如何用 SystemVerilog 描述这个有限状态机。

### 示例代码

```verilog
module divideby3FSM(input logic clk,
                    input logic reset,
                    output logic q);

  typedef enum logic [1:0] {S0, S1, S2} statetype;
  statetype state, nextstate;

  // 状态寄存器
  always_ff @(posedge clk, posedge reset)
    if (reset)
      state <= S0;  // 复位时转到 S0
    else
      state <= nextstate;  // 正常情况下转到下一个状态

  // 下一个状态逻辑
  always_comb
    case (state)
      S0: nextstate = S1;
      S1: nextstate = S2;
      S2: nextstate = S0;
      default: nextstate = S0;
    endcase

  // 输出逻辑
  assign q = (state == S0);  // 输出信号 q 在 S0 状态下为高
endmodule
```

### 代码解释

1. **状态寄存器**：
   - 使用 `always_ff` 块在时钟上升沿更新状态寄存器。如果 `reset` 信号为高，状态会被重置为 `S0`。

2. **下一个状态逻辑**：
   - `always_comb` 块用于计算下一个状态。当状态是 `S0` 时，下一个状态将变为 `S1`，当状态是 `S1` 时，变为 `S2`，以此类推。状态在 `S0`、`S1` 和 `S2` 之间循环。

3. **输出逻辑**：
   - 该 FSM 的输出 `q` 在状态为 `S0` 时为高，在其他状态时为低。这表明 FSM 每三个时钟周期输出一次高电平，模拟了除以 3 的行为。

## FSM Example 2: Sequence Detector

下面例子展示了一个**Moore型有限状态机（FSM）**的实现，用于检测一个特定的输入序列。Moore FSM 的输出仅由当前状态决定，而不依赖于输入信号。这种状态机特别适合用于序列检测，例如检测连续的 "01" 输入模式。

<img src="{{ site.baseurl }}/docs/assets/image-20241017143500633.png" alt="image-20241017143500633" style="zoom:70%;" />

### 状态机说明

这个 Moore FSM 由三个状态组成：

1. **S0** (初始状态，输出 0)：
   - 这是初始状态，表示还没有检测到输入序列中的“0”。
   - 当输入是 `1` 时，状态机保持在 \(S0\)，因为还未检测到“01”的可能性。
   - 当输入是 `0` 时，状态机转换到 \(S1\) 状态，表示检测到了第一个“0”，此时还没有匹配到完整的“01”。

2. **S1** (中间状态，输出 0)：
   - \(S1\) 表示上一个输入是“0”，正在期待一个“1”来匹配“01”。
   - 当输入是 `1` 时，状态机转换到 \(S2\) 状态，表示成功检测到了“01”子序列。
   - 如果输入是 `0`，状态机保持在 \(S1\)，因为仍在等待合适的“1”来构成“01”。状态依旧是：上一个输入是“0”，正在期待一个“1”来匹配“01”。

3. **S2** (匹配状态，输出 1)：
   - \(S2\) 表示已经检测到了“01”子序列，输出为1，表示成功检测到了目标序列。
   - 如果输入是 `1`，状态机返回到 \(S0\)，开始检测下一个可能的“01”序列。
   - 如果输入是 `0`，状态机返回到 \(S1\)。状态 \(S1\) 依旧是：上一个输入是“0”，正在期待一个“1”来匹配“01”。

![image-20241017143347276]({{ site.baseurl }}/docs/assets/image-20241017143347276.png)

## SystemVerilog 代码

### 1. 模块定义

```verilog
module seqDetectMoore(input logic clk, reset, a, output logic smile);
```

- 这是一个名为 `seqDetectMoore` 的模块，输入为 `clk` (时钟信号)、`reset` (复位信号) 和 `a` (输入序列信号)，输出为 `smile`，用于指示是否检测到了目标序列。
  
### 2. 状态定义

```verilog
typedef enum logic [1:0] {S0, S1, S2} statetype;
statetype state, nextstate;
```

- `statetype` 定义了三个状态 `S0`, `S1`, 和 `S2`，这对应了状态机的三个状态：
  - **S0**：初始状态，表示还未检测到目标序列。
  - **S1**：表示已经检测到“0”，期待“1”来完成“01”序列。
  - **S2**：表示成功检测到了“01”序列。
- `state` 和 `nextstate` 用于存储当前状态和下一个状态。

### 3. 状态寄存器逻辑

```verilog
always_ff @(posedge clk, posedge reset)
    if (reset) state <= S0;
    else state <= nextstate;
```

- 这是一个 **同步状态寄存器**，在时钟信号的上升沿 (`posedge clk`) 或复位信号的上升沿 (`posedge reset`) 时触发。
- 如果 `reset` 信号为高（`1`），状态机会重置到 `S0` 状态（初始状态）。否则，状态机会根据 `nextstate` 的值更新当前状态。

### 4. 下一状态逻辑

```verilog
always_comb
    case (state)
        S0: if (a) nextstate = S0;
            else nextstate = S1;
        S1: if (a) nextstate = S2;
            else nextstate = S1;
        S2: if (a) nextstate = S0;
            else nextstate = S1;
        default: nextstate = S0;
    endcase
```

- **状态转换逻辑**基于当前状态和输入信号 `a` 来决定状态的转移：
  - 在状态 `S0` 时，如果输入信号 `a` 为 `1`，状态机保持在 `S0`；如果 `a` 为 `0`，状态机进入 `S1`，表示检测到了“0”。
  - 在状态 `S1` 时，如果输入信号 `a` 为 `1`，状态机进入 `S2`，表示检测到了“01”；如果 `a` 为 `0`，状态机保持在 `S1`。
  - 在状态 `S2` 时，如果输入信号 `a` 为 `1`，状态机返回到 `S0`，表示重新开始检测；如果 `a` 为 `0`，状态机回到 `S1`，准备继续检测下一个“01”序列。

### 5. 输出逻辑

```verilog
assign smile = (state == S2);
```

- 这是输出逻辑，`smile` 的输出与状态 `S2` 相关。如果当前状态是 `S2`，说明检测到了目标序列“01”，此时输出 `smile` 为 `1`；否则输出 `smile` 为 `0`。

## FSM Example 3: Sequence Detector (Mealy FSM)

下面例子展示了一个**Mealy 型有限状态机（FSM）**的实现，用于“01”序列检测。与 Moore FSM 不同，Mealy FSM 的输出不仅取决于当前状态，还与输入信号密切相关。这样可以在状态变化的瞬间对输入做出响应，而不必等到下一个时钟周期。

![image-20241017144433035]({{ site.baseurl }}/docs/assets/image-20241017144433035.png)

### 1. 模块定义

```verilog
module seqDetectMealy(input logic clk, reset, a, output logic smile);
```

- 这是一个名为 `seqDetectMealy` 的模块，输入为 `clk` (时钟信号)、`reset` (复位信号) 和 `a` (输入序列信号)，输出为 `smile`，用于指示是否检测到了目标序列。
  
### 2. 状态定义

```verilog
typedef enum logic {S0, S1} statetype;
statetype state, nextstate;
```

- 代码中定义了两个状态 `S0` 和 `S1`，这两个状态与状态机图中相对应：
  - **S0**：初始状态，表示还未检测到感兴趣的序列。
  - **S1**：表示已经检测到“0”，等待“1”来完成“01”序列的匹配。
  
### 3. 状态寄存器逻辑

```verilog
always_ff @(posedge clk, posedge reset)
    if (reset) state <= S0;
    else state <= nextstate;
```

- **状态寄存器**逻辑在时钟的上升沿或复位信号的上升沿触发。复位时，状态机会重置到 `S0`，否则状态机会根据 `nextstate` 的值更新当前状态。

### 4. 下一状态和输出逻辑

```verilog
always_comb begin
    smile = 1'b0;
    case (state)
        S0: if (a) nextstate = S0;
            else nextstate = S1;
        S1: if (a) begin
                nextstate = S0;
                smile = 1'b1;
            end
            else nextstate = S1;
        default: nextstate = S0;
    endcase
end
```

- **状态转换和输出逻辑**在一个组合逻辑块（`always_comb`）中实现。在此 Mealy 状态机中，输出 `smile` 会根据状态和输入 `a` 的值来确定：
  - 在状态 `S0`：
    - 如果输入 `a` 为 `1`，状态保持在 `S0`，输出 `smile` 为 `0`。
    - 如果输入 `a` 为 `0`，状态转移到 `S1`，输出 `smile` 依然为 `0`。
  - 在状态 `S1`：
    - 如果输入 `a` 为 `1`，状态转移到 `S0`，并且输出 `smile = 1`，表示成功检测到了“01”序列。
    - 如果输入 `a` 为 `0`，状态保持在 `S1`，继续等待 `1` 来完成“01”匹配。
- 这个设计的关键点是：当状态为 `S1` 且输入为 `1` 时，输出 `smile` 为 `1`，其余情况下输出为 `0`。

### 5. 状态机图解释

- **Mealy 状态机状态转换图**（图右上角）展示了两个状态和状态间的转换逻辑：
  - 在状态 `S0` 时，输入 `1` 保持在 `S0`（输出为 `0`），输入 `0` 转移到 `S1`（输出为 `0`）。
  - 在状态 `S1` 时，输入 `1` 转移到 `S0`（输出为 `1`），输入 `0` 保持在 `S1`（输出为 `0`）。
- 从图中可以看出，**输出依赖于输入和状态的组合**，这与 Moore 状态机不同，后者的输出只取决于状态。

### 6. 状态机框图解释

- 图右下角的 **FSM 框图** 展示了 Mealy 状态机的结构，其中：
  - **输入逻辑**：决定状态转换和输出信号。
  - **状态寄存器**：保存当前状态。
  - **输出逻辑**：基于当前状态和输入信号生成输出。

该 Mealy 状态机用于检测输入序列中的“01”模式。在状态 `S1` 时，如果检测到输入为 `1`，状态机会输出 `smile = 1`，表明成功检测到了“01”序列。与 Moore 状态机不同，Mealy 状态机的输出不仅依赖于状态，还依赖于当前的输入信号，因此能够更快地在状态转换的同一周期内给出输出。

# Parameterized Modules

**参数化模块（Parameterized Modules）**是硬件描述语言中的一种设计方法，它允许设计者创建更加通用和灵活的模块，通过参数改变模块的行为或结构。参数化的优势在于可以根据需求调整模块的尺寸、宽度等，而无需每次为不同需求编写新的模块代码。

### 示例：2:1 多路复用器 (2:1 Mux)

下面的代码展示了一个 2:1 多路复用器的参数化模块。多路复用器根据选择信号 `s` 的值，从输入信号 `d0` 和 `d1` 中选择一个信号作为输出。

### 示例代码

```verilog
module mux2
  #(parameter width = 8)   // 参数声明：width 默认值为 8
  (input logic [width-1:0] d0, d1,  // 宽度为 width 的输入
   input logic s,                  // 选择信号
   output logic [width-1:0] y);    // 宽度为 width 的输出

  assign y = s ? d1 : d0;          // 根据选择信号 s 决定输出 y
endmodule
```

### 参数化解释

- **`parameter width = 8`**：这里定义了一个名为 `width` 的参数，默认值为 8。这意味着输入、输出总线的默认宽度为 8 位。
- **`[width-1:0]`**：模块中的数据宽度依赖于参数 `width`，可以根据需要进行调整。如果没有显式指定，默认使用 8 位宽度。

### 实例化 (Instantiation)

在使用参数化模块时，可以通过显式或隐式地为参数赋值，改变模块的具体实现。例如，可以通过以下方式实例化 2:1 多路复用器：

1. **使用默认参数（8 位总线宽度）**：

   ```verilog
   mux2 myMux(d0, d1, s, out);
   ```

   在这种情况下，`myMux` 实例使用默认的 8 位总线宽度，不需要显式指定参数。

2. **显式设置参数（12 位总线宽度）**：

   ```verilog
   mux2 #(12) lowmux(d0, d1, s, out);
   ```

   在这种实例中，`lowmux` 使用了 12 位宽度的总线。通过 `#(12)`，覆盖了默认的 8 位宽度，将 `width` 参数设置为 12。

### 设计优势

- **灵活性**：通过参数化设计，一个模块可以适应不同的需求，如总线宽度、数据位数等，极大提高了代码的可复用性。
- **可扩展性**：设计者可以根据需求创建具有不同尺寸和功能的模块实例，而无需重复编写相似的代码。

参数化模块是硬件设计中一个强大的工具，允许设计者在不修改底层代码的情况下创建更灵活和可扩展的系统。通过调整参数，例如总线宽度，设计者可以在不同的硬件配置中使用同一个模块，大大提高了设计效率。

# Testbenches

**Testbench** 是一种用于验证和测试硬件设计模块的 HDL 代码。它主要负责为被测试的设备（device under test，简称 **DUT**）提供输入信号，并检查输出结果。Testbench 不参与硬件合成，仅用于仿真和验证，确保设计的正确性。

### Testbench 的特点

- **测试其他模块**：Testbench 用于测试另一模块（即 DUT）。
- **不合成**：Testbench 不会被综合为实际硬件，仅用于仿真环境。
- **类型**：
  - **简单 Testbench**：手动提供输入信号，检查输出。
  - **自检 Testbench**：自动检查 DUT 输出，给出通过/失败的反馈。
  - **带测试向量的自检 Testbench**：使用测试向量自动检查不同输入组合下的 DUT 输出。

### SystemVerilog Testbench 示例

### 1. Testbench to Implement the Given Logic Function

目标是实现一个函数 `y = b'c' + a'b`，并对其进行测试。下面展示如何用 SystemVerilog 实现逻辑功能 `sillyfunction` 及其 Testbench。

### 模块实现：`sillyfunction`

```verilog
module sillyfunction(input logic a, b, c,
                     output logic y);

  // 实现逻辑功能：y = b'c' + a'b
  assign y = (~b & ~c) | (~a & b);
  
endmodule
```

### 解释

- 逻辑表达式 `y = b'c' + a'b` 通过两部分实现：
  - **`~b & ~c`** 表示 `b'c'`，即 b 和 c 都取反的结果。
  - **`~a & b`** 表示 `a'b`，即 a 取反且 b 保持原值。
  - 最终将这两部分相或，得到输出 `y`。

### 2. Testbench 示例

```verilog
module tb_sillyfunction;

  // 定义输入和输出信号
  logic a, b, c;
  logic y;

  // 实例化待测模块（DUT）
  sillyfunction dut (.a(a), .b(b), .c(c), .y(y));

  // 初始化输入信号，逐步测试不同组合
  initial begin
    // 监视输入和输出的变化
    $monitor("At time %t: a = %b, b = %b, c = %b, y = %b", $time, a, b, c, y);

    // 第一个测试向量
    a = 0; b = 0; c = 0;
    #10;  // 等待 10 个时间单位
    // 第二个测试向量
    a = 0; b = 0; c = 1;
    #10;
    // 第三个测试向量
    a = 0; b = 1; c = 0;
    #10;
    // 第四个测试向量
    a = 0; b = 1; c = 1;
    #10;
    // 第五个测试向量
    a = 1; b = 0; c = 0;
    #10;
    // 第六个测试向量
    a = 1; b = 0; c = 1;
    #10;
    // 第七个测试向量
    a = 1; b = 1; c = 0;
    #10;
    // 第八个测试向量
    a = 1; b = 1; c = 1;
    #10;

    $finish;  // 结束仿真
  end

endmodule
```

### Testbench 解释

- **实例化 DUT**：`sillyfunction` 模块被实例化为 `dut`。
- **输入信号**：`a`, `b`, `c` 是测试的输入信号，通过初始块逐步赋予不同的值。
- **监视器（`$monitor`）**：当输入或输出信号发生变化时，`$monitor` 会自动打印当前时间和信号值，方便查看仿真结果。
- **时间控制**：`#10` 表示每次输入变化后等待 10 个时间单位，确保信号稳定，方便观察输出。

Testbench 是硬件设计验证中不可或缺的工具。通过编写 Testbench，设计者可以仿真电路在不同输入条件下的行为，验证其是否符合预期输出。在这个例子中，Testbench 用于测试简单的逻辑函数，并逐步通过不同的输入组合来检查输出。

## Testbench 1: Simple Testbench

简单 Testbench 是最基础的测试形式，通过手动设置输入信号，观察和记录输出结果，确保设备符合设计要求。

### 示例代码

```verilog
module testbench1();
  logic a, b, c;
  logic y;

  // 实例化被测设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 逐个应用输入信号
  initial begin
    a = 0; b = 0; c = 0; #10;  // 应用输入信号 a = 0, b = 0, c = 0
    c = 1; #10;                // 应用 c = 1
    b = 1; c = 0; #10;         // 应用 b = 1, c = 0
    c = 1; #10;                // 应用 c = 1
    a = 1; b = 0; c = 0; #10;  // 应用 a = 1, b = 0, c = 0
    c = 1; #10;                // 应用 c = 1
    b = 1; c = 0; #10;         // 应用 b = 1, c = 0
    c = 1; #10;                // 应用 c = 1
  end

endmodule
```

### 解释

- **实例化 DUT**：将被测试的模块 `sillyfunction` 实例化。
- **输入信号**：通过 `initial` 块设置一系列输入信号值，并通过 `#10` 等待 10 个时间单位观察结果。
- **逐步输入**：每次修改输入信号 `a`、`b`、`c`，观察模块输出 `y`。

### 优点

- 易于编写，直接观察输入输出变化。
- 可用于手动验证小型设计。

### 缺点

- 手动验证输出，容易忽略错误。
- 测试自动化较差。

## Testbench 2: Self-Checking Testbench

自检 Testbench 能够自动检查输出，并输出测试结果，提示是否通过。这种方式更为自动化和高效，适合更复杂的设计。

### 示例代码

```verilog
module testbench2();
  logic a, b, c;
  logic y;

  // 实例化被测设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 应用输入并自动检查结果
  initial begin
    a = 0; b = 0; c = 0; #10;  // 应用 a = 0, b = 0, c = 0
    if (y !== 1) $display("000 failed.");  // 检查输出 y

    c = 1; #10;  // 应用 c = 1
    if (y !== 0) $display("001 failed.");

    b = 1; c = 0; #10;  // 应用 b = 1, c = 0
    if (y !== 0) $display("010 failed.");

    c = 1; #10;  // 应用 c = 1
    if (y !== 1) $display("011 failed.");

    a = 1; b = 0; c = 0; #10;  // 应用 a = 1, b = 0, c = 0
    if (y !== 1) $display("100 failed.");

    c = 1; #10;  // 应用 c = 1
    if (y !== 0) $display("101 failed.");

    b = 1; c = 0; #10;  // 应用 b = 1, c = 0
    if (y !== 0) $display("110 failed.");

    c = 1; #10;  // 应用 c = 1
    if (y !== 0) $display("111 failed.");
  end
endmodule
```

### 解释

- **自动检查输出**：在每次设置输入信号后，使用 `if` 语句自动检查输出 `y`，若输出不符合预期，则打印出错误信息，例如 `000 failed`。
- **$display**：在仿真过程中，`$display` 语句会打印出错误信息，方便调试。

### 优点

- 自动化：可以自动检测错误，减少手动检查的工作量。
- 可扩展：适合更大规模的测试集，确保模块行为符合预期。

### 缺点

- 编写比简单 Testbench 稍复杂。
- 对输出的自动检测逻辑需要明确的预期值。

### 总结

- **简单 Testbench** 适合基础功能测试，适用于设计初期的验证。
- **自检 Testbench** 更加自动化，适合大规模验证，可以快速定位错误，提高测试效率。

## Testbench with Clock Generation and Test Vectors

在较为复杂的硬件仿真测试中，Testbench 需要生成时钟信号，并通过外部文件导入大量的测试向量，这样可以有效地测试模块的行为。下面分别讲解如何生成时钟信号，以及如何将测试向量加载到数组中用于测试。

### 1. Generate Clock

时钟信号在数字电路仿真中至关重要，特别是对于同步电路。Testbench 可以使用 `always` 块来生成周期性的时钟信号，确保电路在模拟时钟的驱动下运行。

### 示例代码

```verilog
module testbench3();
  logic clk, reset;
  logic a, b, c, yexpected;
  logic y;
  logic [31:0] vectornum, errors;  // 用于记录测试向量的编号和错误数
  logic [3:0] testvectors[10000:0]; // 测试向量数组

  // 实例化被测设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 生成时钟信号
  always
    begin
      clk = 1; #5;  // 高电平保持 5 个时间单位
      clk = 0; #5;  // 低电平保持 5 个时间单位
    end
endmodule
```

### 解释

- **时钟生成**：`always` 块没有敏感信号列表，因此会持续不断地执行，生成周期性的方波时钟。时钟周期为 10 个时间单位。
- **时钟信号控制**：`clk` 在每个时钟周期中先保持高电平 5 个单位时间，然后变为低电平再保持 5 个单位时间，形成一个完整的时钟周期。

### 2. Read Test Vectors into Array

为了测试电路模块的各个输入组合，我们可以将测试向量存储在文件中，并通过 SystemVerilog 的 `$readmemb` 或 `$readmemh` 指令将它们加载到数组中。这种方法可以用于大规模测试，减少手动输入测试数据的工作量。

### 示例代码

```verilog
// 在测试开始时，加载测试向量并设置复位信号
initial
  begin
    $readmemb("example.tv", testvectors); // 从文件加载测试向量，二进制格式
    vectornum = 0;  // 初始化测试向量编号
    errors = 0;     // 初始化错误计数器
    reset = 1;      // 复位信号为高电平，启动复位
    #22;            // 等待 22 个时间单位
    reset = 0;      // 结束复位
  end
```

### 解释

- **加载测试向量**：`$readmemb` 用于从文件 `"example.tv"` 读取测试向量，并将其加载到数组 `testvectors` 中。文件内容应为二进制格式，也可以使用 `$readmemh` 从十六进制文件中读取。
- **复位操作**：`

### 复位操作

在 Testbench 中，复位信号用于将电路初始化到已知状态。通常在仿真开始时，我们会先激活复位信号一小段时间，确保所有的寄存器和状态都被清零或设置为初始值，然后再开始正常的时钟操作。

在之前的例子中：

```verilog
initial
  begin
    $readmemb("example.tv", testvectors);  // 从文件中读取测试向量，二进制格式
    vectornum = 0;                         // 初始化测试向量编号
    errors = 0;                            // 初始化错误计数器
    reset = 1;                             // 复位信号置高，激活复位
    #22;                                   // 等待 22 个时间单位
    reset = 0;                             // 取消复位，进入正常工作状态
  end
```

### 解释

- **`$readmemb`**：从文件 `"example.tv"` 中读取二进制格式的测试向量，并将其存储在 `testvectors` 数组中。这些测试向量包含不同输入组合以及期望的输出。
- **复位信号（reset）**：在仿真开始时，`reset` 被设置为 `1`，激活复位，确保所有寄存器和电路处于初始状态。保持复位 22 个时间单位后，将 `reset` 设为 `0`，系统恢复正常工作状态。

## 3. Assign Inputs and Expected Outputs

在每个时钟的上升沿，将当前的测试向量分配给 DUT 的输入信号，同时也加载期望的输出值，以便后续进行比较。

### 示例代码

```verilog
// 在时钟的上升沿应用测试向量
always @(posedge clk)
  begin
    #1;  // 延迟一个时间单位，以模拟信号稳定
    {a, b, c, yexpected} = testvectors[vectornum];  // 从当前测试向量读取 a, b, c 和期望的 y
  end
```

### 解释

- **`always @(posedge clk)`**：每当时钟上升沿时触发此块，应用当前的测试向量。
- **`#1`**：延迟 1 个时间单位，以确保信号有足够时间在电路中传播和稳定。
- **`testvectors[vectornum]`**：从数组中读取当前测试向量，将其分配给输入信号 `a`, `b`, `c`，并记录期望输出 `yexpected`。

## 4. Compare with Expected Outputs

在时钟的下降沿，我们对当前的输出值进行验证，检查 DUT 的输出是否与预期输出相符。如果不符，则记录错误。

### 示例代码

```verilog
// 在时钟的下降沿检查输出
always @(negedge clk)
  if (~reset) begin  // 跳过复位期间
    if (y !== yexpected) begin  // 比较输出 y 和期望值 yexpected
      $display("Error: inputs = %b", {a, b, c});
      $display("       outputs = %b (expected %b)", y, yexpected);
      errors = errors + 1;  // 记录错误次数
    end

    // 增加测试向量的编号，读取下一个测试向量
    vectornum = vectornum + 1;
    if (testvectors[vectornum] === 4'bx) begin  // 如果遇到无效测试向量，结束测试
      $display("%d tests completed with %d errors", vectornum, errors);
      $stop;
    end
  end
```

### 解释

- **`always @(negedge clk)`**：在每个时钟的下降沿检查输出信号的正确性。
- **`if (~reset)`**：当 `reset` 为 `0` 时，表示复位已经结束，开始进行正常的输出检查。
- **`y !== yexpected`**：如果实际输出 `y` 和期望输出 `yexpected` 不一致，记录错误并打印详细的错误信息。
- **`vectornum = vectornum + 1;`**：测试完成后，递增测试向量的编号，读取下一个测试向量。
- **结束条件**：当读取到无效测试向量（即 `4'bx`）时，表示所有测试向量已经完成，输出测试结果并停止仿真。

### 5. 总结

完整的 Testbench 实现了以下功能：

1. **时钟生成**：用 `always` 块生成周期性时钟信号，驱动 DUT。
2. **测试向量加载**：从外部文件中读取测试向量，并存储在数组中。
3. **信号分配**：在每个时钟上升沿分配当前测试向量给 DUT 的输入，并记录期望输出。
4. **输出检查**：在时钟下降沿比较实际输出与期望输出，并记录错误。
5. **自动停止**：当所有测试向量完成后，输出测试结果并停止仿真。

通过这种方式，设计者可以自动化地进行大规模硬件验证，并准确找到设计中的错误。

---

Digital Design and Computer Architecture Lecture Notes  © 2021 Sarah Harris and David Harris

These notes may be used and modified for educational and/or  non-commercial purposes so long as the source is attributed.

---

在我们讨论的 **SystemVerilog** 示例中，有一些特性超出了传统 **Verilog** 的涵盖范围。SystemVerilog 是 Verilog 的扩展版，增加了许多高级特性来支持更加复杂的设计和验证需求。以下是这些示例中超出 Verilog 语言的特性：

### 1. **`logic` 数据类型**

- **SystemVerilog**：引入了新的数据类型 `logic`，用来替代 Verilog 中的 `wire` 和 `reg`，简化了信号类型的管理。在 Verilog 中，`wire` 和 `reg` 是分开的，`wire` 用于连接，而 `reg` 用于存储。在 **SystemVerilog** 中，`logic` 既可以用于连接，也可以用于存储，更加灵活。
- **Verilog**：只支持 `wire` 和 `reg` 两种信号类型。

   **示例：**

   ```verilog
   logic a, b, c, y;
   ```

   在 Verilog 中：

   ```verilog
   wire a, b, c;   // 用于连接
   reg y;          // 用于存储值
   ```

### 2. **`always_ff` 和 `always_comb` 语句块**

- **SystemVerilog**：引入了 `always_ff` 和 `always_comb`，分别用于同步逻辑和组合逻辑。它们明确了代码块的功能，帮助避免组合逻辑和时序逻辑的混淆。
  - `always_ff`：表示这是一个时钟驱动的同步逻辑块（触发器、寄存器）。
  - `always_comb`：表示这是一个纯粹的组合逻辑块。
- **Verilog**：只支持 `always` 语句，必须通过手动定义敏感列表来区分时序逻辑和组合逻辑。

   **示例：**

   ```verilog
   always_ff @(posedge clk)
     q <= d;

   always_comb
     y = a & b;
   ```

   在 Verilog 中：

   ```verilog
   always @(posedge clk)
     q <= d;

   always @(*)
     y = a & b;
   ```

### 3. **`$readmemb` 和 `$readmemh` 语法的增强**

- **SystemVerilog**：虽然 `$readmemb` 和 `$readmemh` 都在 Verilog 中支持，但 **SystemVerilog** 对这些文件读写的功能进行了增强，支持更多文件格式，并增加了对多维数组和更大数据集的支持。
- **Verilog**：Verilog 也支持 `$readmemb` 和 `$readmemh`，但在操作复杂数组时支持较少。

   **示例：**

   ```verilog
   $readmemb("example.tv", testvectors);
   ```

### 4. **`typedef` 自定义类型**

- **SystemVerilog**：支持 `typedef` 用于定义新的数据类型，增加了代码的可读性和灵活性。`typedef` 可以为枚举、结构体等定义类型名称。
- **Verilog**：不支持 `typedef`，只能手动定义每个数据类型。

   **示例：**

   ```verilog
   typedef enum logic [1:0] {S0, S1, S2} statetype;
   statetype state, nextstate;
   ```

   在 Verilog 中，你需要手动定义信号的位宽和名称：

   ```verilog
   reg [1:0] state, nextstate;
   ```

### 5. **增强的 `==` 和 `!==` 运算符**

- **SystemVerilog**：运算符 `===` 和 `!==` 能够处理 `x` 和 `z` 的情况，用于精确比较信号的未知状态（`x`）或高阻态（`z`）。
- **Verilog**：Verilog 只支持 `==` 和 `!=`，不能处理 `x` 或 `z` 的情况。

   **示例：**

   ```verilog
   if (y === 1'bx)  // 在 SystemVerilog 中可以直接比较 'x'
   ```

### 6. **更强的数组支持**

- **SystemVerilog**：大大扩展了数组的功能，支持多维数组、动态数组、关联数组等。同时，数组访问变得更加灵活。
- **Verilog**：Verilog 支持有限的静态数组，且不支持动态数组或多维数组的灵活处理。

   **示例：**

   ```verilog
   logic [3:0] testvectors[10000:0];  // SystemVerilog 支持多维数组
   ```

### 7. **高级验证特性**

- **SystemVerilog**：为验证目的添加了许多高级功能，例如类、随机化、断言（assertion）、接口等。这些功能主要用于复杂验证环境，超出了 Verilog 的范围。
- **Verilog**：Verilog 不支持类、随机化或高级断言功能。
