import pygame
import random


pygame.init()


GRID_WIDTH, GRID_HEIGHT = 15, 15
CELL_SIZE = 40
WINDOW_SIZE = (GRID_WIDTH * CELL_SIZE, GRID_HEIGHT * CELL_SIZE)


WHITE, GREY, BLACK, GREEN, RED, BLUE = (255, 255, 255), (200, 200, 200), (0, 0, 0), (0, 255, 0), (255, 0, 0), (0, 0, 255)

# Initialize Pygame window
screen = pygame.display.set_mode(WINDOW_SIZE)
pygame.display.set_caption("Greedy Best-First Search")


delta = [(-1, 0), (0, -1), (1, 0), (0, 1)]

class Node:
    def __init__(self, x, y, parent=None):
        self.x, self.y, self.parent = x, y, parent
        self.distance = float('inf')  # Initialize distance to infinity

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

def heuristic(a, b):
    return abs(a.x - b.x) + abs(a.y - b.y)

def greedy_best_first_search(start, goal, grid):
    open_set = [start]
    closed_set = []
    start.distance = heuristic(start, goal)

    while open_set:
        current = min(open_set, key=lambda o: o.distance)
        if current == goal:
            path = []
            while current.parent:
                path.append((current.y, current.x))
                current = current.parent
            return path[::-1]  # Reverse path

        open_set.remove(current)
        closed_set.append(current)

        for d in delta:
            x, y = current.x + d[1], current.y + d[0]
            if 0 <= x < GRID_WIDTH and 0 <= y < GRID_HEIGHT and grid[y][x] == 0:
                neighbor = Node(x, y, current)
                if neighbor in closed_set:
                    continue
                if neighbor not in open_set:
                    neighbor.distance = heuristic(neighbor, goal)
                    open_set.append(neighbor)
    return []

def create_grid_and_maze():
    grid = [[0 for _ in range(GRID_WIDTH)] for _ in range(GRID_HEIGHT)]
    obstacles = random.sample(range(GRID_WIDTH * GRID_HEIGHT), k=int(GRID_WIDTH * GRID_HEIGHT * 0.3))
    for obstacle in obstacles:
        x, y = divmod(obstacle, GRID_WIDTH)
        grid[y][x] = 1
    return grid

def draw_everything(grid, start, goal, path):
    screen.fill(WHITE)
    for y in range(GRID_HEIGHT):
        for x in range(GRID_WIDTH):
            color = GREY if grid[y][x] == 1 else WHITE
            pygame.draw.rect(screen, color, (x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE))
            pygame.draw.rect(screen, BLACK, (x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE), 1)
    if start:
        pygame.draw.rect(screen, RED, (start.x * CELL_SIZE, start.y * CELL_SIZE, CELL_SIZE, CELL_SIZE))
    if goal:
        pygame.draw.rect(screen, BLUE, (goal.x * CELL_SIZE, goal.y * CELL_SIZE, CELL_SIZE, CELL_SIZE))
    for (y, x) in path:
        pygame.draw.rect(screen, GREEN, (x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE))
    pygame.display.flip()

def main():
    grid = create_grid_and_maze()
    start = goal = None
    path = []

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False
            elif event.type == pygame.MOUSEBUTTONDOWN:
                x, y = event.pos[0] // CELL_SIZE, event.pos[1] // CELL_SIZE
                if grid[y][x] == 0:  # Ensure click is not on an obstacle
                    if not start:
                        start = Node(x, y)
                    elif not goal:
                        goal = Node(x, y)
                        path = greedy_best_first_search(start, goal, grid)
        draw_everything(grid, start, goal, path)

    pygame.quit()

if __name__ == "__main__":
    main()
