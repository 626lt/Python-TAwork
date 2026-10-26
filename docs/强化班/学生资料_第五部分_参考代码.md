# 第五部分：上机练习参考代码

## 1. 打印 n 行 m 列的星号矩形

依次输入行数 `n` 和列数 `m`。

```python
n = int(input("请输入行数 n："))
m = int(input("请输入列数 m："))

for i in range(n):
    for j in range(m):
        print("*", end="")
    print()  # 一行结束，换行
```

## 2. 打印 1～20，但跳过所有 3 的倍数

```python
for i in range(1, 21):
    if i % 3 == 0:
        continue
    print(i)
```

## 3. 打印九九乘法表

第 `i` 行打印从 `1 × i` 到 `i × i` 的乘法算式。

```python
for i in range(1, 10):
    for j in range(1, i + 1):
        print(f"{j}x{i}={i*j}", end="\t")
    print()
```

## 4. 不断输入成绩，输入 -1 结束，输出平均分

`-1` 仅用于结束输入，不参与总分和人数的计算。

```python
total = 0
count = 0

while True:
    score = float(input("请输入成绩（-1 结束）："))
    if score == -1:
        break

    total = total + score
    count = count + 1

if count > 0:
    average = total / count
    print("平均分：", average)
else:
    print("没有输入成绩，无法计算平均分。")
```

## 5. 用麦克劳林级数求自然常数 e

公式：`e = 1 + 1/1! + 1/2! + 1/3! + …`。

输入正数 `eps`，逐项累加，直到当前项小于 `eps` 时停止。这里将该项加入后结束循环。通过“上一项除以当前序号”得到下一项，无须重复计算阶乘。

```python
eps = float(input("请输入精度 eps（例如 1e-12）："))

if eps <= 0:
    print("eps 必须大于 0。")
else:
    e = 1.0
    term = 1.0
    n = 1

    while True:
        term = term / n  # 当前项：1/n!
        e = e + term
        if term < eps:
            break
        n = n + 1

    print(f"{e:.10f}")
```

输入 `1e-12` 时，输出：

```text
2.7182818285
```
