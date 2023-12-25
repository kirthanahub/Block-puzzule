import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
PLAYER_SIZE = 50
BLOCK_SIZE = 50
FPS = 60
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Create the window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Game")

# Clock to control the frame rate
clock = pygame.time.Clock()

# Player
player = pygame.Rect(WIDTH // 2 - PLAYER_SIZE // 2, HEIGHT - PLAYER_SIZE - 10, PLAYER_SIZE, PLAYER_SIZE)
player_speed = 5

# Blocks
blocks = []

def create_block():
    block_x = random.randint(0, WIDTH - BLOCK_SIZE)
    block_y = -BLOCK_SIZE
    return pygame.Rect(block_x, block_y, BLOCK_SIZE, BLOCK_SIZE)

def draw_blocks():
    for block in blocks:
        pygame.draw.rect(screen, RED, block)

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    player.x += (keys[pygame.K_RIGHT] - keys[pygame.K_LEFT]) * player_speed

    # Update blocks
    blocks = [block for block in blocks if block.y < HEIGHT]
    blocks.append(create_block())

    # Check for collisions
    if any(block.colliderect(player) for block in blocks):
        print("Game Over!")
        pygame.quit()
        sys.exit()

    # Draw everything
    screen.fill(WHITE)
    pygame.draw.rect(screen, WHITE, player)
    draw_blocks()

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)
