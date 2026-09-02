# 7-33 有理数加法（Lua语言实现）

## 前言

本题（7-33 有理数加法）的主要考点是：准确理解题意、规范处理输入输出格式，并正确处理边界与精度。下面先给出题目描述与格式要求，再通过清晰的思路与可直接运行的lua代码进行讲解。

## 题目描述

本题要求编写程序，计算两个有理数的和。

## 输入格式

输入在一行中按照a1/b1 a2/b2的格式给出两个分数形式的有理数，其中分子和分母全是整形范围内的正整数。

## 输出格式

在一行中按照a/b的格式输出两个有理数的和。注意必须是该有理数的最简分数形式，若分母为1，则只输出分子。

## 输入样例

```in
1/3 1/6
```

```in
4/3 2/3
```

## 输出样例

```out
1/2
```

```out
2
```

## 解题思路

### 核心问题分析
本题需要计算两个分数的和并输出最简形式。核心难点在于：一是正确解析带斜杠的输入格式（如"1/3 1/6"），二是分数相加后的约分处理。例如1/3 + 1/6 = 6/18 + 3/18 = 9/18，约分后为1/2。

### 算法原理说明
1. **分数加法公式**：a1/b1 + a2/b2 = (a1×b2 + a2×b1) / (b1×b2)
2. **约分算法**：使用辗转相除法（欧几里得算法）求分子和分母的最大公约数（GCD），然后分子分母同时除以GCD得到最简分数。
3. **辗转相除法原理**：gcd(a, b) = gcd(b, a mod b)，当b=0时a即为最大公约数。
- 时间复杂度O(log(min(a,b)))：由辗转相除法的复杂度决定
- 空间复杂度O(1)：仅需几个整数变量

### 具体计算步骤
1. 按"a1/b1 a2/b2"格式读取两个分数的分子和分母
2. 计算通分后的分子：numerator = a1×b2 + a2×b1
3. 计算通分后的分母：denominator = b1×b2
4. 求分子和分母的最大公约数g = gcd(numerator, denominator)
5. 约分：numerator = math.floor(numerator / g)，denominator = math.floor(denominator / g)
6. 若分母为1，只输出分子；否则输出"分子/分母"格式

## 完整代码

```lua
-- 7-33 有理数加法
local function gcd(a, b)
    if a < 0 then a = -a end
    if b < 0 then b = -b end
    while b ~= 0 do
        local temp = b
        b = a % b
        a = temp
    end
    return a
end

local line = io.read("*l") or ""
if line ~= "" then
    local a1, b1, a2, b2 = line:match("(%-?%d+)/(%d+)%s+(%-?%d+)/(%d+)")
    if a1 and b1 and a2 and b2 then
        a1 = tonumber(a1); b1 = tonumber(b1)
        a2 = tonumber(a2); b2 = tonumber(b2)
        local numerator = a1*b2 + a2*b1
        local denominator = b1 * b2
        local g = gcd(numerator, denominator)
        if g ~= 0 then
            numerator = math.floor(numerator / g)
            denominator = math.floor(denominator / g)
        end
        if denominator == 1 then
            print(numerator)
        else
            print(numerator .. "/" .. denominator)
        end
    end
end
```

## 代码流程说明

1. **定义gcd函数**：local function gcd(a, b)循环实现辗转相除法求最大公约数，内部先对a、b取绝对值
2. **读取输入行**：local line = io.read()读取整行输入
3. **解析分数**：使用line:match模式匹配(%-?%d+)/(%d+)%s+(%-?%d+)/(%d+)提取a1、b1、a2、b2四个字符串
4. **转换数值**：使用tonumber将四个字符串转为整数
5. **计算和**：按分数加法公式计算分子numerator和分母denominator
6. **求GCD**：调用gcd函数求分子分母的最大公约数
7. **约分**：分子分母分别除以最大公约数，使用math.floor确保整数结果
8. **输出结果**：判断分母是否为1，使用print输出对应格式（拼接用..运算符）
9. **程序自然结束**

## 代码流程图

```mermaid
flowchart TD
    A["开始"] --> B["local function gcd(a, b)定义函数"]
    B --> C["a、b取绝对值"]
    C --> D["while b ~= 0 do"]
    D --> E{"循环结束?"}
    E -- 否 --> F["temp = b, b = a % b, a = temp"]
    F --> D
    E -- 是 --> G["return a"]
    G --> H["local line = io.read读取整行"]
    H --> I["line:match模式匹配提取a1,b1,a2,b2"]
    I --> J["tonumber转换为整数"]
    J --> K["numerator = a1*b2 + a2*b1"]
    K --> L["denominator = b1 * b2"]
    L --> M["g = gcd(numerator, denominator)"]
    M --> N["numerator = math.floor(numerator / g)"]
    N --> O["denominator = math.floor(denominator / g)"]
    O --> P{"denominator == 1?"}
    P -- 是 --> Q["print(numerator)"]
    P -- 否 --> R["print(numerator..'/'..denominator)"]
    Q --> S["程序结束"]
    R --> S
```

## 解题流程图

```mermaid
flowchart TD
    A[输入a1/b1 a2/b2] --> B[解析4个整数]
    B --> C[计算分子 = a1*b2 + a2*b1]
    C --> D[计算分母 = b1*b2]
    D --> E[求分子分母的GCD]
    E --> F[分子分母同除以GCD,用math.floor取整]
    F --> G{分母 == 1?}
    G -- 是 --> H[输出分子]
    G -- 否 --> I[输出 分子/分母]
    H --> J[完成]
    I --> J
```

## 代码解析

```lua
-- 7-33 有理数加法
local function gcd(a, b)
    if a < 0 then a = -a end
    if b < 0 then b = -b end
    while b ~= 0 do
        local temp = b
        b = a % b
        a = temp
    end
    return a
end

local line = io.read("*l") or ""
if line ~= "" then
    local a1, b1, a2, b2 = line:match("(%-?%d+)/(%d+)%s+(%-?%d+)/(%d+)")
    if a1 and b1 and a2 and b2 then
        a1 = tonumber(a1); b1 = tonumber(b1)
        a2 = tonumber(a2); b2 = tonumber(b2)
        local numerator = a1*b2 + a2*b1
        local denominator = b1 * b2
        local g = gcd(numerator, denominator)
        if g ~= 0 then
            numerator = math.floor(numerator / g)
            denominator = math.floor(denominator / g)
        end
        if denominator == 1 then
            print(numerator)
        else
            print(numerator .. "/" .. denominator)
        end
    end
end
```

定义gcd函数

## 复杂度分析

输入只包含两个分数，约分使用辗转相除法，时间复杂度为 O(log M)，其中 M 为结果分子或分母的数量级，额外空间复杂度为 O(1).

## 常见易错点

- 分子可以为负，最大公约数计算应使用绝对值，约分后分母保持为正。
- 结果分母为 1 时只输出分子，不能继续输出 /1。

## 更多测试

边界测试：输入 4/3 2/3，预期输出 2，验证分母约分为 1。

特殊测试：输入 -1/2 1/2，预期输出 0，验证结果分子为零。

## 总结

本题的核心在于理清「有理数加法」的输入输出关系与边界处理：先按格式读取输入，再依据规则逐步计算或遍历，最后按规范输出。该思路可迁移到同类格式化输入输出与模拟计算的题目。
