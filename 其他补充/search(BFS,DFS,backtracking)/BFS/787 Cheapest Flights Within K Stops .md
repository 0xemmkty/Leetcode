787. Cheapest Flights Within K Stops
Solved
Medium
Topics
Companies
There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

```python
class Solution(object):
    def findCheapestPrice(self, n, flights, src, dst, K):
        if not flights:
            return -1

    # Create a graph
        graph = defaultdict(list)
        for u, v, w in flights:
            graph[u].append((v, w))

    # Check if there's a path from src to dst using BFS
        visited = set()
        queue = deque([src])
        while queue:
            node = queue.popleft()
            visited.add(node)
            for v, _ in graph[node]:
                if v not in visited:
                    queue.append(v)

        if dst not in visited:
            return -1

    # Min heap to store the minimum cost to reach a node with a certain number of stops
        heap = [(0, src, K + 1)]  # Cost, Node, Stops Remaining

        while heap:
            cost, node, stops = heappop(heap)
            if node == dst:
                return cost
            if stops > 0:
                for v, w in graph[node]:
                    heappush(heap, (cost + w, v, stops - 1))

        return -1

```