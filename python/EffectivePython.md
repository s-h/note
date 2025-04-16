# Effective python 读书笔记
## 字典get方法
```python
fruit = {
    "apple": 10,
    "banana": 8,
    "lemon": 5,
}
fruit.get("apple", 0)
```
在Python字典的`get`方法中，第二个参数（如示例中的`0`）的作用是**提供默认值**。具体来说：

1. **当键存在时**：无论是否提供第二个参数，`get()`都会返回该键对应的实际值（第二个参数会被忽略）。
2. **当键不存在时**：若提供了第二个参数，则返回该参数的值；若未提供，则默认返回`None`。

## 海象表达式
### **核心作用**
海象表达式允许在条件判断、循环、推导式等场景中，**将赋值和表达式结合**，减少重复代码，提升代码简洁性。

---

### **典型使用场景**

#### 1. **简化循环中的重复计算**
传统写法需要重复调用函数或计算：
```python
while True:
    data = input("输入内容: ")
    if not data:
        break
    process(data)
```

使用海象表达式后：
```python
while (data := input("输入内容: ")) != "":
    process(data)
```

---

#### 2. **条件判断中捕获变量值**
传统写法需要先计算再判断：
```python
n = calculate_value()
if n > 10:
    print(f"结果: {n}")
```

使用海象表达式合并赋值和判断：
```python
if (n := calculate_value()) > 10:
    print(f"结果: {n}")
```

---

#### 3. **列表推导式中的高效操作**
传统写法需要重复调用函数：
```python
results = [func(x) for x in items if func(x) > 0]
```

使用海象表达式避免重复计算：
```python
results = [y for x in items if (y := func(x)) > 0]
```

---

### **注意事项**
1. **不是完全替代赋值语句**
   海象表达式适用于需要同时赋值和使用的场景，不能替代常规的变量赋值（如全局变量）。

2. **作用域问题**
   在列表推导式或生成器表达式中，海象表达式赋值的变量可能具有与预期不同的作用域（Python 3.8+ 已优化）。

3. **可读性平衡**
   虽然简化代码，但过度使用可能导致可读性下降，需根据场景权衡。

---

### **总结**
海象表达式 (`:=`) 在以下场景非常有用：
- 需要重复计算或调用函数时（避免冗余代码）
- 在条件判断或循环中需要同时赋值和使用变量时
- 简化推导式中的复杂逻辑

合理使用可以提升代码简洁性，但需注意避免滥用导致代码难以理解。

## defaultdict

- **用途**：为字典提供默认值，避免 `KeyError`。
- **场景**：分组聚合、构建复杂数据结构（如树）。
- **示例**：
```python
from collections import defaultdict
dd = defaultdict(list)
dd['fruits'].append('apple')  # 自动初始化空列表
```


初始化一个默认值为空集合的字典，用于存储键对应的不重复元素集合。defaultdict(set)会在访问不存在的键时自动创建新set。
```python
from collections import defaultdict

class Visits:
    def __init__(self):
        self.data = defaultdict(set)

    def add(self, country, city):
        self.data[country].add(city)


vists = Vists()
vists.add('England', 'Bath')
vists.add('England', 'London')
vists.add('England', 'London')
print(vists.data) 
>>
defaultdict(<class 'set'>, {'England': {'London', 'Bath'}})
```