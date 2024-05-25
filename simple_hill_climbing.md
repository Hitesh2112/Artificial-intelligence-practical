## Simple Hill Climbing
```python
# Simple Hill Climbing
import pygame
import numpy as np

# Initialize Pygame
pygame.init()

# Set up display
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Hill Climbing Animation (Maximization)")

# Objective function to optimize (maximize)
def objective_function(x):
    return -((x - 3) * (x + 3) * (x - 7) * (x + 7) * np.sin(x))

# Hill climbing algorithm for maximization
def hill_climbing_maximization(starting_point, step_size, num_iterations):
    current_point = starting_point
    history = [current_point]

    for _ in range(num_iterations):
        new_point = current_point + step_size
        if objective_function(new_point) > objective_function(current_point):
            current_point = new_point
        else:
            step_size *= -0.5  # Reverse direction and reduce step size

        history.append(current_point)

    return np.array(history)

# Set up initial parameters
starting_point = np.random.uniform(0, 10)
step_size = 0.5
num_iterations = 50

# Run hill climbing algorithm for maximization
history = hill_climbing_maximization(starting_point, step_size, num_iterations)

# Set up colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Main loop
running = True
frame = 0

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    screen.fill(WHITE)

    # Draw the objective function curve
    x = np.linspace(-10, 5, 1000)
    y = objective_function(x)
    scaled_x = (x + 10) * (width / 20)  # Scale x to fit the screen
    scaled_y = height/2 - (y * (height / (2 * max(y))))  # Scale y to fit the screen
    pygame.draw.lines(screen, RED, False, list(zip(scaled_x.astype(int), scaled_y.astype(int))), 2)

    # Draw the current point
    current_x = (history[frame] + 10) * (width / 20)
    current_y = height/2 - (objective_function(history[frame]) * (height / (2 * max(y))))
    pygame.draw.circle(screen, RED, (int(current_x), int(current_y)), 5)

    pygame.display.flip()
    pygame.time.delay(100)  # Delay for 1 second (1000 milliseconds)
    frame += 1

    if frame >= len(history):
        running = False

pygame.quit()
```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/ee7f5b91-9a82-4805-825a-9cd844131f56)
