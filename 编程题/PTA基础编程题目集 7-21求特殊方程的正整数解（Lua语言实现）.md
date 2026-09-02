# 7-21 求特殊方程的正整数解（Lua语言实现）

## 前言

本题（7-21 求特殊方程的正整数解）的主要考点是：准确理解题意、规范处理输入输出格式，并正确处理边界与精度。下面先给出题目描述与格式要求，再通过清晰的思路与可直接运行的lua代码进行讲解。

## 题目描述

本题要求对任意给定的正整数N，求方程X² + Y² = N的全部正整数解。

## 输入格式
输入在一行中给出正整数N（≤10000）。

## 输出格式
输出方程X² + Y² = N的全部正整数解，其中X≤Y。每组解占1行，两数字间以1空格分隔，按X的递增顺序输出。如果没有解，则输出No Solution。

## 输入样例
```in
884
```


### 输入样例2：
```in
11
```
## 输出样例
```out
10 28
20 22
```


### 输出样例2：
```out
No Solution
```
## 解题思路

### 1. 核心问题分析

给定正整数N，找出所有满足X² + Y² = N且X≤Y的正整数对(X, Y)。由于N最大为10000，X和Y的最大值都不超过100，暴力枚举完全可行。

### 2. 算法原理

采用双重循环枚举法。外层循环枚举X（从1到√N），内层循环枚举Y（从X到√N），对每对(X, Y)判断是否满足方程X² + Y² = N。Y从X开始枚举保证了X≤Y的约束条件。

### 3. 具体计算步骤

1. 读取正整数N
2. 初始化标记found=0表示未找到解
3. X从1开始递增，直到X² > N为止
4. 对每个X，Y从X开始递增，直到Y² > N为止
5. 若X² + Y² = N，输出该组解并标记found=1
6. 遍历结束后若found仍为0，输出"No Solution"

## 完整代码

```lua
-- 7-21 求特殊方程的正整数解
local N = tonumber(io.read("*n"))
local found = 0

for X = 1, math.floor(math.sqrt(N)) do
    for Y = X, math.floor(math.sqrt(N)) do
        if X*X + Y*Y == N then
            print(X .. " " .. Y)
            found = 1
        end
    end
end

if found == 0 then
    print("No Solution")
end
```

## 代码流程说明

1. **变量声明**：使用local关键字声明N存储输入值，found标记是否找到解（初始为0）
2. **输入读取**：使用tonumber(io.read("*n"))读取正整数N
3. **外层循环（X枚举）**：for X = 1, math.floor(math.sqrt(N)) do，枚举X从1到√N
4. **内层循环（Y枚举）**：for Y = X, math.floor(math.sqrt(N)) do，枚举Y从X到√N
5. **方程判断**：若X*X + Y*Y == N，则使用print(X .. " " .. Y)输出解，设置found=1
6. **无解判断**：双重循环结束后，若found == 0，输出"No Solution"
7. **程序结束**：脚本执行完毕

## 代码流程图

```mermaid
flowchart TD
    A[开始] --> B[声明变量N,X,Y,found=0]
    B --> C[输入N]
    C --> D[X=1]
    D --> E{X*X <= N?}
    E -->|否| K{found==0?}
    E -->|是| F[Y=X]
    F --> G{Y*Y <= N?}
    G -->|否| J[X++]
    J --> E
    G -->|是| H{X*X+Y*Y == N?}
    H -->|是| I[输出X Y, found=1]
    I --> Y1[Y++]
    H -->|否| Y1[Y++]
    Y1 --> G
    K -->|是| L[输出No Solution]
    K -->|否| M[结束]
    L --> M
```

## 解题流程图

```mermaid
flowchart TD
    A["输入正整数N"] --> B["X=1"]
    B --> C{"X² <= N?"}
    C -->|否| H{"是否找到解?"}
    C -->|是| D["Y=X"]
    D --> E{"Y² <= N?"}
    E -->|否| G["X++"]
    G --> C
    E -->|是| F{"X²+Y² == N?"}
    F -->|是| F1["输出X Y，标记找到解"]
    F1 --> F2["Y++"]
    F -->|否| F2["Y++"]
    F2 --> E
    H -->|是| I["正常结束"]
    H -->|否| J["输出No Solution"]
    J --> I
```

## 代码解析

```lua
-- 7-21 求特殊方程的正整数解
local N = tonumber(io.read("*n"))
local found = 0

for X = 1, math.floor(math.sqrt(N)) do
    for Y = X, math.floor(math.sqrt(N)) do
        if X*X + Y*Y == N then
            print(X .. " " .. Y)
            found = 1
        end
    end
end

if found == 0 then
    print("No Solution")
end
```

变量声明

## 复杂度分析

两个循环的上界均为 floor(sqrt(N))，总枚举规模为 O(N)，额外空间复杂度为 O(1).

## 常见易错点

- 只枚举正整数解，并从 Y=X 开始以保证 X≤Y，避免重复输出。
- 遍历结束后要根据 found 标记决定是否输出 No Solution。

## 更多测试

边界测试：输入 25，预期输出 3 4。

特殊测试：输入 2，预期输出 No Solution，验证无正整数解时的处理。

## 总结

本题的核心在于理清「求特殊方程的正整数解」的输入输出关系与边界处理：先按格式读取输入，再依据规则逐步计算或遍历，最后按规范输出。该思路可迁移到同类格式化输入输出与模拟计算的题目。
