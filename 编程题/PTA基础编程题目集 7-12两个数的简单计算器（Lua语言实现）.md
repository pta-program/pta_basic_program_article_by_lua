# 7-12 两个数的简单计算器（Lua语言实现）

## 前言

本题（7-12 两个数的简单计算器）的主要考点是：准确理解题意、规范处理输入输出格式，并正确处理边界与精度。下面先给出题目描述与格式要求，再通过清晰的思路与可直接运行的lua代码进行讲解。

## 题目描述

本题要求编写一个简单计算器程序，可根据输入的运算符，对2个整数进行加、减、乘、除或求余运算。题目保证输入和输出均不超过整型范围。

## 输入格式

输入在一行中依次输入操作数1、运算符、操作数2，其间以1个空格分隔。操作数的数据类型为整型，且保证除法和求余的分母非零。

## 输出格式

当运算符为+、-、*、/、%时，在一行输出相应的运算结果。若输入是非法符号（即除了加、减、乘、除和求余五种运算符以外的其他符号）则输出ERROR。

## 输入样例1

```in
-7 / 2
```

## 输入样例2

```in
3 & 6
```

## 输出样例1

```out
-3
```

## 输出样例2

```out
ERROR
```

## 解题思路

程序先从整行输入中解析出两个整数和一个运算符，再检查运算符是否合法。加、减、乘法可直接计算；除法和求余需要按照题目所采用的 C 语言整数运算规则处理，即商向零截断、余数符号与被除数一致，因此使用 `math.modf(a / b)` 得到截断商。

若运算符不在 `+`、`-`、`*`、`/`、`%` 中，则输出 `ERROR`。题目保证除数非零，代码同时保留除零保护，使非法输入也能安全结束。

## 完整代码

```lua
-- 7-12 两个数的简单计算器
-- 读取整行并用模式解析出操作数1、运算符、操作数2，处理负数与空格
local line = io.read("*l") -- 读取整行输入
if not line or line:match("^%s*$") then line = io.read("*a") end
local a_str, op, b_str = line:match("(-?%d+)%s*(%S)%s*(-?%d+)") -- 正则解析
local a = tonumber(a_str) -- 操作数1
local b = tonumber(b_str) -- 操作数2
-- 合法运算符集合
local valid = { ["+"]=true, ["-"]=true, ["*"]=true, ["/"]=true, ["%"]=true }
if not valid[op] then
    print("ERROR") -- 非法运算符
    return
end
-- 除零保护（题目虽保证分母非零，但需兼容除数为 0 时输出 ERROR）
if (op == "/" or op == "%") and b == 0 then
    print("ERROR")
    return
end
local result
if op == "+" then
    result = a + b -- 加法
elseif op == "-" then
    result = a - b -- 减法
elseif op == "*" then
    result = a * b -- 乘法
elseif op == "/" then
    -- 整数除法向零截断（C 语义），Lua 的 // 为向下取整，需用 math.modf 截断
    result = math.modf(a / b)
elseif op == "%" then
    -- 求余需与 C 保持一致（符号与被除数一致），用截断除法推导
    local q = math.modf(a / b) -- 截断商
    result = a - q * b -- 余数 = 被除数 - 商*除数
end
print(result)
```

## 代码流程说明

1. 读取整行，并用模式 `(-?%d+)%s*(%S)%s*(-?%d+)` 提取两个可能带负号的整数和一个运算符。
2. 通过集合表判断运算符是否属于五种合法运算。
3. 对 `/` 和 `%` 检查除数，遇到零时输出 `ERROR`。
4. 根据运算符选择对应的算式；除法使用向零截断的商，求余通过 `a - q * b` 计算。
5. 输出整数结果。

## 代码流程图

```mermaid
flowchart TD
  A[开始] --> B[读取整行并解析 a、op、b]
  B --> C{op 是否为合法运算符?}
  C -->|否| D[输出 ERROR]
  C -->|是| E{op 为除法或求余且 b=0?}
  E -->|是| D
  E -->|否| F[按运算符计算结果]
  F --> G[输出结果]
  D --> H[结束]
  G --> H
```

## 解题流程图

```mermaid
flowchart TD
  A[识别输入中的两个整数和运算符] --> B[建立合法运算符集合]
  B --> C[分别设计加减乘除和求余公式]
  C --> D[处理负数除法的向零截断规则]
  D --> E[按结果或 ERROR 输出]
  E --> F[用-7 / 2和3 & 6验证]
```

## 代码解析

```lua
-- 7-12 两个数的简单计算器
local line = io.read("*l")
if not line or line:match("^%s*$") then line = io.read("*a") end
local a_str, op, b_str = line:match("(-?%d+)%s*(%S)%s*(-?%d+)")
local a = tonumber(a_str)
local b = tonumber(b_str)
local valid = { ["+"]=true, ["-"]=true, ["*"]=true, ["/"]=true, ["%"]=true }
if not valid[op] then print("ERROR"); return end
if (op == "/" or op == "%") and b == 0 then print("ERROR"); return end
local result
if op == "+" then result = a + b
elseif op == "-" then result = a - b
elseif op == "*" then result = a * b
elseif op == "/" then result = math.modf(a / b) -- C 语义向零截断
elseif op == "%" then local q = math.modf(a / b); result = a - q * b end
print(result)
```

与完整代码一致：用 `(-?%d+)%s*(%S)%s*(-?%d+)` 解析负数，`math.modf` 实现 C 语义的向零截断除法/求余，避免 Lua `//`、`%` 的 floor 语义在负数时错误（如 `-7/2` 应为 `-3` 而非 `-4`）。

## 复杂度分析

程序只解析两个整数并执行一次运算，时间复杂度为 O(1)，额外空间复杂度为 O(1).

## 常见易错点

- 除法需要使用整数除法的向零截断规则，不能直接使用 Lua 的向下取整除法替代。
- 只接受五种运算符，非法符号以及除零情况都应输出 ERROR。

## 更多测试

边界测试：输入 8 / 3，预期输出 2，验证整数除法向零截断。

特殊测试：输入 -8 % 3，预期输出 -2，验证负数求余。

## 总结

本题的核心在于理清「两个数的简单计算器」的输入输出关系与边界处理：先按格式读取输入，再依据规则逐步计算或遍历，最后按规范输出。该思路可迁移到同类格式化输入输出与模拟计算的题目。
