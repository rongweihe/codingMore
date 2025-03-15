# 字典树

```go
package main

import (
	"fmt"
)

// TrieNode 表示字典树的一个节点
type TrieNode struct {
	children map[rune]*TrieNode // 子节点映射
	isEnd    bool               // 是否是单词的结尾
}

// Trie 表示字典树
type Trie struct {
	root *TrieNode // 根节点
}

// NewTrie 创建一个新的字典树
func NewTrie() *Trie {
	return &Trie{
		root: &TrieNode{children: make(map[rune]*TrieNode)},
	}
}

// Insert 向字典树中插入一个单词
func (t *Trie) Insert(word string) {
	node := t.root
	for _, char := range word {
		if _, exists := node.children[char]; !exists {
			node.children[char] = &TrieNode{children: make(map[rune]*TrieNode)}
		}
		node = node.children[char]
	}
	node.isEnd = true
}

// Search 查询字典树中是否存在一个单词
func (t *Trie) Search(word string) bool {
	node := t.root
	for _, char := range word {
		if _, exists := node.children[char]; !exists {
			return false
		}
		node = node.children[char]
	}
	return node.isEnd
}

// StartsWith 查询字典树中是否存在以某个前缀开头的单词
func (t *Trie) StartsWith(prefix string) bool {
	node := t.root
	for _, char := range prefix {
		if _, exists := node.children[char]; !exists {
			return false
		}
		node = node.children[char]
	}
	return true
}

func main() {
	// 创建一个新的字典树
	trie := NewTrie()

	// 插入一些单词
	trie.Insert("apple")
	trie.Insert("app")
	trie.Insert("banana")
	trie.Insert("orange")

	// 查询单词
	fmt.Println("Search 'apple':", trie.Search("apple"))       // true
	fmt.Println("Search 'app':", trie.Search("app"))           // true
	fmt.Println("Search 'banana':", trie.Search("banana"))     // true
	fmt.Println("Search 'orange':", trie.Search("orange"))     // true
	fmt.Println("Search 'grape':", trie.Search("grape"))       // false

	// 查询前缀
	fmt.Println("StartsWith 'app':", trie.StartsWith("app"))   // true
	fmt.Println("StartsWith 'ban':", trie.StartsWith("ban"))   // true
	fmt.Println("StartsWith 'ora':", trie.StartsWith("ora"))   // true
	fmt.Println("StartsWith 'gra':", trie.StartsWith("gra"))   // false
}
```

## 代码说明

TrieNode 结构体：

- children：一个映射，存储当前节点的子节点，键是字符（rune），值是子节点的指针。
- isEnd：一个布尔值，表示当前节点是否是一个单词的结尾。

Trie 结构体：

- root：字典树的根节点，初始化时创建一个空的 TrieNode。

NewTrie 函数：

- 创建一个新的字典树实例，初始化根节点。
- Insert 方法：遍历单词的每个字符，逐层创建节点，直到单词结束。在单词的最后一个字符节点上标记 isEnd 为 true。
- Search 方法：遍历单词的每个字符，逐层查找节点。如果某个字符不存在于当前节点的子节点中，返回 false。如果遍历完单词且最后一个字符节点的 isEnd 为 true，则返回 true。
- StartsWith 方法：遍历前缀的每个字符，逐层查找节点。如果某个字符不存在于当前节点的子节点中，返回 false。如果遍历完前缀，返回 true。

测试结果

运行上述代码，输出如下：
```go

Search 'apple': true
Search 'app': true
Search 'banana': true
Search 'orange': true
Search 'grape': false
StartsWith 'app': true
StartsWith 'ban': true
StartsWith 'ora': true
StartsWith 'gra': false
```
