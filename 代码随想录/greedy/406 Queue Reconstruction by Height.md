# 406. Queue Reconstruction by Height

>You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

>Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

 
```
Example 1:

Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```

## S1 (贪心)
思路： 按身高排序，从高的开始插入，根据k插
排序完的people： [[7,0], [7,1], [6,1], [5,0], [5,2]，[4,4]]

插入的过程：

插入[7,0]：[[7,0]]
插入[7,1]：[[7,0],[7,1]]
插入[6,1]：[[7,0],[6,1],[7,1]]
插入[5,0]：[[5,0],[7,0],[6,1],[7,1]]
插入[5,2]：[[5,0],[7,0],[5,2],[6,1],[7,1]]
插入[4,4]：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]

>关于出现**两个维度**一起考虑的情况，我们已经做过两道题目了，另一道就是135. 分发糖果。

其技巧都是确定一边然后贪心另一边

(没看懂？看下一个)
```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        // 身高从大到小排（身高相同k小的站前面）
        Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return b[0] - a[0];
        });

        LinkedList<int[]> que = new LinkedList<>();
        // add(int index , E element ) : 
        // inserts the element at the given index .
        for (int[] p : people) {
            que.add(p[1],p);
        }

        return que.toArray(new int[people.length][]);
    }
}
```
```java
class Solution {
     public int[][] reconstructQueue(int[][] people) {
        //按数组第一个元素进行降序，按第二个元素进行升序
        Arrays.sort(people, new Comparator<int[]>() {
            @Override
            public int compare(int[] person1, int[] person2){
                if (person1[0] != person2[0]){
                    //第一个元素不相等时，第一个元素降序
                    return person2[0] - person1[0];
                }else{
                    //第一个元素相等时，第二个元素升序
                    return person1[1] - person2[1];
                }
            }
        });
        //新建一个list,用于保存结果集
        List<int[]> list = new LinkedList<>();
        for (int i = 0; i < people.length; i++) {
            if (list.size() > people[i][1]){
                //结果集中元素个数大于第i个人前面应有的人数时，将第i个人插入到结果集的 people[i][1]位置
                list.add(people[i][1],people[i]);
            }else{
                //结果集中元素个数小于等于第i个人前面应有的人数时，将第i个人追加到结果集的后面
                list.add(list.size(),people[i]);
            }
        }
        //将list转化为数组，然后返回
        return list.toArray(new int[list.size()][]);
    }
}
```
时间复杂度：O(nlog n + n^2)
空间复杂度：O(n)