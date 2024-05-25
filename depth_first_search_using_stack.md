## Implement Depth first Search or Depth first traversal of a graph or tree data structure using Stack 
```python
def dfs(graph, start):
    visited = set()
    stack = [start]
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            print(node, end=' ')
        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)
graph = {
'A': ['B', 'C'],
'B': ['A', 'D', 'E'],
'C': ['A', 'F', 'G'],
'D': ['B'],
'E': ['B'],
'F': ['C'],
'G': ['C']
}
start_node = 'B'
print("DFS traversal:")
dfs(graph, start_node)

```
### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/c3167d03-7231-4066-ae4f-0c88217bb1f2)
