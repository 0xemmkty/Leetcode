# 818. Race Car (欠)

>Your car starts at position 0 and speed +1 on an infinite number line. Your car can go into negative positions. Your car drives automatically according to a sequence of instructions 'A' (accelerate) and 'R' (reverse):

>When you get an instruction 'A', your car does the following:
position += speed
speed *= 2
When you get an instruction 'R', your car does the following:
If your speed is positive then speed = -1
otherwise speed = 1
Your position stays the same.
For example, after commands "AAR", your car goes to positions 0 --> 1 --> 3 --> 3, and your speed goes to 1 --> 2 --> 4 --> -1.

>Given a target position target, return the length of the shortest sequence of instructions to get there.

>Solution:https://leetcode.com/problems/race-car/discuss/124326/Summary-of-the-BFS-and-DP-solutions-with-intuitive-explanation

## Solutiom 1 （BFS)
```java
public int racecar(int target) {
    Deque<int[]> queue = new LinkedList<>();
    queue.offerLast(new int[] {0, 1}); // starts from position 0 with speed 1
    
    Set<String> visited = new HashSet<>();
    visited.add(0 + " " + 1);
    
    for (int level = 0; !queue.isEmpty(); level++) {
        for(int k = queue.size(); k > 0; k--) {
            int[] cur = queue.pollFirst();  // cur[0] is position; cur[1] is speed
            
            if (cur[0] == target) {
                return level;
            }
            
            int[] nxt = new int[] {cur[0] + cur[1], cur[1] << 1};  // accelerate instruction
            String key = (nxt[0] + " " + nxt[1]);
            
            if (!visited.contains(key) && 0 < nxt[0] && nxt[0] < (target << 1)) {
                queue.offerLast(nxt);
                visited.add(key);
            }
            
            nxt = new int[] {cur[0], cur[1] > 0 ? -1 : 1};  // reverse instruction
            key = (nxt[0] + " " + nxt[1]);
            
            if (!visited.contains(key) && 0 < nxt[0] && nxt[0] < (target << 1)) {
                queue.offerLast(nxt);
                visited.add(key);
            }
        }
    }
    
    return -1;
}
```

## Solution 2(DP:Top-down DP)
```java
public int racecar(int target) {
    int[] dp = new int[target + 1];
    Arrays.fill(dp, 1, dp.length, -1);
    return racecar(target, dp);
}

private int racecar(int i, int[] dp) {
    if (dp[i] >= 0) {
        return dp[i];
    }
    
    dp[i] = Integer.MAX_VALUE;
    
    int m = 1, j = 1;
    
    for (; j < i; j = (1 << ++m) - 1) {
        for (int q = 0, p = 0; p < j; p = (1 << ++q) - 1) {
            dp[i] = Math.min(dp[i],  m + 1 + q + 1 + racecar(i - (j - p), dp));
        }
    }
    
    dp[i] = Math.min(dp[i], m + (i == j ? 0 : 1 + racecar(j - i, dp)));
    
    return dp[i];
}
```

## Solution 3 (DP：Bottom-up DP)
```java
public int racecar(int target) {
    int[] dp = new int[target + 1];
    
    for (int i = 1; i <= target; i++) {
        dp[i] = Integer.MAX_VALUE;
        
        int m = 1, j = 1;
        
        for (; j < i; j = (1 << ++m) - 1) {
            for (int q = 0, p = 0; p < j; p = (1 << ++q) - 1) {
                dp[i] = Math.min(dp[i], m + 1 + q + 1 + dp[i - (j - p)]);
            }
        }
        
        dp[i] = Math.min(dp[i], m + (i == j ? 0 : 1 + dp[j - i]));
    }
    
    return dp[target];
}
```


 