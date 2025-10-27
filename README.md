# -En-Python
un simple jeu 
snake-game/
│
├── main.py
├── README.md
├── requirements.txt
└── assets/
    ├── apple.png
    └── snake.png
    pip import pygame
pygame.init()

width, height = 600, 400
window = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game 🐍")install pygame
import random

snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
direction = "RIGHT"
change_to = direction

apple_pos = [random.randrange(1, (width//10)) * 10,
             random.randrange(1, (height//10)) * 10]
apple_spawn = True
import random

snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
direction = "RIGHT"
change_to = direction

apple_pos = [random.randrange(1, (width//10)) * 10,
             random.randrange(1, (height//10)) * 10]
apple_spawn = True
running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_to = "UP"
            if event.key == pygame.K_DOWN:
                change_to = "DOWN"
            if event.key == pygame.K_LEFT:
                change_to = "LEFT"
            if event.key == pygame.K_RIGHT:
                change_to = "RIGHT"

    # Empêche le serpent de faire demi-tour
    if change_to == "UP" and direction != "DOWN":
        direction = "UP"
    if change_to == "DOWN" and direction != "UP":
        direction = "DOWN"
    if change_to == "LEFT" and direction != "RIGHT":
        direction = "LEFT"
    if change_to == "RIGHT" and direction != "LEFT":
        direction = "RIGHT"

    # Déplace le serpent
    if direction == "UP":
        snake_pos[1] -= 10
    if direction == "DOWN":
        snake_pos[1] += 10
    if direction == "LEFT":
        snake_pos[0] -= 10
    if direction == "RIGHT":
        snake_pos[0] += 10

    snake_body.insert(0, list(snake_pos))
    if snake_pos == apple_pos:
        apple_spawn = False
    else:
        snake_body.pop()

    if not apple_spawn:
        apple_pos = [random.randrange(1, (width//10)) * 10,
                     random.randrange(1, (height//10)) * 10]
    apple_spawn = True

    # Fond et dessin
    window.fill((0, 0, 0))
    for pos in snake_body:
        pygame.draw.rect(window, (0, 255, 0), pygame.Rect(pos[0], pos[1], 10, 10))
    pygame.draw.rect(window, (255, 0, 0), pygame.Rect(apple_pos[0], apple_pos[1], 10, 10))

    # Conditions de fin
    if (snake_pos[0] < 0 or snake_pos[0] > width-10 or
        snake_pos[1] < 0 or snake_pos[1] > height-10):
        running = False

    for block in snake_body[1:]:
        if snake_pos == block:
            running = False

    pygame.display.update()
    clock.tick(15)

pygame.quit()
