---
title: AC自动机的构建步骤
date: 2024-06-05 20:12:38
tags: 算法
---
# AC自动机详解及Python实现

## 什么是AC自动机？

AC自动机（Aho-Corasick Automaton）是一种用于多模式字符串匹配的高效算法。它可以在一个文本中同时查找多个模式串，时间复杂度为 \(O(n + m + z)\)，其中 \(n\) 是文本长度，\(m\) 是所有模式串的总长度，\(z\) 是匹配结果的数量。

AC自动机结合了字典树（Trie）和失败指针的概念，能够实现快速的多模式查找。它首先构建一个字典树，然后为每个节点建立失败指针，从而在匹配过程中能够有效地回溯。

## AC自动机的构建步骤

1. **构建字典树**：将所有模式串插入到字典树中。
2. **建立失败指针**：为字典树中的每个节点建立失败指针，指向该节点的最长后缀匹配节点。
3. **匹配过程**：在文本中逐字符匹配，利用字典树和失败指针快速找到所有模式串。

## Python实现

以下是一个简单的AC自动机实现示例，包含字典树的构建、失败指针的建立以及匹配过程。

```python
class ACNode:
    def __init__(self):
        self.children = {}
        self.fail = None
        self.output = []

class ACAutomaton:
    def __init__(self):
        self.root = ACNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = ACNode()
            node = node.children[char]
        node.output.append(word)

    def build_failure_pointers(self):
        from collections import deque
        queue = deque()
        # Initialize the first level children of root
        for child in self.root.children.values():
            child.fail = self.root
            queue.append(child)

        while queue:
            current_node = queue.popleft()
            
            for char, child in current_node.children.items():
                # Find the fail pointer
                fail_node = current_node.fail
                while fail_node and char not in fail_node.children:
                    fail_node = fail_node.fail
                child.fail = fail_node.children[char] if fail_node else self.root
                # Merge output lists
                if child.fail:
                    child.output += child.fail.output
                queue.append(child)

    def search(self, text):
        node = self.root
        results = []
        
        for index, char in enumerate(text):
            while node and char not in node.children:
                node = node.fail
            if not node:
                node = self.root
                continue
            node = node.children[char]
            if node.output:
                for pattern in node.output:
                    results.append((index - len(pattern) + 1, pattern))
        
        return results
```

### 示例用法
``` python
if __name__ == "__main__":
    patterns = ["he", "she", "his", "hers"]
    text = "ushers"

    ac = ACAutomaton()
    for pattern in patterns:
        ac.insert(pattern)
    
    ac.build_failure_pointers()
    
    results = ac.search(text)
    for index, pattern in results:
        print(f"找到模式 '{pattern}' 在索引 {index}")
```

## 代码解析
1. ACNode类：每个节点包含一个字典（children）用于存储子节点，一个失败指针（fail），以及一个输出列表（output）用于存储匹配模式。
2. ACAutomaton类：包含插入模式串的方法（insert），构建失败指针的方法（build_failure_pointers），以及搜索文本的方法（search）。
3. 搜索过程：通过逐字符匹配文本，如果当前节点没有对应的子节点，则沿着失败指针回溯，直到找到匹配或回到根节点。
## 总结
AC自动机是一种高效的多模式匹配算法，非常适合用于字符串搜索、文本分析等场景。通过构建字典树和失败指针，AC自动机能够在复杂度上优于传统的逐一匹配方法。希望本文能够帮助你理解AC自动机的基本原理及其在Python中的实现。
