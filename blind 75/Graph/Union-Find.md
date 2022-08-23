## Union-Find

```java
作者：labuladong
链接：https://zhuanlan.zhihu.com/p/98406740
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

class UF {
    // 连通分量个数
    private int count;
    // 存储一棵树
    private int[] parent;
    // 记录树的“重量”
    private int[] size;

    public UF(int n) {
        this.count = n;
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ)
            return;

        // 小树接到大树下面，较平衡
        if (size[rootP] > size[rootQ]) {
            parent[rootQ] = rootP;
            size[rootP] += size[rootQ];
        } else {
            parent[rootP] = rootQ;
            size[rootQ] += size[rootP];
        }
        count--;
    }

    public boolean connected(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        return rootP == rootQ;
    }

    private int find(int x) {
        while (parent[x] != x) {
            // 进行路径压缩
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    public int count() {
        return count;
    }
}
//
```

## more detailed
>**1** 现在我们的 Union-Find 算法主要**需要实现这两个 API**：
```java
class UF {
    /* 将 p 和 q 连接 */
    public void union(int p, int q);
    /* 判断 p 和 q 是否连通 */
    public boolean connected(int p, int q);
    /* 返回图中有多少个连通分量 */
    public int count();
}
```

## 2
```java
class UF {
    // 记录连通分量
    private int count;
    // 节点 x 的节点是 parent[x]
    private int[] parent;

    /* 构造函数，n 为图的节点总数 */
    public UF(int n) {
        // 一开始互不连通
        this.count = n;
        // 父节点指针初始指向自己
        parent = new int[n];
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }

    /* 其他函数 */
}


```

## 3
```java
如果某两个节点被连通，则让其中的（任意）一个节点的根节点接到另一个节点的根节点上：public void union(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ)
        return;
    // 将两棵树合并为一棵
    parent[rootP] = rootQ;
    // parent[rootQ] = rootP 也一样
    count--; // 两个分量合二为一
}

/* 返回某个节点 x 的根节点 */
private int find(int x) {
    // 根节点的 parent[x] == x
    while (parent[x] != x)
        x = parent[x];
    return x;
}

/* 返回当前的连通分量个数 */
public int count() { 
    return count;
}

```

## 4 
>这样，如果节点p和q连通的话，它们一定拥有相同的根节点：
```java
public boolean connected(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    return rootP == rootQ;
}
```

>那么这个算法的复杂度是多少呢？我们发现，主要 API **connected**和**union**中的复杂度都是find函数造成的，所以说它们的复杂度和find一样。find主要功能就是从某个节点向上遍历到树根，**其时间复杂度就是树的高度。我们可能习惯性地认为树的高度就是logN**，但这并不一定。logN的高度只存在于平衡二叉树，对于一般的树可能出现极端不平衡的情况，使得「树」几乎退化成「链表」，**树的高度最坏情况下可能变成N**。

## 5 平衡与优化
>引入 size （理解成重量） 通过比较size，决定插入什么地方(等号理解成指针 指向)
```java
public void union(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ)
        return;

    // 小树接到大树下面，较平衡
    if (size[rootP] > size[rootQ]) {
        parent[rootQ] = rootP;
        size[rootP] += size[rootQ];
    } else {
        parent[rootP] = rootQ;
        size[rootQ] += size[rootP];
    }
    count--;
}


```
## 6 路径压缩
> 很漂亮的一步
> 我们能不能进一步压缩每棵树的高度，使树高始终保持为常数？这样find就能以 O(1) 的时间找到某一节点的根节点，相应的，connected和union复杂度都下降为 O(1)。
>总结:节点 2 向上寻找，也不是根节点，那么把元素 2 指向原来父节点的父节点，操后后树的层数相应减少了一层，
```java
private int find(int x) {
    while (parent[x] != x) {
        // 进行路径压缩
        parent[x] = parent[parent[x]];
        x = parent[x];
    }
    return x;
}

```




