## Water Jug Problem
```python
import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 600, 400
FPS = 60
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
BLUE = (0, 0, 255)

# Jug capacities
jug1_capacity = 4
jug2_capacity = 3
target_amount = 2

# Initial state
initial_state = (0, 0)

# Create a window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Water Jug Problem")

# Fonts
font = pygame.font.SysFont(None, 30)

def draw_jugs(jug1, jug2, action=None, new_state=None):
    screen.fill(WHITE)

    # Draw Jug 1
    pygame.draw.rect(screen, BLACK, (150, 150, 50, 200))  # Jug outline
    pygame.draw.rect(screen, BLUE, (150, 150, 50, jug1 * 50))  # Water

    # Draw Jug 2
    pygame.draw.rect(screen, BLACK, (400, 150, 50, 200))  # Jug outline
    pygame.draw.rect(screen, BLUE, (400, 150, 50, jug2 * 50))  # Water

    # Text displaying the amount in each jug
    text1 = font.render(f"{jug1}/{jug1_capacity}", True, BLACK)
    text2 = font.render(f"{jug2}/{jug2_capacity}", True, BLACK)
    screen.blit(text1, (175, 125))
    screen.blit(text2, (425, 125))

    # Display action and new state
    if action is not None and new_state is not None:
        action_text = font.render(f"{action}: {new_state[0]}, {new_state[1]}", True, BLACK)
        screen.blit(action_text, (50, 10))

    pygame.display.flip()

def pour_water(state, action):
    jug1, jug2 = state

    if action == "fill_jug1":
        return (jug1_capacity, jug2)
    elif action == "fill_jug2":
        return (jug1, jug2_capacity)
    elif action == "empty_jug1":
        return (0, jug2)
    elif action == "empty_jug2":
        return (jug1, 0)
    elif action == "pour_jug1_to_jug2":
        transfer_amount = min(jug1, jug2_capacity - jug2)
        return (jug1 - transfer_amount, jug2 + transfer_amount)
    elif action == "pour_jug2_to_jug1":
        transfer_amount = min(jug2, jug1_capacity - jug1)
        return (jug1 + transfer_amount, jug2 - transfer_amount)
    else:
        return state

def is_goal_state(state):
    return state[0] == target_amount or state[1] == target_amount

def depth_first_search(state, path=[]):
    if is_goal_state(state):
        return path + [state]

    for action in ["fill_jug1", "fill_jug2", "empty_jug1", "empty_jug2",
                   "pour_jug1_to_jug2", "pour_jug2_to_jug1"]:
        new_state = pour_water(state, action)
        if new_state not in path:
            print(f"{action}: {state} -> {new_state}")
            draw_jugs(*state, action=action, new_state=new_state)
            pygame.time.wait(1000)
            clock = pygame.time.Clock()
            clock.tick(FPS)
            new_path = depth_first_search(new_state, path + [new_state])
            if new_path:
                return new_path

    return None

def main():
    clock = pygame.time.Clock()

    path = depth_first_search(initial_state)
    if path is None:
        print("No solution found.")
        sys.exit()

    for i, state in enumerate(path):
        print(f"Step {i + 1}: {state}")
        draw_jugs(*state)
        pygame.time.wait(1000)
        clock.tick(FPS)

    pygame.quit()
    sys.exit()

if __name__ == "__main__":
    main()

```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/410025c5-3e21-4760-b908-2c527e8502b2)
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/bfdb1a00-eb84-4afb-b70c-d62536c226ab)
