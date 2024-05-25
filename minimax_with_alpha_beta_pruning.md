## Minimax Algorithm with Alpha-Beta Pruning
```python
import networkx as nx
import matplotlib.pyplot as plt

# Create a sample binary tree
G = nx.DiGraph()
G.add_edges_from([(1, 2), (1, 3), (2, 4), (2, 5), (3, 6), (3, 7)])

# Define node values
node_values = {1: 5, 2: 3, 3: 8, 4: 1, 5: 2, 6: 6, 7: 9}

# Function to visualize the tree with improved visibility
def visualize_tree(graph, current_node, values, alpha=None, beta=None, player=None):
    pos = nx.spring_layout(graph, seed=48)  # Set seed for reproducibility
    nx.draw(graph, pos, with_labels=False, font_weight='bold', node_size=800, node_color='lightblue')

    # Draw node values with improved visibility
    for node, (x, y) in pos.items():
        label = f"{node}\n({values[node]})"
        if alpha is not None and node in alpha:
            label += f"\nα={alpha[node]}"
        if beta is not None and node in beta:
            label += f"\nβ={beta[node]}"

        plt.text(x, y, label, ha='center', va='center', color='black',
                 bbox=dict(facecolor='white', alpha=0.7, edgecolor='white'))

    # Highlight the current node with a different color
    nx.draw_networkx_nodes(graph, pos, nodelist=[current_node], node_color='red', node_size=700)

    # Show player number
    plt.text(0, 1.2, f"Player: {player}", ha='center', va='center', color='black', fontsize=12)

    plt.show()

# Visualize the tree with improved visibility
visualize_tree(G, 1, node_values)

# Minimax strategy function with alpha-beta pruning
def minimax_strategy(node, depth, maximizing_player, alpha=None, beta=None, route=None, player=1):
    if route is None:
        route = []

    if depth == 0 or G.out_degree(node) == 0:
        return node_values[node], route + [node]

    if maximizing_player:
        print(f"\nPlayer {player}: Evaluating Node {node}, α={alpha}, β={beta}")
        max_eval = float('-inf')
        max_route = None
        for child in G.successors(node):
            eval_child, child_route = minimax_strategy(child, depth - 1, False, alpha, beta, route + [node], player=2)
            if eval_child > max_eval:
                max_eval = eval_child
                max_route = child_route
            if alpha is not None:
                alpha[node] = max_eval
            visualize_tree(G, node, node_values, alpha, beta, player=player)
            if beta is not None and max_eval >= beta.get(node, float('inf')):
                print(f"Pruning at Max Node {node}, α={alpha[node]}, β={beta[node]}")
                break  # Beta cut-off
        return max_eval, max_route if depth == 3 else None
    else:
        print(f"\nPlayer {player}: Evaluating Node {node}, α={alpha}, β={beta}")
        min_eval = float('inf')
        min_route = None
        for child in G.successors(node):
            eval_child, child_route = minimax_strategy(child, depth - 1, True, alpha, beta, route + [node], player=1)
            if eval_child < min_eval:
                min_eval = eval_child
                min_route = child_route
            if beta is not None:
                beta[node] = min_eval
            visualize_tree(G, node, node_values, alpha, beta, player=player)
            if alpha is not None and min_eval <= alpha.get(node, float('-inf')):
                print(f"Pruning at Min Node {node}, α={alpha[node]}, β={beta[node]}")
                break  # Alpha cut-off
        return min_eval, min_route

# Find the optimal move using minimax with alpha-beta pruning
root_node = 1
optimal_strategy, final_route = minimax_strategy(root_node, 3, True, {root_node: float('-inf')}, {root_node: float('inf')})

if optimal_strategy > 0:
    print(f"\nPlayer 1 wins!")
    print(f"Final Route for Player 1: {final_route}")
else:
    print(f"\nPlayer 2 wins!")
    print(f"Final Route for Player 2: {final_route}")

print(f"\nOptimal value for the root node: {optimal_strategy}")
```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/2406dc7a-6b27-473f-b99c-46627c84bca9)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/245890de-a86f-4f26-aca5-c302c7a9d1fd)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/3a88c90a-26f5-4580-980b-08a786996684)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/4e93c88c-2870-4c1d-b083-0a64c158d17a)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/aede3548-7a39-4f26-9dc5-df18c935f48a)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/ad57507c-320f-41ca-a481-e75b80576a52)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/0313c2d0-0080-4ec6-9fce-2c0fd1ab2310)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/4f97c3d8-32e2-4f7e-8d18-9028f8e454b1)
