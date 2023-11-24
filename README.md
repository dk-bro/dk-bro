import pygame
import sys
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
FPS = 60
WHITE = (255, 255, 255)
RED = (255, 0, 0)

# Player
player_width, player_height = 50, 50
player_x = (WIDTH - player_width) // 2
player_y = HEIGHT - player_height - 10
player_speed = 5

# Bullet
bullet_width, bullet_height = 10, 20
bullet_speed = 7
bullets = []

# Enemy
enemy_width, enemy_height = 50, 50
enemy_speed = 3
enemies = []

# Create the game window
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Shooting Game")

# Clock to control the frame rate
clock = pygame.time.Clock()

# Function to draw the player
def draw_player(x, y):
    pygame.draw.rect(screen, WHITE, [x, y, player_width, player_height])

# Function to draw the bullets
def draw_bullets(bullets):
    for bullet in bullets:
        pygame.draw.rect(screen, WHITE, [bullet[0], bullet[1], bullet_width, bullet_height])

# Function to draw the enemies
def draw_enemies(enemies):
    for enemy in enemies:
        pygame.draw.rect(screen, RED, [enemy[0], enemy[1], enemy_width, enemy_height])

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Move the player
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and player_x > 0:
        player_x -= player_speed
    if keys[pygame.K_RIGHT] and player_x < WIDTH - player_width:
        player_x += player_speed

    # Shoot bullets
    if keys[pygame.K_SPACE]:
        bullets.append([player_x + (player_width - bullet_width) // 2, player_y])

    # Move bullets
    bullets = [[bx, by - bullet_speed] for bx, by in bullets]

    # Spawn enemies
    if random.randint(1, 100) < 5:
        enemy_x = random.randint(0, WIDTH - enemy_width)
        enemy_y = -enemy_height
        enemies.append([enemy_x, enemy_y])

    # Move enemies
    enemies = [[ex, ey + enemy_speed] for ex, ey in enemies]

    # Check for collisions
    for bullet in bullets:
        for enemy in enemies:
            if (
                bullet[0] < enemy[0] + enemy_width
                and bullet[0] + bullet_width > enemy[0]
                and bullet[1] < enemy[1] + enemy_height
                and bullet[1] + bullet_height > enemy[1]
            ):
                bullets.remove(bullet)
                enemies.remove(enemy)

    # Update the display
    screen.fill((0, 0, 0))
    draw_player(player_x, player_y)
    draw_bullets(bullets)
    draw_enemies(enemies)

    # Update the display
    pygame.display.flip()

    # Cap the frame rate
    clock.tick(FPS)

