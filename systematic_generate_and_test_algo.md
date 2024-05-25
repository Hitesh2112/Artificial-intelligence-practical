## To implement Systematic Generate and Test
```python
import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 400, 400
TARGET = 40
GENERATE_RANGE = 100

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Initialize Pygame window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Systematic Generate and Test Algorithm with Heuristic")

# Function to draw text on the screen
def draw_text(text, font, color, x, y):
    text_surface = font.render(text, True, color)
    text_rect = text_surface.get_rect()
    text_rect.center = (x, y)
    screen.blit(text_surface, text_rect)

# Heuristic function
def heuristic(candidate):
    return abs(candidate - TARGET)

# Main loop
def main():
    clock = pygame.time.Clock()
    font = pygame.font.Font(None, 36)

    found = False
    solution = None

    min_heuristic = float('inf')  # Initialize to positive infinity

    for candidate in range(1, GENERATE_RANGE + 1):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()

        # Calculate heuristic value
        candidate_heuristic = heuristic(candidate)

        # Test the solution
        if candidate_heuristic < 5:
            found = True
            solution = candidate
            break

        # Update the minimum heuristic value found so far
        if candidate_heuristic < min_heuristic:
            min_heuristic = candidate_heuristic

        # Draw the current candidate, its heuristic value, and the target on the screen
        screen.fill(WHITE)
        draw_text(f"Current Candidate: {candidate}", font, BLACK, WIDTH // 2, HEIGHT // 2 - 50)
        draw_text(f"Heuristic Value: {candidate_heuristic}", font, RED, WIDTH // 2, HEIGHT // 2)
        draw_text(f"Target: {TARGET}", font, RED, WIDTH // 2, HEIGHT // 2 + 50)
        draw_text(f"Min Heuristic So Far: {min_heuristic}", font, RED, WIDTH // 2, HEIGHT // 2 + 100)

        pygame.display.flip()
        clock.tick(5)  # Adjust the speed of the animation

    # Display the final solution
    screen.fill(WHITE)
    draw_text(f"Solution Found: {solution}", font, BLACK, WIDTH // 2, HEIGHT // 2)
    pygame.display.flip()

    pygame.time.delay(2000)  # Display the solution for 2 seconds before quitting
    pygame.quit()
    sys.exit()

if __name__ == "__main__":
    main()
```

### OUTPUT
![image](https://github.com/Hitesh2112/Artificial-intelligence-practical/assets/97521900/49df8fe3-52b6-41ba-88f1-0d0503b038da)
