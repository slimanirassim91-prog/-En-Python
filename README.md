# -snake-jeu
snake-game/
import pygame
import random
import sys

# Initialisation
pygame.init()

# --- Paramètres du jeu ---
WIDTH, HEIGHT = 600, 400
BLOCK_SIZE = 10
FPS = 15

# Couleurs
WHITE = (255, 255, 255)
GREEN = (0, 200, 0)
RED = (200, 0, 0)
BLACK = (0, 0, 0)

# Fenêtre
window = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("🐍 Snake Game")

# Police pour le score
font = pygame.font.SysFont("Arial", 25)


def draw_snake(snake_body):
    """Dessine le serpent"""
    for pos in snake_body:
        pygame.draw.rect(window, GREEN, pygame.Rect(pos[0], pos[1], BLOCK_SIZE, BLOCK_SIZE))


def draw_apple(pos):
    """Dessine la pomme"""
    pygame.draw.rect(window, RED, pygame.Rect(pos[0], pos[1], BLOCK_SIZE, BLOCK_SIZE))


def show_score(score):
    """Affiche le score à l'écran"""
    value = font.render(f"Score : {score}", True, WHITE)
    window.blit(value, [10, 10])


def game_over(score):
    """Écran de fin"""
    over_font = pygame.font.SysFont("Arial", 50)
    message = over_font.render("Game Over", True, RED)
    score_text = font.render(f"Ton score final : {score}", True, WHITE)

    window.fill(BLACK)
    window.blit(message, [WIDTH / 2 - 120, HEIGHT / 2 - 50])
    window.blit(score_text, [WIDTH / 2 - 100, HEIGHT / 2 + 10])
    pygame.display.flip()
    pygame.time.wait(3000)
    pygame.quit()
    sys.exit()


def main():
    # Position et corps du serpent
    snake_pos = [100, 50]
    snake_body = [[100, 50], [90, 50], [80, 50]]

    # Pomme
    apple_pos = [random.randrange(1, (WIDTH // BLOCK_SIZE)) * BLOCK_SIZE,
                 random.randrange(1, (HEIGHT // BLOCK_SIZE)) * BLOCK_SIZE]
    apple_spawn = True

    direction = "RIGHT"
    change_to = direction
    score = 0

    clock = pygame.time.Clock()

    # --- Boucle principale ---
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP and direction != "DOWN":
                    change_to = "UP"
                elif event.key == pygame.K_DOWN and direction != "UP":
                    change_to = "DOWN"
                elif event.key == pygame.K_LEFT and direction != "RIGHT":
                    change_to = "LEFT"
                elif event.key == pygame.K_RIGHT and direction != "LEFT":
                    change_to = "RIGHT"

        direction = change_to

        # Déplacement du serpent
        if direction == "UP":
            snake_pos[1] -= BLOCK_SIZE
        if direction == "DOWN":
            snake_pos[1] += BLOCK_SIZE
        if direction == "LEFT":
            snake_pos[0] -= BLOCK_SIZE
        if direction == "RIGHT":
            snake_pos[0] += BLOCK_SIZE

        # Croissance du serpent
        snake_body.insert(0, list(snake_pos))
        if snake_pos == apple_pos:
            score += 10
            apple_spawn = False
        else:
            snake_body.pop()

        # Nouvelle pomme
        if not apple_spawn:
            apple_pos = [random.randrange(1, (WIDTH // BLOCK_SIZE)) * BLOCK_SIZE,
                         random.randrange(1, (HEIGHT // BLOCK_SIZE)) * BLOCK_SIZE]
            apple_spawn = True

        # --- Collisions ---
        if snake_pos[0] < 0 or snake_pos[0] >= WIDTH or snake_pos[1] < 0 or snake_pos[1] >= HEIGHT:
            game_over(score)

        for block in snake_body[1:]:
            if snake_pos == block:
                game_over(score)

        # --- Affichage ---
        window.fill(BLACK)
        draw_snake(snake_body)
        draw_apple(apple_pos)
        show_score(score)

        pygame.display.update()
        clock.tick(FPS)


if __name__ == "__main__":
    main()
