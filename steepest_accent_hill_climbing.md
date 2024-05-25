## Steepest Accent Hill Climbing
```python
import tkinter as tk
import matplotlib
from tkinter import ttk
import matplotlib
matplotlib.use("TkAgg")  # Explicitly set the TkAgg backend
import matplotlib.pyplot as plt

import matplotlib.pyplot as plt
import matplotlib.backends
from mpl_toolkits.mplot3d import Axes3D
import random

# Function to calculate the objective value (you can replace this with your own function)
def objective_function(x, y):
    return -x**2 - y**2

# Steepest Ascent Hill Climbing function
def steepest_ascent_hill_climbing(max_iterations, step_size):
    x = random.uniform(-5, 5)
    y = random.uniform(-5, 5)
    path = [(x, y, objective_function(x, y))]  # Store the optimization path
    
    for _ in range(max_iterations):
        current_value = objective_function(x, y)
        best_neighbor_x = x
        best_neighbor_y = y
        best_neighbor_value = current_value
        
        for neighbor_x, neighbor_y in [(x - step_size, y), (x + step_size, y), (x, y - step_size), (x, y + step_size)]:
            neighbor_value = objective_function(neighbor_x, neighbor_y)
            
            if neighbor_value > best_neighbor_value:
                best_neighbor_x = neighbor_x
                best_neighbor_y = neighbor_y
                best_neighbor_value = neighbor_value
        
        if best_neighbor_value <= current_value:
            break  # If no improvement, stop
            
        x = best_neighbor_x
        y = best_neighbor_y
        path.append((x, y, best_neighbor_value))  # Store the new position and value
    
    return path

# Function to update the 3D plot
def update_3d_plot():
    global iteration, path
    if iteration < max_iterations:
        path = steepest_ascent_hill_climbing(iteration + 1, step_size)
        x_values, y_values, z_values = zip(*path)
        ax.clear()
        ax.plot(x_values, y_values, z_values, marker='o', linestyle='-')
        ax.set_xlabel('X')
        ax.set_ylabel('Y')
        ax.set_zlabel('Objective Value')
        ax.set_title('Steepest Ascent Hill Climbing')
        canvas.draw()

# Function to start optimization
def start_optimization():
    global iteration
    if iteration < max_iterations:
        iteration += 1
        update_3d_plot()

# Create the main window
root = tk.Tk()
root.title("Steepest Ascent Hill Climbing")

# Create a 3D Matplotlib plot
fig = plt.figure(figsize=(8, 6))
ax = fig.add_subplot(111, projection='3d')
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Objective Value')
ax.set_title('Steepest Ascent Hill Climbing')

# Create a Matplotlib canvas for the plot
canvas = matplotlib.backends.backend_tkagg.FigureCanvasTkAgg(fig, master=root)
canvas_widget = canvas.get_tk_widget()
canvas_widget.pack()

# Create a button to start optimization
start_button = ttk.Button(root, text="Start Optimization", command=start_optimization)
start_button.pack()

# Parameters
max_iterations = 100
step_size = 0.1
iteration = 0
path = []

# Run the Tkinter main loop
root.mainloop()
```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/38e2796d-1db5-4c80-aa66-b7c2256ec894)
