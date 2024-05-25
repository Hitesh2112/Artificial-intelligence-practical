## Minimax Algorithm
```python
import networkx as nx
import matplotlib.pyplot as plt

# Create a sample binary tree
G = nx.DiGraph()
G.add_edges_from([(1, 2), (1, 3), (2, 4), (2, 5), (3, 6), (3, 7)])

# Define node values
node_values = {1: 5, 2: 6, 3: 8, 4: 9, 5: 2, 6: 5, 7: 4}  # for player 1 win and player 2 lose
# node_values = {1: 5, 2: 1, 3: 2, 4: -1, 5: -2, 6: -3, 7: -4} # for player 1 lose and player 2 win
# node_values = {1: 5, 2: 1, 3: 2, 4: 0, 5: 2, 6: 0, 7: 4} # for draw

# Function to visualize the tree
def visualize_tree(graph, current_node, values, strategy=None, outcome=None):
    pos = nx.spring_layout(graph, seed=68)  # Set seed for reproducibility
    nx.draw(graph, pos, with_labels=False, font_weight='bold', node_size=700, node_color='lightblue')

    # Draw node values
    for node, (x, y) in pos.items():
        plt.text(x, y, f"{node}\n({values[node]})", ha='center', va='center', color='black',
                 bbox=dict(facecolor='white', alpha=0.7, edgecolor='white'))

    # Highlight the current node with a different color
    nx.draw_networkx_nodes(graph, pos, nodelist=[current_node], node_color='red', node_size=700)

    # Show strategy and outcome
    plt.text(0, 1.2, f"Strategy: {strategy}", ha='center', va='center', color='black', fontsize=12)
    plt.text(0, 1, f"Outcome for {strategy}: {outcome}", ha='center', va='center', color='black', fontsize=12)
    plt.show()
visualize_tree(G, 1, node_values)   # Visualize tree

# Minimax strategy function without alpha-beta pruning for a two-player game
def minimax_strategy(node, depth, player, route=None):
    if route is None:
        route = []
    if depth == 0 or G.out_degree(node) == 0:
        return node_values[node], route + [node]

    if player == 1:  # Max player
        print(f"\nPlayer 1: Evaluating Node {node}")
        max_eval = float('-inf')
        max_route = None
        for child in G.successors(node):
            eval_child, child_route = minimax_strategy(child, depth - 1, 2, route + [node])
            if eval_child > max_eval:
                max_eval = eval_child
                max_route = child_route
        print(f"Max Value for Node {node}: {max_eval}")
        outcome = "Win" if max_eval > 0 else "Draw" if max_eval == 0 else "Lose"
        visualize_tree(G, node, node_values, strategy="Player 1 (Max)", outcome=outcome)
        return max_eval, max_route if depth == 3 else None
    else:  # player == 2 (Min player)
        print(f"\nPlayer 2: Evaluating Node {node}")
        min_eval = float('inf')
        min_route = None
        for child in G.successors(node):
            eval_child, child_route = minimax_strategy(child, depth - 1, 1, route + [node])
            if eval_child < min_eval:
                min_eval = eval_child
                min_route = child_route
        print(f"Min Value for Node {node}: {min_eval}")
        outcome = "Lose" if min_eval > 0 else "Draw" if min_eval == 0 else "Win"
        visualize_tree(G, node, node_values, strategy="Player 2 (Min)", outcome=outcome)
        return min_eval, min_route

# Find the optimal move using minimax without alpha-beta pruning for a two-player game
root_node = 1
optimal_strategy, final_route = minimax_strategy(root_node, 3, 1)

if optimal_strategy > 0:
    print(f"\nPlayer 1 wins!")
    print(f"Final Route for Player 1: {final_route}")
else:
    print(f"\nPlayer 2 wins!")
    print(f"Final Route for Player 2: {final_route}")
print(f"\nOptimal value for the root node: {optimal_strategy}")
```
### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/6ff013fd-8517-4078-bb78-578b5c08dfeb)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/9e839c2e-84aa-4d71-9beb-6d45aaaee60b)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/4983e861-d4e9-42f0-a245-e89c60949bad)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/b181102c-dea4-4d6a-947a-cd148e4a0783)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/ea9d3c20-8d2a-4938-9c5f-2ce1b5d6e8b5)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/9daf4e8c-3b3f-4904-b29d-35bdd1f01887)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/d4f4c9a1-2094-4b3a-9a87-c5a9919cc1c8)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/045bccec-7650-4d35-8f96-9a3dd93b6107)
