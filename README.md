# COOL 代码生成器 - 简化版 MIPS 代码生成实现

这是一个简化版的 COOL 编程语言到 MIPS 汇编代码的代码生成器，是编译原理课程实践项目的一部分。

## 📋 项目概述

该项目实现了一个基本的 COOL 语言子集的代码生成器，能够将简单的 COOL 程序编译为 MIPS 汇编代码，并在 SPIM 模拟器中运行。

### 主要特性
- ✅ 支持整数常量处理
- ✅ 实现基本算术运算（加法、减法）
- ✅ 支持 `out_int` 输出函数
- ✅ 基本的变量存储和加载功能
- ✅ 块表达式的顺序执行
- ✅ MIPS 汇编代码生成

## 🚀 快速开始

### 环境要求
- Linux/macOS 系统
- g++ 编译器（支持 C++11）
- SPIM 模拟器（可选，用于运行生成的汇编代码）

### 构建项目
```bash
# 克隆项目（如果从 GitHub 下载）
git clone <repository-url>
cd potential-goggles

# 编译代码生成器
make
运行测试
bash
# 运行自动化构建和测试脚本
chmod +x build_and_run.sh
./build_and_run.sh
手动测试
bash
# 生成示例 COOL 程序的 MIPS 汇编
./mycgen demo.s test.cl

# 查看生成的汇编代码
cat demo.s

# 使用 SPIM 运行（需要安装 SPIM）
spim -f demo.s
📁 文件结构
text
.
├── README.md              # 项目说明文档
├── Makefile              # 编译配置
├── build_and_run.sh      # 自动化构建脚本
├── test.cl              # 测试用的 COOL 程序
├── cgen.h               # 代码生成器头文件
├── cgen.cc              # 代码生成器实现
├── cool-tree.h          # AST 节点定义
├── cool-tree.handcode.h # 辅助宏定义
└── demo.s              # 生成的 MIPS 汇编输出
🔧 实现细节
1. 表达式类体系
cpp
Expression (抽象基类)
├── IntConst    (整数常量)
├── Plus        (加法运算)
├── Minus       (减法运算)
├── Object      (变量引用)
├── Assign      (赋值语句)
├── OutInt      (输出整数)
└── Block       (块表达式)
2. 代码生成流程
词法分析和语法分析 → 生成 AST

语义分析 → 类型检查和符号表构建

代码生成 → 遍历 AST 生成 MIPS 汇编

汇编和链接 → 生成可执行文件（通过 SPIM）

3. MIPS 约定
结果寄存器：$a0

临时寄存器：$t0-$t9

栈指针：$sp

帧指针：$fp

返回地址：$ra

💻 支持的语法特性
基本表达式
cool
3 + 5                     -- 整数加法
10 - 2                    -- 整数减法
x                         -- 变量引用
x <- 5                    -- 赋值语句
块表达式
cool
{
    out_int(3 + 5);
    out_int(10 - 2);
}
完整示例
cool
class Main {
    main() : Object {
        {
            out_int(3 + 5);        -- 输出 8
            out_int(10 - 2);       -- 输出 8
            out_int((3 + 5) * 2);  -- 输出 16
        }
    };
};
📊 测试用例
测试程序	预期输出	状态
out_int(3 + 5)	8	✅
out_int(10 - 2)	8	✅
out_int((3 + 5) * 2)	16	⚠️ (乘法待实现)
变量赋值和引用	按预期工作	✅
嵌套表达式	按预期工作	✅
