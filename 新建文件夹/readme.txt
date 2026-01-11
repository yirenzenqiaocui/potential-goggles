COOL语言代码生成器是一个简化的编译器项目，将COOL（Classroom Object Oriented Language）语言的子集编译为MIPS汇编代码。本项目实现了从抽象语法树（AST）到MIPS汇编的完整代码生成流程，适合学习编译原理和代码生成技术。

📂 项目结构
text
cool-compiler/
├── src/                    # 源代码目录
│   ├── cgen.h             # 头文件：类定义和接口声明
│   ├── cgen.cc            # 主实现：代码生成逻辑
│   ├── cool-tree.h        # 抽象语法树节点定义
│   └── cool-tree.handcode.h # 辅助宏定义
├── examples/              # 示例文件
│   └── test.cl           # COOL测试程序
├── docs/                  # 文档
│   └── README.md         # 本文档
├── Makefile              # 编译配置
├── build_and_run.sh      # 一键构建运行脚本
└── README.md            # 项目说明
🔧 功能特性
✅ 已实现功能
基础表达式：整数常量、算术运算（加、减）

变量管理：变量赋值、访问

块表达式：多个表达式的顺序执行

输出功能：out_int()函数支持

MIPS代码生成：完整的汇编代码生成

栈帧管理：基本的函数调用栈管理

🔄 待实现功能
乘法、除法运算

条件表达式（if-else）

循环结构（while）

函数调用

面向对象特性

🛠️ 环境要求
必需环境
操作系统：Linux/macOS/Windows（WSL）

编译器：g++ (支持C++11)

MIPS模拟器：SPIM 或 QTSPIM

可选环境
COOL工具链：lexer, parser, semant（用于完整编译流程）

📦 安装与运行
快速开始（推荐）
bash
# 1. 克隆或下载项目
git clone <repository-url>
cd cool-compiler

# 2. 给运行脚本执行权限
chmod +x build_and_run.sh

# 3. 一键运行
./build_and_run.sh
手动构建
bash
# 1. 编译代码生成器
make

# 2. 运行测试
./mycgen output.s examples/test.cl

# 3. 使用SPIM运行生成的汇编
spim -f output.s
📝 使用示例
1. 基本算术运算
COOL源文件 (examples/test.cl):

cool
class Main {
    main() : Object {
        {
            out_int(3 + 5);
            out_int(10 - 2);
        }
    };
};
生成MIPS汇编:

bash
./mycgen arithmetic.s examples/test.cl
运行结果:

text
COOL代码生成器演示
8
8
程序结束
2. 自定义COOL程序
创建自己的COOL程序 (myprogram.cl):

cool
class Main {
    main() : Object {
        {
            out_int(100 - 50);
            out_int(20 + 30);
        }
    };
};
编译运行:

bash
./mycgen myoutput.s myprogram.cl
spim -f myoutput.s
🔍 技术实现
代码生成架构
text
COOL源程序
    ↓
抽象语法树 (AST)
    ↓
代码生成器 (cgen)
    ↓
MIPS汇编代码 (.s文件)
    ↓
SPIM模拟器
    ↓
执行结果
核心类说明
1. Expression 类层次
cpp
Expression (抽象基类)
├── IntConst      // 整数常量
├── Plus          // 加法运算
├── Minus         // 减法运算
├── Object        // 变量访问
├── Assign        // 变量赋值
├── OutInt        // 输出整数
└── Block         // 代码块
2. CodeGenerator 类
cpp
class CodeGenerator {
    // MIPS指令生成
    void emitAdd(), emitSub(), emitLi(), ...
    
    // 变量管理
    int getVarOffset(), addVariable()
    
    // 代码生成主流程
    void generateCode()
};
MIPS寄存器约定
寄存器	用途	说明
$a0	返回值/参数	表达式计算结果
$t0-$t9	临时寄存器	中间计算
$sp	栈指针	函数调用栈
$fp	帧指针	栈帧基址
$ra	返回地址	函数返回