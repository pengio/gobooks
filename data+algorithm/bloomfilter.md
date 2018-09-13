## bloom-filter

---

### 常见散列函数

```go
/**
 * 加法 hash
 */
function additiveHash(string, range) {
    let hash, i;
    for (hash = string.length, i = 0; i < string.length; i++) {
        hash = hash + string.charCodeAt(i);
    }
    return range ? hash % range : hash;
}

/**
 * 位运算 HASH
 */
function rotatingHash(string, range) {
    let hash, i;
    for (hash = string.length, i = 0; i < string.length; i++) {
        hash = hash << 4 ^ hash >> 28 ^ string.charCodeAt(i);
    }
    hash = range ? hash % range : hash;
    return Math.abs(hash);
}

/**
 * 乘法 hash
 * 推荐乘数可以是31.131.13131.13131...
 */
function bernsteinHash(string, range) {
    let hash = 0,
        i;
    for (i = 0; i < string.length; i++) hash = 131 * hash + string.charCodeAt(i);
    return range ? hash % range : hash;
}

/**
 * 改进后的 FNVHash
 */
function FNVHash(string, range) {
    let p = 16777619,
        hash = 2166136261;
    for (let i = 0; i < string.length; i++) hash = (hash ^ string.charCodeAt(i)) * p;
    hash += hash << 13;
    hash ^= hash >> 7;
    hash += hash << 3;
    hash ^= hash >> 17;
    hash += hash << 5;
    return range ? hash % range : hash;
}
```

## 分布及其性能 {#anchor3}

在这里，笔者对每种 hash 散列算法的分布情况\(10000000个无规律数据分布在1000个点上\)和性能（node v8）进行了一个简单的测试，比较一下各算法的优劣。

| 算法 | 分布 | 性能 |
| :--- | :--- | :--- |
| 加法 | 差 | 优 |
| 位运算 | 优 | 优 |
| 乘法 | 良 | 优 |
| FNVHash | 优 | 良 |

> 统计方式和详细结果会在稍后给出

由此可见，一般的话，我们选择**位运算 hash 算法**是最合适的。笔者的统计可能比较我狭义，不能代表其它的算法就完全没有用，这个有机会会对些进行进一步研究。

---





