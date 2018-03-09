### iOS 的深复制和浅复制

#### 1. 复制的概念

- 浅复制是指针复制，并不拷贝对象本身
- 深复制是内容复制，拷贝对象到另一块内存中

#### 2. 单层深复制

- 浅复制(shallow copy)：在浅复制操作时，对于被复制对象的每一层都是指针复制。
- 深复制(one-level-deep copy)：在深复制操作时，对于被复制对象，至少有一层是深复制。
- 完全复制(real-deep copy)：在完全复制操作时，对于被复制对象的每一层都是对象复制。

#### 3. 系统对象的 copy 和 multableCopy 方法

##### 3.1 非集合类对象的 copy 与 mutableCopy

在非集合类对象中：对 immutable 对象进行 copy 操作，是指针复制，mutableCopy 操作时内容复制；对 mutable对象进行 copy 和 mutableCopy 都是内容复制。用代码简单表示如下：

- [immutableObject copy] // 浅复制
- [immutableObject mutableCopy] //深复制
- [mutableObject copy] //深复制
- [mutableObject mutableCopy] //深复制

##### 3.2 集合类对象的 copy 与 mutableCopy

在集合类对象中，对 immutable 对象进行 copy，是指针复制，mutableCopy 是内容复制；对 mutable 对象进行copy 和 mutableCopy 都是内容复制。但是：集合对象的内容复制仅限于对象本身，对象元素仍然是指针复制。用代码简单表示如下：

- [immutableObject copy] // 浅复制
- [immutableObject mutableCopy] //单层深复制
- [mutableObject copy] //单层深复制
- [mutableObject mutableCopy] //单层深复制