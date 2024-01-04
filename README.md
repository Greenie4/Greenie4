import pygame
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
FPS = 60

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Bouncing Ball Game")

# Fonts
font = pygame.font.Font(None, 36)

# Variables
ball_speed = 5
score = 0
balls = []
num_balls = 1

# Function to create new balls
def create_ball():
    ball = {
        "x": random.randint(20, WIDTH - 20),
        "y": random.randint(20, HEIGHT - 20),
        "dx": random.choice([-1, 1]) * ball_speed,
        "dy": random.choice([-1, 1]) * ball_speed,
        "color": (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255)),
    }
    balls.append(ball)

# Function to update ball position and check collisions
def update_balls():
    global score
    for ball in balls:
        ball["x"] += ball["dx"]
        ball["y"] += ball["dy"]
        
        if ball["x"] <= 0 or ball["x"] >= WIDTH:
            ball["dx"] *= -1
            score += 1
            
        if ball["y"] <= 0 or ball["y"] >= HEIGHT:
            ball["dy"] *= -1
            score += 1

# Game loop
running = True
clock = pygame.time.Clock()

while running:
    screen.fill(BLACK)
    
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                num_balls += 1
                create_ball()
            elif event.key == pygame.K_UP:
                ball_speed += 1
    
    # Update logic
    update_balls()
    
    # Display balls
    for ball in balls:
        pygame.draw.circle(screen, ball["color"], (ball["x"], ball["y"]), 20)
    
    # Display score
    score_text = font.render(f"Score: {score}", True, WHITE)
    screen.blit(score_text, (10, 10))
    
    # Display number of balls
    balls_text = font.render(f"Balls: {num_balls}", True, WHITE)
    screen.blit(balls_text, (10, 50))
    
    pygame.display.flip()
    clock.tick(FPS)

pygame.quit()
