# [LeetCode] 1152. Analyze User Website Visit Pattern (欠)

You are given two string arrays username and website and an integer array timestamp. All the given arrays are of the same length and the tuple [username[i], website[i], timestamp[i]] indicates that the user username[i] visited the website website[i] at time timestamp[i].

A pattern is a list of three websites (not necessarily distinct).

For example, ["home", "away", "love"], ["leetcode", "love", "leetcode"], and ["luffy", "luffy", "luffy"] are all patterns.
The score of a pattern is the number of users that visited all the websites in the pattern in the same order they appeared in the pattern.

For example, if the pattern is ["home", "away", "love"], the score is the number of users x such that x visited "home" then visited "away" and visited "love" after that.
Similarly, if the pattern is ["leetcode", "love", "leetcode"], the score is the number of users x such that x visited "leetcode" then visited "love" and visited "leetcode" one more time after that.
Also, if the pattern is ["luffy", "luffy", "luffy"], the score is the number of users x such that x visited "luffy" three different times at different timestamps.
Return the pattern with the largest score. If there is more than one pattern with the same largest score, return the lexicographically smallest such pattern.

Example 1:

Input: username = ["joe","joe","joe","james","james","james","james","mary","mary","mary"], timestamp = [1,2,3,4,5,6,7,8,9,10], website = ["home","about","career","home","cart","maps","home","home","about","career"]
Output: ["home","about","career"]
Explanation: The tuples in this example are:
["joe","home",1],["joe","about",2],["joe","career",3],["james","home",4],["james","cart",5],["james","maps",6],["james","home",7],["mary","home",8],["mary","about",9], and ["mary","career",10].
The pattern ("home", "about", "career") has score 2 (joe and mary).
The pattern ("home", "cart", "maps") has score 1 (james).
The pattern ("home", "cart", "home") has score 1 (james).
The pattern ("home", "maps", "home") has score 1 (james).
The pattern ("cart", "maps", "home") has score 1 (james).
The pattern ("home", "home", "home") has score 0 (no user visited home 3 times).


## solution 1
```java
class Solution {
    public List<String> mostVisitedPattern(String[] username, int[] timestamp, String[] website) {
        HashMap<String, List<Pair>> map = new HashMap<>();
        int len = username.length;
        // collect the website info for every user, key: username, value: (timestamp, website)
        for (int i = 0; i < len; i++) {
            map.putIfAbsent(username[i], new ArrayList<>());
            map.get(username[i]).add(new Pair(timestamp[i], website[i]));
        }

        // count map
        HashMap<String, Integer> count = new HashMap<>();
        String res = "";
        for (String key : map.keySet()) {
            HashSet<String> set = new HashSet<>();
            List<Pair> list = map.get(key);
            // 时间早的在前，这也符合访问顺序
            Collections.sort(list, (a, b) -> a.time - b.time);
            for (int i = 0; i < list.size(); i++) {
                for (int j = i + 1; j < list.size(); j++) {
                    for (int k = j + 1; k < list.size(); k++) {
                        String str = list.get(i).web + " " + list.get(j).web + " " + list.get(k).web;
                        if (!set.contains(str)) {
                            count.put(str, count.getOrDefault(str, 0) + 1);
                            set.add(str);
                        }
                        if (res.equals("") || count.get(res) < count.get(str) || (count.get(res) == count.get(str) && res.compareTo(str) > 0)) {
                            res = str;
                        }
                    }
                }
            }
        }

        // 再把字符串改回list
        String[] p = res.split(" ");
        List<String> path = new ArrayList<>();
        for (String site : p) {
            path.add(site);
        }
        return path;
    }
}

class Pair {
    int time;
    String web;
    public Pair(int time, String web) {
        this.time = time;
        this.web = web;
    }
}
```

python
```
题目将按时间顺序排列的三次访问称作一种访问模式，不过需要注意的是，这里的访问模式并不要求连续，也就是说对于a->b->c->c这个访问序列而言，可以形成两组a->b->c的访问模式。

由于题目的数据规模很小，因此可以先一趟遍历，将每个用户的访问序列收集起来，接下来三重循环枚举a、b、c，并使用哈希表将所有枚举的访问模式记录下来，最后通过哈希表来找出访问频率最大的模式即可。

在具体实现上，由于题目中的访问过的网站不超过50个，因此可以使用3位的50进制来对访问模式进行编码来进行哈希。
```

```python
class Solution(object):
    def mostVisitedPattern(self, username, timestamp, website):
        """
        :type username: List[str]
        :type timestamp: List[int]
        :type website: List[str]
        :rtype: List[str]
        """

        user2uid = {}
        web2wid = {}
        wid2web = {}

        user_watch = {}

        for user, t, web in zip(username, timestamp, website):
            if user not in user2uid:
                user2uid[user] = len(user2uid)
            uid = user2uid[user]
            
            if uid not in user_watch:
                user_watch[uid] = []
            
            if web not in web2wid:
                web2wid[web] = len(web2wid)
                
            wid = web2wid[web]
            wid2web[wid] = web

            user_watch[uid].append([t, wid])

        freq = {}
        for user in user_watch:
            watch_list = user_watch[user]
            watch_list.sort()
            pattern = set()
            for first in range(len(watch_list) - 2):
                for second in range(first + 1, len(watch_list) - 1):
                    for third in range(second + 1, len(watch_list)):
                        key = watch_list[first][1] * 2500 + watch_list[second][1] * 50 + watch_list[third][1]
                        pattern.add(key)
            
            for key in pattern:
                freq[key] = freq.get(key, 0) + 1
        
        res = None
        max_f = 0

        for key, f in freq.items():
            if f >= max_f:
                first_id = key / 2500
                second_id = key / 50 % 50
                third_id = key % 50
                candidate = [wid2web[first_id], wid2web[second_id], wid2web[third_id]]

                if f > max_f or candidate < res:
                    max_f = f
                    res = candidate[:]
        
        return res
```
## 暴力解法
这个问题首先不难想到暴力解法，我们可以现将[username,timestamp,website]按照这种方式将元素排序（这样就可以保证所有相同姓名的元素按照timestamp排在一起）。然后我们可以暴力枚举相同姓名的人所有访问的website的组合方式，我们可以通过itertools.combinations轻松实现这一点。接着就是建立一个字典，将相同website组合的人添加进去。最后我们只要按照所有website组合的人数去排列，我们选择人数最多的那个website组合即可
————————————————
版权声明：本文为CSDN博主「coordinate_blog」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_17550379/article/details/99209763
```python
class Solution:
    def mostVisitedPattern(self, username: List[str], timestamp: List[int], website: List[str]) -> List[str]:
        data = [[username[i], timestamp[i], website[i]] for i in range(len(username))]
        data.sort()
        d = collections.defaultdict(list)
        for u, t, w in data:
            d[u].append(w)
        
        res = collections.defaultdict(set)
        for i, v in d.items():
            for j in itertools.combinations(v, 3):
                res[j].add(i)

        return sorted(res.items(), key=lambda a:(-len(a[1]), a[0]))[0][0]

```
