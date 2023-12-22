735. Asteroid Collision
简单 没意思。
需要注意的是，碰撞的条件
[-1,-2,1,2]是不会碰撞的 因为分别向左和向右 碰撞条件不是简单的正负数相抵
Medium
7.4K
879
Companies
We are given an array asteroids of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.
```python
class Solution(object):
    def asteroidCollision(self, asteroids):
        """
        :type asteroids: List[int]
        :rtype: List[int]
        """
        res = []
        for num in asteroids:
            while res and num < 0 < res[-1]:
                if abs(num) > res[-1]:
                    # Current asteroid destroys the last one in res
                    res.pop()
                    continue
                elif abs(num) == res[-1]:
                    # Both asteroids destroy each other
                    res.pop()
                    break
                else:
                    # Current asteroid is smaller and gets destroyed
                    break
            else:
                # No collision, append the asteroid
                res.append(num)

        return res

```

