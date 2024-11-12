import pygame
import sys

# Initialize pygame
pygame.init()

# Game settings
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
FPS = 60
PLAYER_SPEED = 5

# Colors
WHITE = (255, 255, 255)
GREEN = (0, 255, 0)

# Set up the display
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Open World Game")

# Load player character
player_size = 50
player_color = GREEN
player_pos = [SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2]

# World boundary limits (for a larger world than the screen)
world_width = 2000
world_height = 2000
camera_pos = [0, 0]  # Camera offset to create a scrolling effect

# Game loop
clock = pygame.time.Clock()

while True:
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_pos[0] > 0:
        player_pos[0] -= PLAYER_SPEED
    if keys[pygame.K_RIGHT] and player_pos[0] < world_width - player_size:
        player_pos[0] += PLAYER_SPEED
    if keys[pygame.K_UP] and player_pos[1] > 0:
        player_pos[1] -= PLAYER_SPEED
    if keys[pygame.K_DOWN] and player_pos[1] < world_height - player_size:
        player_pos[1] += PLAYER_SPEED

    # Update camera position to follow player
    camera_pos[0] = player_pos[0] - SCREEN_WIDTH // 2
    camera_pos[1] = player_pos[1] - SCREEN_HEIGHT // 2

    # Limit camera boundaries
    camera_pos[0] = max(0, min(camera_pos[0], world_width - SCREEN_WIDTH))
    camera_pos[1] = max(0, min(camera_pos[1], world_height - SCREEN_HEIGHT))

    # Fill background
    screen.fill(WHITE)

    # Draw the player relative to the camera position
    pygame.draw.rect(
        screen,
        player_color,
        (player_pos[0] - camera_pos[0], player_pos[1] - camera_pos[1], player_size, player_size)
    )

    # Update display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)
