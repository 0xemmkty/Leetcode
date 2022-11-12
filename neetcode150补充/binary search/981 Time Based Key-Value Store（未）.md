# 981. Time Based Key-Value Store (未)

>Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

>Implement the TimeMap class:

>TimeMap() Initializes the object of the data structure.
void set(String key, String value, int timestamp) Stores the key key with the value value at the given time timestamp.
String get(String key, int timestamp) Returns a value such that set was called previously, with timestamp_prev <= timestamp. If there are multiple such values, it returns the value associated with the largest timestamp_prev. If there are no values, it returns "".

```
解题:
题目分析

相信挺多人一开始看到题目都没弄清楚题目在说什么，设计题都这样，有点不知所云

在这个题目中，我们需要构建一个类用于存储数据，同时实现他的两个方法，set与get，也即是存值与取值

问题在于存的是什么样的值呢？

是用典型的 [key-value]形式的键值对拓展而来，但是有需要带有另一个非空不重复的元素timestamps
可以理解为双主键
构建的时候，ttimestamps总是随着方法调用而递增的，那么说明其总是有序的
如果key重复，也不做value的替换，因为timestamps还不同
那么就意味着，一个key可能要对应着多个值
当然因为timestamps的特殊性质，也可以理解为双主键
解题思路

最主要的问题就是如何设计存储的数据结构

如果采用双主键的形式，那么首先想到的是嵌套HashMap的形式：

Map<key, Map<time, value >>
大概形式如下所示：

key1:
​ time1: value1
​ time2: value1
key2:
​ time4: value1
​ time5: value1

但是还是得分析分析需要实现的两个方法的逻辑再做决定

set：
单纯的添加元素，即将数据存入预设的数据结构
由于timestamps是不重复的，其实也没必要考虑其冲突的情况
get：
首先判断是否存在key，不存在返回空字符串
然后调用 set(key, value, timestamp_prev) 所存储的值，其中 timestamp_prev <= timestamp
如果有多个这样的值，则返回对应最大的 timestamp_prev 的那个值，没有也返回空字符串
也就是说这个时候存在一个查找的过程，HashMap就不是那么合适了
因此，判断不使用HashMap做内层的嵌套
```

设计数据结构的题！！值得好好研究
其实本题重点就是用二分法找最近存入的时间值？

## 思路1：HashMap套List

Map<key, List[time -> value]>

内部使用List存储，这样的话就能更方便地在get中进行查找了

因为是timestamps是单调的，同时有作为内层主键

那么我们可以使用二分法快速确定需要查找的value的位置
```java
class TimeMap {
    // 构建一个内部类，用于存储 时间戳-值 的键值对
    // 当然也可以使用HashMap，但是在get的时候需要顺序查值，还是用数组更好
    class Pair {
        String k, v; 
        int t;
        Pair (String _k, String _v, int _t) {
            k = _k; v = _v; t = _t;
        }
    }
    
    // 定义一个数据结构：hash套数组，因为一个hash主键可能有多个值，那么则使用List将那些值存起来
    Map<String, List<Pair>> map = new HashMap<>();

    // set操作
    public void set(String k, String v, int t) {
        // 寻找是否已经存在Hash主键，没有就创建一个
        List<Pair> list = map.getOrDefault(k, new ArrayList<>());
        // 值添加进list
        list.add(new Pair(k, v, t));
        // list添加进map
        map.put(k, list);
    }
    
    // get操作
    public String get(String k, int t) {
        // 首先判断是否存在HashMap主键
        List<Pair> list = map.getOrDefault(k, new ArrayList<>());
        if (list.isEmpty()){
            return "";
        }

        // 使用二分法找到，最接近 t 的 [time-value]
        int n = list.size();
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (list.get(mid).t <= t) {
                l = mid;
            }
            else {
                r = mid - 1;
            }
        }
        // 判断是否存在 [time-value]
        return list.get(r).t <= t ? list.get(r).v : "";
    }
}

```
## 思路2：HashMap套TreeMap

同样的道理，要是懒也可以考虑在内部使用TreeMap，TreeMap是能自动排序的，而且还有便利的方法，让你二分法都不用写
```java
class TimeMap {

    Map<String, TreeMap<Integer, String>> map = new HashMap<>();

    public TimeMap() {

    }

    public void set(String k, String v, int t) {
        // 寻找是否已经存在Hash主键，没有就创建一个
        TreeMap<Integer, String> ts = map.getOrDefault(k, new TreeMap<Integer, String>());
        // 以time为主键添加进TreeMap
        ts.put(t, v);
        // 将TreeMap添加进HashMap
        map.put(k, ts);

    }

    public String get(String k, int t) {
        // 首先判断是否存在HashMap主键
        TreeMap<Integer, String> ts = map.getOrDefault(k, new TreeMap<>());
        // 然后获取timestamp_prev
        Map.Entry<Integer, String> entry = ts.floorEntry(t);
        // 判断是否为空
        if(entry == null){
            return "";
        }
        // 获取值
        return entry.getValue();
    }

    // 顺便写个删除，没有验证，但是我觉得没问题
    public String del(String k, int t) {
        // 首先判断是否存在HashMap主键
        TreeMap<Integer, String> ts = map.get(k);
        if (ts == null) return null;
        // 然后获取timestamp_prev
        Map.Entry<Integer, String> entry = ts.floorEntry(t);
        if (entry == null) return null;

        // 获取value，然后删除value，并返回value
        String v = entry.getValue();
        ts.remove(t);
        return v;
    }
}

```