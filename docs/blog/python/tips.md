---
icon: simple/python
---

# Python 实用技巧

:material-calendar: 2026-03-06 · :material-tag: Python, 编程技巧

---

分享几个我在日常开发中常用的 Python 技巧，希望能帮你写出更简洁、更 Pythonic 的代码。

## 1. 列表推导式的妙用

列表推导式是 Python 最优雅的特性之一。

``` python title="基础用法"
# 传统写法
squares = []
for x in range(10):
    squares.append(x ** 2)

# 列表推导式
squares = [x ** 2 for x in range(10)]
```

带条件过滤：

``` python title="条件过滤"
# 只保留偶数的平方
even_squares = [x ** 2 for x in range(10) if x % 2 == 0]
# [0, 4, 16, 36, 64]
```

!!! tip "性能提示"

    列表推导式通常比等价的 `for` 循环快 10-30%，因为它在 C 层面进行了优化。

## 2. 装饰器：优雅的函数增强

装饰器让你在不修改原函数的情况下为其添加功能。

``` python title="计时装饰器" hl_lines="3-10"
import time
from functools import wraps

def timer(func):
    """测量函数执行时间的装饰器"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} 执行耗时: {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "done"
```

## 3. 上下文管理器

`with` 语句不仅能用于文件操作，你也可以自定义上下文管理器。

=== "类实现"

    ``` python
    class DatabaseConnection:
        def __enter__(self):
            self.conn = create_connection()
            return self.conn

        def __exit__(self, exc_type, exc_val, exc_tb):
            self.conn.close()
            return False  # 不吞掉异常

    with DatabaseConnection() as conn:
        conn.execute("SELECT * FROM users")
    ```

=== "contextmanager 装饰器"

    ``` python
    from contextlib import contextmanager

    @contextmanager
    def database_connection():
        conn = create_connection()
        try:
            yield conn
        finally:
            conn.close()

    with database_connection() as conn:
        conn.execute("SELECT * FROM users")
    ```

## 4. f-string 高级格式化

Python 3.6+ 的 f-string 远比你想象的强大。

``` python title="f-string 技巧"
name = "Python"
version = 3.12

# 对齐和填充
print(f"{'左对齐':<20}")
print(f"{'右对齐':>20}")
print(f"{'居中':^20}")

# 数字格式化
pi = 3.14159265
print(f"保留两位小数: {pi:.2f}")      # 3.14
print(f"百分比: {0.856:.1%}")          # 85.6%
print(f"千位分隔: {1000000:,}")        # 1,000,000

# 调试模式（3.8+）
x = 42
print(f"{x = }")  # x = 42
```

## 5. walrus 运算符 `:=`

Python 3.8 引入的海象运算符可以在表达式中赋值。

``` python title="海象运算符"
# 读取文件直到空行
while (line := input()) != "":
    process(line)

# 在列表推导式中避免重复计算
results = [
    y
    for x in data
    if (y := expensive_computation(x)) > threshold
]
```

!!! warning "使用建议"

    海象运算符虽然好用，但不要滥用。只在能明显提升可读性的地方使用它。

## 总结

| 技巧 | 适用场景 | Python 版本 |
|------|---------|------------|
| 列表推导式 | 数据转换和过滤 | 2.0+ |
| 装饰器 | 横切关注点（日志、缓存等） | 2.4+ |
| 上下文管理器 | 资源管理 | 2.5+ |
| f-string | 字符串格式化 | 3.6+ |
| 海象运算符 | 表达式内赋值 | 3.8+ |

希望这些技巧对你有所帮助！如果你有其他实用的 Python 技巧，欢迎交流。:sparkles:
