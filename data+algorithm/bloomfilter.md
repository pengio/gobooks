## bloom-filter 

---

#### 常见散列函数

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





