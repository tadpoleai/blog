---
title: 一种声明式编程语言
author: ADoyle <adoyle.h@gmail.com>
copyright: '未经授权，不得全文转载。转载前请先阅读[本站版权声明](http://adoyle.me/copyright)'
tags:
  - 声明式语言
  - DSL
categories:
  - 技术
date: 2018-03-04 18:11:11
updated: 2018-03-04 18:11:11
---



## 前言 (Intro)

我设计了一种声明式编程语言，以下暂称为 comia language。
该语言借鉴了函数式编程的思想，冯·诺依曼机器模型，以及 Minecraft 的红石原理。
它是一种可以基于任何编程语言实现的高级抽象语言。

它既不是 OOP，也不是 FP。它跟命令式编程语言有很大差别，请做好心理准备。


<!-- more -->

## 概念设计

```
┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐
│ C │ │ O │ │ M │ │ I │ │ A │
└───┘ └───┘ └───┘ └───┘ └───┘
                                     ┌───────────────────────────────────────────────┐
 C: Control Chip                     │ Code/Command/Logic is Graph.                  │
 O: Output Chip                      │ Data flows in pipe line.                      │
 M: Memory Chip                      │ Inspired by Von Neumann model and Minecraft.  │
 I: Input Chip                       │                                               │
 A: Arithmetic Chip                  │ Author: ADoyle <adoyle.h@gmail.com>           │
                                     └───────────────────────────────────────────────┘

---------------------------          -------------------------------------
        Arithmetic                                Arithmetic

                                                 ┌───┐
                                              ┌─▶│ A │──┐
  ┌───┐    ┌───┐    ┌───┐              ┌───┐──┘  └───┘  └─▶┌───┐   ┌───┐
  │ I │───▶│ A │───▶│ O │              │ I │               │ A │──▶│ O │
  └───┘    └───┘    └───┘              └───┘──┐  ┌───┐  ┌─▶└───┘   └───┘
                                              └─▶│ A │──┘
                                                 └───┘
---------------------------          -------------------------------------


----------------------------------------      ----------------------------------------
                If-Else                                      Switch-Of

                    ┌───┐                                         ┌───┐
  ┌───┐          ┌─▶│ A │──┐                    ┌───┐          ┌─▶│ A │──┐
  │ I │─┐        │  └───┘  ▼                    │ I │─┐        │  └───┘  ▼
  └───┘ └▶┌───┐──┘       ┌───┐   ┌───┐          └───┘ └▶┌───┐──┘       ┌───┐   ┌───┐
          │ C │          │ A │──▶│ O │                  │ C │─────────▶│ A │──▶│ O │
  ┌───┐ ┌▶└───┘──┐       └───┘   └───┘          ┌───┐ ┌▶└───┘──┐       └───┘   └───┘
  │ M │─┘        │  ┌───┐  ▲                    │ M │─┘        │  ┌───┐  ▲
  └───┘          └─▶│ A │──┘                    └───┘          └─▶│ A │──┘
                    └───┘                                         └───┘

----------------------------------------      ----------------------------------------


---------------------------      ---------------------------
         Iteration                     Never-end Loop

           ┌───┐                            ┌───┐
           │ I │                            │ I │
   ┌───┐   └───┘                            └───┘
   │ M │─┐   │                                │
   └───┘ │   ▼                                ▼
         └▶┌───┐   ┌───┐                    ┌───┐   ┌───┐
      ┌───▶│ C │──▶│ O │               ┌───▶│ C │──▶│ O │
      │    └───┘   └───┘               │    └───┘   └───┘
    ┌───┐    │                       ┌───┐    │
    │ A │◀───┘                       │ A │◀───┘
    └───┘                            └───┘
---------------------------      ---------------------------


---------------------------      ---------------------------
          Pulses                           Filter

  ┌───┐    ┌───┐    ┌───┐          ┌───┐    ┌───┐    ┌───┐
  │ M │───▶│ A │───▶│ O │          │ I │───▶│ A │───▶│ O │
  └───┘    └───┘    └───┘          └───┘    └───┘    └───┘

---------------------------      ---------------------------
```

## comia 是什么？

comia 这名称取自 I/O/A/M/C 的一种组合。
comia 是一种声明式编程语言。它的书写语法基于 JSON/YAML 格式，它是一种 [Embedded DSL][]。
它借鉴了函数式编程的思想，冯·诺依曼机器模型，以及 Minecraft 的红石原理。
使用者只需专注于数据的流动以及逻辑的编排，只需描述你需要什么，不需要描述它具体怎样实现。
它一种可以基于任何编程语言实现的高级抽象语言。
它没有类型系统，只有数据结构。


## 概念

comia 主要有四个概念：芯片、数据、端口、管道。

代码逻辑是由管道和芯片组合出的静态逻辑图，数据会在程序运行时在管道中动态流动。

### 芯片

芯片代表一种复杂的计算。每一种芯片只处理一种逻辑。它的处理范围包括冯·诺依曼模型中的输入输出设备、运算器、控制器、存储器所负责的逻辑。
芯片的具体实现，是根据其本地代码 (native code) 而异。但是每个芯片的功能，输入、输出端口，接收参数格式是固定不变的。
作为该语言的使用者，你不需要关心芯片是怎么实现的，你只需关心这个芯片是用来做什么的，它接收什么输入，输出什么数据。然后利用 comia 语法组装这些芯片，表达出数据流动即可。

我将芯片分为五个大类。

#### 输入芯片 (Input Chip)

输入芯片用来接收来自外部设备传入的数据。
外部数据可能包裹在某种协议下，因此输入芯片有多种实现，分别对应不同的协议。

#### 输出芯片 (Output Chip)

输出芯片用来向外部设备输出数据。
外部设备可能要求用某种协议传输数据，因此输出芯片有多种实现，分别对应不同的协议。

#### 计算芯片 (Arithmetic Chip)

计算芯片的范围主要是逻辑运算，数学运算，计算任务，节拍器，脉冲过滤器等等。相当于函数。
由包含输入、输出芯片以及其他任意芯片组合的程序，都算是计算芯片。

#### 存储芯片 (Memory Chip)

存储芯片主要定义（存储）数据。等同于变量声明和赋值。
数据可以存储于内存、硬盘，或云存储等等。

#### 控制芯片 (Control Chip)

控制芯片主要实现流程控制，比如 if-else，n 选一的条件输出，迭代遍历。

### 数据 (Data)

该语言没有数据类型，只有数据结构。
在语言里传输的数据是宽泛的概念，任何可以用书面形式表达（即可以序列化）的数据结构或者单一的值，都是数据。以 JSON 的数据结构表达。可以是二进制数据，也可以是文本数据。

### 端口 (Port)

每个芯片都有两组输入端口和输出端口，和一组其他端口。
数据会从输入端口流入芯片，从输出端口上流出。

#### 数据输入端口

输入端口是一个数组，数组长度没有限制，数组长度和数组每个元素的结构含义由芯片自身功能决定。

#### 数据输出端口

输出端口是一个数组，数组长度没有限制，数组长度和数组每个元素的结构含义由芯片自身功能决定。
一般将正常输出的数据放在数组第一位。

#### 其他端口

其他端口都是输出端口，也是一个数组，用来输出其他维度的数据，比如错误信息或者系统层面的信息。当前定义了两种端口：

- 第 0 位：异常端口。当芯片处理出错，输出端口不会输出数据，异常端口会输出数据。
- 第 1 位：元数据端口。每次触发该芯片，会输出的相应的元数据信息。

其他特性后续再补充。

### 管道 (Pipe)

芯片之间通过管道建立联系，开发者使用管道连接芯片的端口。在程序运行时，芯片输出的数据就通过管道流进下一个芯片的输入端口。

## 语法设计

```yaml
<name>:
  chip: <chip-name>
  chipProps: <any-data-for-chip-properties>
  inputs:
    - $<other-name>[<port-group>][<index>]
```

comia 的结构是平铺的，只有一层。你可以用 JSON 表述，但我建议用 YAML 更便于书写。
每个元素必须有唯一的名字 (name)，你可以看做是内存地址的抽象引用，赋予了有意义的名称而已。你也可以命名无意义的名称，只要保证唯一就行。
元素的值是一些配置信息：

- chip: 指示使用哪个芯片。根据 chip 的选择不同，chipProps 需要填的内容不同。
- chipProps: 芯片的参数，芯片接收参数来配置本身的行为。具体要看芯片的实现。
- inputs: 它是个数组，代表数据管道，数据从哪里来。inputs 的配置必须符合芯片的输入端口要求。
  - 在 `inputs` 属性中通过引用名字建立数据关联。
    `$<other-name>[port-group][index]` 语法是表示引用名字为 `<other-name>` 的元素，选择 `port-group` 分类下的第 `index` 个输出。
    每个元素在运行时会产生输出，芯片实际的输出值会是这样的结构 `{<port-group>: [<value>]}`，数组中的每个值流向对应位置的输出端口。
    `<port-group>` 按定义有以下几个取值:
      - outputs: 数据输出端口
      - others: 其他端口

inputs 写法有些啰嗦，以后再优化语法。


画了一些芯片的标准图和管道示例图，便于理解。

![comia-cell-description](http://7xniyb.com1.z0.glb.clouddn.com/share/comia-cell-description.png)

![comia-example-1](http://7xniyb.com1.z0.glb.clouddn.com/share/comia-example-1.png)

![comia-example-2](http://7xniyb.com1.z0.glb.clouddn.com/share/comia-example-2.png)


## 示例

概念设计章节所列出的例子，每一张图都可以用 comia 描述。比如以下这些例子。

### 简单算数示例

```
---------------------------
        Arithmetic

  ┌───┐    ┌───┐    ┌───┐
  │ I │───▶│ A │───▶│ O │
  └───┘    └───┘    └───┘

---------------------------
```

上图的 comia 为：

```yaml
i:
  # input 是 I 类芯片
  chip: input

a:
  # 这里以计数 +1 为例
  # inc 是 A 类芯片
  chip: inc
  chipProps: 1
  inputs:
    - $i[outputs][0]

o:
  # output 是 O 类芯片
  chip: output
  inputs:
    - $a[outputs][0]
```

当这段代码被编译器编译成可执行程序，功能就是用户输入 n，返回 n + 1。

### If-Else 流程控制示例

```
----------------------------------------
                If-Else

                    ┌───┐
  ┌───┐          ┌─▶│ A │──┐
  │ I │─┐        │  └───┘  ▼
  └───┘ └▶┌───┐──┘       ┌───┐   ┌───┐
          │ C │          │ A │──▶│ O │
  ┌───┐ ┌▶└───┘──┐       └───┘   └───┘
  │ M │─┘        │  ┌───┐  ▲
  └───┘          └─▶│ A │──┘
                    └───┘

----------------------------------------
```

comia:

```yaml
i:
  chip: input
m:
  chip: memory
  chipProps: true

c:
  chip: if-else
  chipProps: (condition) => !!condition
  inputs:
    - $i[outputs][0]
    - $m[outputs][0]

a-then:
  # 这里以计数 +1 为例
  chip: inc
  chipProps: 1
  inputs:
    - $c[outputs][0]

a-else:
  chip: inc
  chipProps: 1
  inputs:
    - $c[outputs][1]

a:
  chip: merge
  inputs:
    - $a-then[outputs][0]
    - $a-else[outputs][0]

o:
  chip: output
  inputs:
    - $a[outputs][0]
```

### 迭代流程控制示例

```
---------------------------
         Iteration

           ┌───┐
           │ I │
   ┌───┐   └───┘
   │ M │─┐   │
   └───┘ │   ▼
         └▶┌───┐   ┌───┐
      ┌───▶│ C │──▶│ O │
      │    └───┘   └───┘
    ┌───┐    │
    │ A │◀───┘
    └───┘
---------------------------
```

comia:

```yaml
i:
  chip: input
m:
  chip: memory
  chipProps: true

c:
  chip: iterator
  chipProps: (condition) => !!condition
  inputs:
    - $i[outputs][0]
    - $m[outputs][0]
    - $a[outputs][0]

a:
  chip: inc
  chipProps: 2
  inputs:
    - $c[outputs][1]

o:
  chip: output
  inputs:
    - $a[outputs][0]
```

### 错误处理示例

```
--------------------------------------------
                Error Handle

           ┌───┐
           │ I │
   ┌───┐   └───┘   ┌───┐
   │ M │─┐   │   ┌▶│ A │─┐
   └───┘ │   ▼   │ └───┘ └─▶┌───┐    ┌───┐
         └▶┌───┐─┘          │ C │───▶│ O │
      ┌───▶│ C │───────────▶└───┘    └───┘
      │    └───┘
    ┌───┐    │
    │ A │◀───┘
    └───┘
--------------------------------------------
```

comia:

```yaml
i:
  chip: input
m:
  chip: memory
  chipProps: true

c:
  chip: iterator
  chipProps: (condition) => !!condition
  inputs:
    - $i[outputs][0]
    - $m[outputs][0]
    - $a[outputs][0]

a:
  chip: inc
  chipProps: 2
  inputs:
    - $c[outputs][1]

a2:
  chip: error-handle
  chipProps:
    return: exception
  inputs:
    - $c[others][0]

c2:
  chip: merge
  inputs:
    - $a2[outputs][0]
    - $c[outputs][0]

o:
  chip: output
  inputs:
    - $c2[outputs][0]
```


## 衍生语言

[语法设计](#语法设计)章节定义了最精简核心的语法。
语言的设计者可以加入新的语法，来扩展功能特性，或者优化面向人类友好的语法书写方式。

例如，可能会有这样的衍生版本。

```yaml
<name>:
  chip: <chip-name>
  chipProps: <any-data-for-chip-properties>
  # 只有一个端口的话，不用写成数组。如果取第 0 位的输出，还可以不写 [port-group][index]
  inputs: $<other-name>

  ## 其他增强功能的属性
  # 自定义输出值
  outputs:
  # debug 机制
  debug:
  # 写日志
  log:
```

基础语法没有定义进程、线程这种概念，但也并非不能实现，所有语法特性，要看语言处理环境是否支持，comia 语法层面是都可以实现扩展的。

## 图灵完备

暂时给不出证明，等我把 Rule110 的图灵完备证明论文看完应该就会证了。

## 意义

这是一个编程语言语法无关的编程语言。它抽象了编程语言的逻辑处理，以结构化数据的形式编写逻辑。大大降低了编程语言的学习成本。
因为有这层抽象，你可以用任何你喜欢的语言来实现“芯片”以及语法执行环境（解析器，执行器）。

它是平台无关的，它描述的是抽象逻辑，以数据流为核心，通过管道操作数据流来表达逻辑的控制流。
因此它是跨平台的语言，从前端、服务端、移动端、IoT、嵌入式平台都可以使用同一段代码。

它以结构化数据构成书写语法，并且操作的也是结构化数据，因此很轻松就能扩展语法以实现新的功能。

## 缺点

因为语言结构是平铺的，所以即使是简单的小程序，代码行数也会很长。
这可以通过 GUI 工具来弥补，通过操作 GUI，来自动构建文本。

## 完整的语言

### 执行环境

以上只是定义了一套语法，还需要一套语法编译器和解析器，才能真正作为程序运行起来。

### 标准库

可以发现，一段 comia 编写的程序还可以作为一个 chip，来为其他程序提供服务。
因此可以形成标准库，让开发者方便调用。

### 模块机制

为了使用标准库，需要有一套模块加载机制。


## 最后

暂且写到这里。关于语法设计细节还有待优化的地方，以后再补。
现在我还不能非常确定能否把它称之为编程语言，如果有冒犯学术界的地方，请轻拍。
至于这套语言的使用场景，暂且不赘述，等恰当的时候再谈吧。

## 参考 (Bibliographies)

- [wikiwand - 冯·诺伊曼结构][B1]

## 引用 (References)

[^1]: [][R1]


<!-- 以下是相关链接 -->

[R1]: <url> "备注"

[B1]: https://www.wikiwand.com/zh-hans/%E5%86%AF%C2%B7%E8%AF%BA%E4%BC%8A%E6%9B%BC%E7%BB%93%E6%9E%84
[Embedded DSL]: https://martinfowler.com/bliki/InternalDslStyle.html