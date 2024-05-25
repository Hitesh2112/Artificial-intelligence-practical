## Implement Breadth first Search or Breadth first traversal of a graph or tree data structure using Queue 

```python
from collections import deque
def bfs(graph, start):
    visited = set()
    queue = deque([start])
    while queue:
        node = queue.popleft()
        if node not in visited:
            visited.add(node)
            print(node, end=' ')
        for neighbor in graph[node]:
            if neighbor not in visited:
                queue.append(neighbor)
graph = {
'A': ['B', 'C'],
'B': ['A', 'D', 'E'],
'C': ['A', 'F', 'G'],
'D': ['B'],
'E': ['B'],
'F': ['C'],
'G': ['C']
}
start_node = 'C'
print("BFS traversal:")
bfs(graph, start_node)

```

### Output
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/3de26d02-ae80-42a3-ad7e-976984d29f18)
