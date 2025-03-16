+++
date = '2025-03-16T21:42:23+08:00'
title = '一级标题：综合样式测试文章'
categories= "测试"
tags = ['测试1', '测试2']
description = "测试描述"
featured_image = "/images/pexels-christian-heitz-285904-842711.jpg"
+++

## 二级标题：文字排版测试

**粗体文字** *斜体文字* ~~删除线~~ `行内代码`

<!--more-->

> 引用区块：昨夜星辰昨夜风，画楼西畔桂堂东。身无彩凤双飞翼，心有灵犀一点通。

无序列表：
- 列表项一
- 列表项二
  - 嵌套列表项
  - 嵌套列表项

有序列表：
1. 第一项
2. 第二项
3. 第三项

---

## 二级标题：代码块测试

### 三级标题：基础代码块
```python
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

print(list(fibonacci(10)))
```

### 三级标题：带行号代码块
```javascript {.line-numbers}
function debounce(func, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}
```

---

## 二级标题：数学公式测试

### 三级标题：行内公式
质能方程 $E=mc^2$ 是狭义相对论的重要结论

### 三级标题：块级公式
$$
\begin{cases}
\frac{\partial u}{\partial t} + u \cdot \nabla u = -\frac{1}{\rho}\nabla p + \nu \nabla^2 u \\
\nabla \cdot u = 0
\end{cases}
$$

---

## 二级标题：图片测试

![默认图片](/images/pexels-christian-heitz-285904-842711.jpg)

---

## 二级标题：表格测试

| 算法         | 时间复杂度 | 空间复杂度 |
|--------------|------------|------------|
| 快速排序     | O(n log n) | O(log n)   |
| 冒泡排序     | O(n²)      | O(1)       |
| 归并排序     | O(n log n) | O(n)       |

---

## 二级标题：特殊元素

水平分割线测试：
***

脚注测试[^1]

[^1]: 这是一个示例脚注内容