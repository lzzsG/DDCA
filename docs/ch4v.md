---
layout: page
title: Chapter 4v Verilog Edition
permalink: /ch4-hardware-description-languages/Verilog
nav_order: 1
parent: Chapter 4 Hardware Description Languages
description: Chapter 4 v Hardware Description Languages (HDL) 硬件描述语言（HDL）是数字系统设计的关键工具。本章深入介绍HDL的基础知识与实际应用，通过高层次代码定义硬件电路功能，并利用仿真工具验证设计的正确性。本章为读者理解复杂硬件系统设计提供了全面支持。verilog版
---


# Verilog 与 SystemVerilog 的主要区别

Verilog 是一种用于数字设计和仿真的硬件描述语言（HDL），而 SystemVerilog 是在 Verilog 基础上的扩展，旨在增强设计和验证的功能。SystemVerilog 引入了大量新的特性，使得硬件设计和验证更强大和高效。在讲解这些语言时，主要需要注意其语法和使用场景的区别。

### Verilog 基础概念及其使用

#### 1. `input` 和 `output`

- **`input`**：定义电路模块的输入端口，通常表示从外部接收到的信号。
- **`output`**：定义模块的输出端口，表示该模块产生的信号。

**示例**：

```verilog
module example(input a, b, output y);
  assign y = a & b;  // 逻辑与操作
endmodule
```

- 在 Verilog 中，`input` 和 `output` 通常用来声明端口，它们定义信号的方向性。

#### 2. `reg` 与 `wire` 的区别

- **`wire`**：用于组合逻辑信号，表示一个连接信号。通常在不需要存储的情况下使用，它连接逻辑门、模块之间的信号。
  
- **`reg`**：用于存储逻辑信号，可以在触发事件（如时钟信号）时保持其值。需要注意的是，尽管名字叫 "reg"，它并不一定映射到硬件中的寄存器，具体取决于实际使用场景。

**使用场景**：

- **`wire`**：用于组合逻辑电路，例如门电路的连接。
- **`reg`**：用于时序逻辑电路，通常在 `always` 块中使用，用于存储时钟驱动的信号。

**示例**：

```verilog
module example(input a, b, output reg y); // 输出为寄存器类型
  always @ (posedge clk) begin
    y <= a & b;  // 在时钟上升沿时更新 y 的值
  end
endmodule
```

在这个示例中，`y` 被声明为 `reg` 类型，因为它的值在时钟上升沿更新并保持。

#### 3. `always` 块

`always` 块是 Verilog 中的核心结构之一，用于描述组合逻辑和时序逻辑。

- **组合逻辑（Combinational Logic）**：
  使用 `always @(*)` 或 `always @(信号列表)` 表示组合逻辑，任何输入信号发生变化时，都会重新计算输出。

- **时序逻辑（Sequential Logic）**：
  使用 `always @(posedge clk)` 或 `always @(negedge clk)`，表示在时钟的上升沿或下降沿触发的逻辑，通常用于寄存器更新。

**示例**：

- **组合逻辑**：

  ```verilog
  always @(*) begin
    y = a & b;  // 当输入 a 或 b 变化时，重新计算 y
  end
  ```

- **时序逻辑**：

  ```verilog
  always @(posedge clk) begin
    y <= a & b;  // 在时钟上升沿时更新 y
  end
  ```

### Verilog 常见使用规范

1. **`assign` 语句**：
   - `assign` 用于定义组合逻辑的输出。在组合逻辑电路中，`assign` 将输入信号立即分配给输出信号。

   **示例**：

   ```verilog
   assign y = a & b;
   ```

2. **`case` 语句**：
   - `case` 语句用于多路选择。Verilog 中需要明确列出所有可能的输入组合，或者使用 `default` 来处理其他未列出的情况，以避免综合器推断锁存器。

   **示例**：

   ```verilog
   always @(*) begin
     case (sel)
       2'b00: y = a;
       2'b01: y = b;
       default: y = 1'b0;  // 避免综合锁存器
     endcase
   end
   ```

3. **寄存器驱动与输出**：
   - 如果输出是时钟驱动的信号（即需要在时钟沿更新），应该使用 `reg` 类型，并且必须在 `always` 块中更新。

   **示例**：

   ```verilog
   module dff(input clk, d, output reg q);
     always @(posedge clk)
       q <= d;
   endmodule
   ```

### SystemVerilog 的增强特性

1. **`logic` 代替 `wire` 和 `reg`**：
   在 SystemVerilog 中，`logic` 可以替代 `wire` 和 `reg`，减少了开发者对信号类型的混淆。`logic` 可以在组合逻辑和时序逻辑中使用。

   **示例**：

   ```verilog
   module example(input logic a, b, output logic y);
     always @(*) y = a & b;
   endmodule
   ```

2. **`always_ff` 和 `always_comb`**：
   SystemVerilog 提供了更为专门化的 `always_ff` 和 `always_comb` 块，用来分别表示时序逻辑和组合逻辑。这使得代码更具可读性和安全性，防止了无意中混合时序和组合逻辑。

   - **`always_ff`**：用于描述时序逻辑，需要时钟信号作为触发。
   - **`always_comb`**：用于描述组合逻辑，自动推断所有输入信号。

   **示例**：

   ```verilog
   always_ff @(posedge clk) begin
     q <= d;
   end
   ```

   ```verilog
   always_comb begin
     y = a & b;
   end
   ```

3. **强类型定义**：
   SystemVerilog 引入了强类型定义功能，如 `enum`、`typedef`，使得代码更加清晰，并且便于调试和验证。例如，状态机可以使用 `enum` 定义状态类型。

   **示例**：

   ```verilog
   typedef enum logic [1:0] {S0, S1, S2} state_t;
   state_t state;
   ```

4. **接口（Interface）**：
   SystemVerilog 引入了 `interface`，用于模块之间信号的分组和管理。通过接口，多个信号可以通过一个接口变量传递，简化了模块端口的连接。

   **示例**：

   ```verilog
   interface bus();
     logic [31:0] data;
     logic valid, ready;
   endinterface
   ```

5. **Assertions (SVA)**：
   SystemVerilog 增强了验证能力，增加了断言（assertions）功能，允许用户在代码中直接检查设计的正确性。这些断言可以用来检测时序或逻辑条件，方便调试。

   **示例**：

   ```verilog
   assert property (posedge clk |-> (q == d));
   ```

### 总结

- Verilog 是一种功能强大的硬件描述语言，但在语法和功能上相对简单。
- SystemVerilog 是 Verilog 的扩展，增加了诸多现代硬件设计和验证的特性，特别是增强了时序逻辑、组合逻辑的描述以及模块化设计的能力。
- 使用 `always_ff` 和 `always_comb` 可以提高设计的安全性和清晰度；使用 `logic` 可以简化信号类型的管理。
- 了解何时使用 `reg` 和 `wire` 以及如何在 `always` 块中分配信号，对于 Verilog 代码的正确性至关重要。

---

## 以下是本节(ch4)相关内容的Verilog版本

### Verilog 行为级别描述（Behavioral Verilog）

```verilog
module example(input a, b, c, 
               output y);

  assign y = ~a & ~b & ~c | a & ~b & ~c | a & ~b & c;
endmodule
```

这个 Verilog 模块有三个输入 `a`、`b` 和 `c`，一个输出 `y`。通过逻辑表达式计算输出 `y`。主要逻辑操作符包括按位与（`&`）、按位或（`|`）、按位取反（`~`），生成一个复杂的组合逻辑表达式：

- 当 `a` 为 0 且 `b`、`c` 均为 0，或当 `a` 为 1 且 `b` 为 0 且 `c` 为 0，或当 `a` 为 1 且 `b` 为 0 且 `c` 为 1 时，输出 `y` 为 1。

### Verilog 语法规则

- **区分大小写**：例如，`reset` 和 `Reset` 是不同的信号。
- **变量命名规则**：不能以数字开头，例如 `2mux` 是非法名称。
- **空白字符**：会被忽略。
- **注释**：支持单行注释 `//` 和多行注释 `/*...*/`。

### 结构化建模 - 层次结构

Verilog 支持模块化设计，代码展示了如何定义模块并在另一个模块中实例化这些模块：

```verilog
module and3(input a, b, c, output y);
  assign y = a & b & c;
endmodule

module inv(input a, output y);
  assign y = ~a;
endmodule

module nand3(input a, b, c, output y);
  wire n1;            // 内部信号
  and3 andgate(a, b, c, n1);  // 实例化 and3 模块
  inv inverter(n1, y);        // 实例化 inv 模块
endmodule
```

- `and3` 模块执行 3 输入与操作。
- `inv` 模块是一个反相器。
- `nand3` 模块将 `and3` 和 `inv` 组合起来形成 3 输入与非（NAND）功能。

### 位运算符

Verilog 提供了位级别的逻辑运算符，用于多位总线的逻辑操作：

```verilog
module gates(input [3:0] a, b, output [3:0] y1, y2, y3, y4, y5);
  assign y1 = a & b;  // 与操作
  assign y2 = a | b;  // 或操作
  assign y3 = a ^ b;  // 异或操作
  assign y4 = ~(a & b); // 与非操作
  assign y5 = ~(a | b); // 或非操作
endmodule
```

- 位操作符包括 **与** (`&`)、**或** (`|`)、**异或** (`^`)、**与非** (`~&`) 和 **或非** (`~|`)。

### 归约运算符

归约运算符用于将多位信号进行归约，合并为单一输出位：

```verilog
module and8(input [7:0] a, output y);
  assign y = &a;  // 所有位的按位与
endmodule
```

- `&a` 是对 `a` 信号的所有位进行按位与的简写形式，结果是一个单一位输出。

### Conditional Assignment

Verilog中的条件赋值（**三元运算符**）基于一个条件信号 `s` 来选择两个输入信号 `d0` 和 `d1` 之一作为输出 `y`。代码如下：

```verilog
module mux2(input [3:0] d0, d1,
            input s,
            output [3:0] y);
  assign y = s ? d1 : d0;
endmodule
```

- **三元运算符** `? :` 是一个条件运算符，根据 `s` 的值选择输出 `d1` 或 `d0`。

### Internal Variables

使用内部信号（`wire`）实现一个全加器（Full Adder），内部信号 `p` 和 `g` 用于中间计算：

```verilog
module fulladder(input a, b, cin, output s, cout);
  wire p, g;  // 内部节点

  assign p = a ^ b;   // 计算a和b的异或
  assign g = a & b;   // 计算a和b的与
  assign s = p ^ cin; // 计算sum
  assign cout = g | (p & cin); // 计算进位
endmodule
```

- 使用 `wire` 定义了两个内部信号 `p` 和 `g`，分别表示异或和与操作的中间结果。
- 最终计算得到输出 `s`（和）和 `cout`（进位）。

### Bit Manipulations: Example 1

对信号进行**位操作**，通过将不同信号的位组合成一个新的信号：

```verilog
assign y = {a[2:1], {3{b[0]}}, a[0], 6'b100_010};
```

- 该语句将 `a` 和 `b` 的特定位拼接，并在最后添加常量 `6'b100_010`，生成一个 12 位的输出信号 `y`。

### Bit Manipulations: Example 2

使用多位信号操作实现一个 8 位的多路选择器（**Mux**）：

```verilog
module mux2_8(input [7:0] d0, d1,
              input s,
              output [7:0] y);
  mux2 lsbmux(d0[3:0], d1[3:0], s, y[3:0]);
  mux2 msbmux(d0[7:4], d1[7:4], s, y[7:4]);
endmodule
```

- 通过将 `mux2` 分成两部分，分别处理高 4 位和低 4 位，从而实现 8 位数据的多路选择。

### Z: Floating Output

 Verilog 实现一个三态缓冲器（**tristate buffer**），在三态缓冲器中，当使能信号 `en` 为高时，输出 `y` 直接连接到输入 `a`；而当使能信号 `en` 为低时，输出 `y` 变为高阻态（`Z`），表示断开状态：

```verilog
module tristate(input [3:0] a,
                input en,
                output [3:0] y);
  assign y = en ? a : 4'bz;  // 如果 en 为高，输出 a；否则输出 Z（高阻态）
endmodule
```

- **高阻态**（`Z`）用于共享总线或需要断开连接的场合。

### Delays

 Verilog 中使用**延迟语句**，通过 `#` 延迟语法可以设置信号变化的时间延迟，模拟真实硬件中的时序行为。代码如下：

```verilog
module example(input a, b, c,
               output y);
  wire ab, bb, cb, n1, n2, n3;

  assign #1 {ab, bb, cb} = ~(a, b, c);  // 1ns 延迟
  assign #2 n1 = ab & bb & cb;          // 2ns 延迟
  assign #2 n2 = a & bb & cb;           // 2ns 延迟
  assign #2 n3 = a & bb & c;            // 2ns 延迟
  assign #4 y = n1 | n2 | n3;           // 4ns 延迟
endmodule
```

- 这里展示了如何给信号分配增加时间延迟，从而实现不同的时序模拟。

### Always Statement

Verilog 中的 `always` 语句用于描述**过程块**，其执行基于指定的敏感信号列表。当列表中的信号发生变化时，过程块被执行：

```verilog
always @(sensitivity list)
  statement;
```

- 敏感列表中的信号变化时，触发过程块的执行。`always` 语句是实现组合逻辑和时序逻辑的基础。

### D Flip-Flop

 Verilog 实现一个简单的 D 触发器（D Flip-Flop），在时钟上升沿时将输入 `d` 的值存储到输出 `q` 中：

```verilog
module flop(input clk,
            input [3:0] d,
            output reg [3:0] q);

  always @(posedge clk)
    q <= d;  // 在时钟上升沿，q 获取 d 的值
endmodule
```

- 使用 `always @(posedge clk)` 描述了在时钟上升沿触发的行为。
- `q` 被声明为 `reg` 类型，因为它在 `always` 块中被赋值。
- **注意**：`reg` 类型并不意味着一定是寄存器，它只是用于在 `always` 块中存储数据。

### Resettable D Flip-Flop

具有**异步复位**的 D 触发器。在时钟上升沿时，D 触发器捕获输入 `d` 的值并将其存储在输出 `q` 中。而当 `reset` 信号为高时，输出 `q` 会被复位为 0，无论时钟的状态如何：

```verilog
module flopr(input clk,
             input reset,
             input [3:0] d,
             output reg [3:0] q);

  // 异步复位
  always @(posedge clk, posedge reset)
    if (reset)
      q <= 4'b0;  // 当 reset 为高时，输出复位为 0
    else
      q <= d;     // 否则在时钟上升沿捕获 d 的值
endmodule
```

- 异步复位可以在任意时刻复位输出值，而不依赖时钟信号。

### D Flip-Flop with Enable

具有**异步复位和使能信号**的 D 触发器。在时钟上升沿时，如果 `en`（使能）信号为高，D 触发器捕获输入 `d` 的值并将其存储到输出 `q` 中；如果 `reset` 信号为高，输出 `q` 会被复位为 0：

```verilog
module flopen(input clk,
              input reset,
              input en,
              input [3:0] d,
              output reg [3:0] q);

  // 异步复位与使能信号
  always @(posedge clk, posedge reset)
    if (reset)
      q <= 4'b0;  // 当 reset 为高时，输出复位为 0
    else if (en)
      q <= d;     // 否则仅当 en 为高时捕获 d 的值
endmodule
```

- 这个设计允许在时钟周期中根据使能信号的状态决定是否更新输出值。

### Latch

**锁存器**，在时钟为高电平时捕获输入 `d` 的值，并将其存储到输出 `q` 中：

```verilog
module latch(input clk,
             input [3:0] d,
             output reg [3:0] q);

  always @(clk, d)
    if (clk)
      q <= d;  // 当 clk 为高时，锁存 d 的值到 q 中
endmodule
```

- **注意**：锁存器不是课程推荐使用的电路结构，因为它会产生不期望的时序问题。如果在合成硬件时包含了锁存器，通常被认为是设计错误。

### Other Behavioral Statements

在 Verilog 中，`if/else` 和 `case`（以及其扩展形式 `casez`）等语句必须放置在 `always` 语句块中使用。此外，所有在 `always` 语句中分配的变量都必须声明为 `reg` 类型，即使这些变量并未实际被注册。

### Combinational Logic using `always`

下面的代码展示了如何使用 `always` 语句实现组合逻辑电路。`always @(*)` 表示该块将对所有输入信号的任何变化做出响应：

```verilog
module gates(input [3:0] a, b,
             output reg [3:0] y1, y2, y3, y4, y5);

  always @(*) begin
    y1 = a & b;   // AND 操作
    y2 = a | b;   // OR 操作
    y3 = a ^ b;   // XOR 操作
    y4 = ~(a & b); // NAND 操作
    y5 = ~(a | b); // NOR 操作
  end
endmodule
```

- 该代码实现了多个逻辑运算操作，但在这种简单情况下，可以通过使用 `assign` 语句实现更加简洁的组合逻辑，避免使用 `always` 块。

### Combinational Logic using `case`

`case` 语句是描述组合逻辑的另一种方式，尤其适用于多路选择器、编码器等应用。以下示例将 4 位输入信号解码为 7 段显示器的输出：

```verilog
module sevenseg(input [3:0] data,
                output reg [6:0] segments);

  always @(*) begin
    case (data)
      4'd0: segments = 7'b111_1110; // 显示 0
      4'd1: segments = 7'b011_0000; // 显示 1
      4'd2: segments = 7'b110_1101; // 显示 2
      4'd3: segments = 7'b111_1001; // 显示 3
      4'd4: segments = 7'b011_0011; // 显示 4
      4'd5: segments = 7'b101_1011; // 显示 5
      4'd6: segments = 7'b101_1111; // 显示 6
      4'd7: segments = 7'b111_0000; // 显示 7
      4'd8: segments = 7'b111_1111; // 显示 8
      4'd9: segments = 7'b111_1011; // 显示 9
      default: segments = 7'b000_0000; // 默认输出
    endcase
  end
endmodule
```

- 该代码根据 `data` 输入值设置 `segments` 的输出状态，以显示不同的数字。
- `default` 语句确保所有可能的输入组合都被涵盖，这是使用 `case` 语句时的一个重要规范。

### FSM in Verilog

该模块实现了一个简单的有限状态机（FSM），用来执行三分频操作。通过状态机将输入信号按特定模式转换为输出 `q`。

### Verilog代码解析

```verilog
module divideby3FSM (input clk,
                     input reset,
                     output q);

  reg [1:0] state, nextstate;

  parameter S0 = 2'b00;  // 状态 S0
  parameter S1 = 2'b01;  // 状态 S1
  parameter S2 = 2'b10;  // 状态 S2

  // 状态寄存器
  always @(posedge clk, posedge reset)
    if (reset) 
      state <= S0;      // 当复位信号为高时，状态重置为 S0
    else 
      state <= nextstate;

  // 状态转移逻辑
  always @(*)
    case (state)
      S0: nextstate = S1;
      S1: nextstate = S2;
      S2: nextstate = S0;
      default: nextstate = S0;
    endcase

  // 输出逻辑
  assign q = (state == S0);  // 仅当状态为 S0 时，输出 q 为高电平

endmodule
```

- **状态寄存器**：基于时钟信号或复位信号，状态机会在 `S0`、`S1`、`S2` 之间转移。
- **输出逻辑**：在状态 `S0` 时输出 `q` 为高电平，表示三分频的信号输出。

### Parameterized Modules

参数化模块允许我们根据不同需求调整模块的宽度或特定行为。

### 2:1 MUX 参数化实现

```verilog
module mux2 #(parameter width = 8) // 默认宽度为 8 位
             (input [width-1:0] d0, d1,
              input s,
              output [width-1:0] y);
  
  assign y = s ? d1 : d0; // 根据选择信号 s，选择 d0 或 d1
  
endmodule
```

#### 模块实例化

- 使用默认 8 位宽度：

  ```verilog
  mux2 mux1(d0, d1, s, out);
  ```

- 使用 12 位宽度：

  ```verilog
  mux2 #(12) lowmux(d0, d1, s, out);
  ```

这个设计展示了 Verilog 模块的参数化特性，允许同一个模块在不同实例中具有不同的输入和输出位宽，从而极大地提升了模块的可复用性和灵活性。

### Simple Testbench

该模块实现了一个简单的测试平台（testbench），用于验证被测试设备（DUT）的行为。在这个例子中，测试平台通过设置输入信号 `a`, `b`, `c`，并观察输出信号 `y` 来检测被测试设备的功能是否符合预期。

### Verilog代码解析

```verilog
module testbench1();
  reg a, b, c;   // 定义输入信号
  wire y;        // 定义输出信号

  // 实例化被测试设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 初始块，逐个应用输入
  initial begin
    a = 0; b = 0; c = 0; #10;   // 应用不同的输入组合
    c = 1; #10;
    b = 1; c = 0; #10;
    c = 1; #10;
    a = 1; b = 0; c = 0; #10;
    c = 1; #10;
    b = 1; c = 0; #10;
    c = 1; #10;
  end
endmodule
```

- **输入信号**： `a`, `b`, `c` 是三位输入信号，使用 `reg` 定义。
- **输出信号**： `y` 是被测试设备的输出信号，使用 `wire` 定义。
- **初始块**：通过逐步更改 `a`, `b`, `c` 的值，并在每次更改后等待 10 个时间单位（`#10`），测试不同的输入组合。

### Self-checking Testbench

这个自检测试平台不仅应用了输入信号，还检查输出信号是否符合预期。使用 `if` 语句在每次输入后检查输出，如果输出不正确，则通过 `display` 语句输出错误信息。

### Verilog代码解析

```verilog
module testbench2();
  reg a, b, c;
  wire y;

  // 实例化被测试设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 初始块，逐个应用输入并检查结果
  initial begin
    a = 0; b = 0; c = 0; #10;
    if (y !== 1) $display("000 failed.");

    c = 1; #10;
    if (y !== 0) $display("001 failed.");

    b = 1; c = 0; #10;
    if (y !== 0) $display("010 failed.");

    c = 1; #10;
    if (y !== 1) $display("011 failed.");

    a = 1; b = 0; c = 0; #10;
    if (y !== 1) $display("100 failed.");

    c = 1; #10;
    if (y !== 0) $display("101 failed.");

    b = 1; c = 0; #10;
    if (y !== 0) $display("110 failed.");

    c = 1; #10;
    if (y !== 1) $display("111 failed.");
  end
endmodule
```

- **输出检查**：每次更改输入后，程序使用 `if` 语句检查输出是否符合预期。若不符合预期，输出相应的错误信息，例如 "000 failed."。
- **自检机制**：通过自检机制，测试平台可以自动检测到输出错误，减少手动验证的工作量。这对于复杂设计的调试特别有用。

下面使用 Verilog 实现一个**自动化测试平台（Testbench）**，该平台使用外部文件中的测试向量来验证电路设计的正确性。

### 1. 生成时钟信号

测试平台首先生成一个时钟信号，用于驱动电路的同步操作。

### Verilog代码

```verilog
module testbench3();
  reg clk, reset;           // 时钟信号和复位信号
  reg a, b, c, yexpected;   // 输入信号和期望输出
  wire y;                   // DUT的实际输出
  reg [31:0] vectornum, errors;  // 记录测试向量和错误数的变量
  reg [3:0] testvectors[10000:0];  // 用于存储测试向量的数组

  // 实例化被测试设备 (DUT)
  sillyfunction dut(a, b, c, y);

  // 生成时钟信号
  always
    begin
      clk = 1; #5;   // 时钟上升沿
      clk = 0; #5;   // 时钟下降沿
    end
endmodule
```

- **clk**：时钟信号，每隔 5 个时间单位从高变低，模拟一个标准的时钟脉冲。
- **reset**：复位信号，用于在测试开始时初始化电路。

### 2. 读取测试向量

在时钟信号生成后，测试平台需要从外部文件中读取测试向量文件 `example.tv`，并将其加载到数组 `testvectors` 中。

### Verilog代码

```verilog
initial
  begin
    $readmemb("example.tv", testvectors);  // 从文件中读取测试向量
    vectornum = 0; errors = 0;  // 初始化测试向量计数器和错误计数器
    reset = 1; #27; reset = 0;  // 初始化复位信号
  end
```

- **$readmemb**：用于从文件中按二进制格式读取测试向量并存储到 `testvectors` 数组中。
- **example.tv**：测试向量文件，包含输入信号 `a`, `b`, `c` 和期望输出 `yexpected` 的组合。

### 3. 分配输入和期望输出

在每个时钟上升沿，将当前测试向量中的输入信号 `a`, `b`, `c` 和期望的输出 `yexpected` 分配给 DUT 的输入和输出。

### Verilog代码

```verilog
always @(posedge clk)
  begin
    #1;  // 延迟1个时间单位，确保时钟稳定
    {a, b, c, yexpected} = testvectors[vectornum];  // 分配当前测试向量
  end
```

- **{a, b, c, yexpected}**：从 `testvectors` 中提取当前测试向量并分配给输入和期望输出。

### 4. 比较输出与期望输出

在每个时钟的下降沿，测试平台比较 DUT 的实际输出 `y` 和期望输出 `yexpected`，并输出错误信息（如果有）。然后，读取下一个测试向量并重复这个过程。

### Verilog代码

```verilog
always @(negedge clk)
  begin
    if (~reset) begin
      if (y !== yexpected) begin
        $display("Error: Inputs = %b", {a, b, c});
        $display(" outputs = %b (%b expected)", y, yexpected);
        errors = errors + 1;
      end

      vectornum = vectornum + 1;  // 读取下一个测试向量
      if (testvectors[vectornum] === 4'bx) begin
        $display("%d tests completed with %d errors", vectornum, errors);
        $finish;  // 测试完成，结束仿真
      end
    end
  end
```

- **y !== yexpected**：用于比较实际输出和期望输出，`!==` 运算符用于检测不同值，包括 `x`, `z` 状态。
- **$display**：打印错误信息，显示出错的输入组合和期望输出。

这个测试平台通过时钟信号驱动，被动地读取存储在文件中的测试向量，并自动将其应用于 DUT。每当输出与期望不符时，平台会记录并报告错误，最终在所有测试完成后给出测试结果总结。这种方法极大地提高了测试效率和自动化程度，适合用于大规模的电路验证。
