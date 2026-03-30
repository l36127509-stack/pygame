import pygame
import sys
import math
import random

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 1200, 800
FPS = 60
G = 0.5  # Gravitational constant (tweak for feel)

WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
YELLOW = (255, 220, 0)
STAR_COLOR = (255, 240, 100)

screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Solar System Builder - Click & Drag to Create Planets!")
clock = pygame.time.Clock()
font = pygame.font.SysFont("Arial", 24)
small_font = pygame.font.SysFont("Arial", 18)

class Body:
    def __init__(self, x, y, mass, color, radius, is_star=False):
        self.x = x
        self.y = y
        self.vx = 0
        self.vy = 0
        self.mass = mass
        self.color = color
        self.radius = radius
        self.is_star = is_star
        self.trail = []  # For nice orbital trails

    def update_position(self):
        self.x += self.vx
        self.y += self.vy

    def draw(self, surface):
        # Draw trail
        if len(self.trail) > 1:
            pygame.draw.lines(surface, (*self.color, 80), False, self.trail, 2)
        
        # Draw body
        pygame.draw.circle(surface, self.color, (int(self.x), int(self.y)), self.radius)
        
        # Glow for star
        if self.is_star:
            pygame.draw.circle(surface, (*STAR_COLOR, 60), (int(self.x), int(self.y)), self.radius + 12)

    def add_trail(self):
        self.trail.append((int(self.x), int(self.y)))
        if len(self.trail) > 300:  # Limit trail length
            self.trail.pop(0)


def calculate_gravity(bodies):
    for i, body1 in enumerate(bodies):
        fx = fy = 0
        for j, body2 in enumerate(bodies):
            if i == j:
                continue
            dx = body2.x - body1.x
            dy = body2.y - body1.y
            dist_sq = dx*dx + dy*dy
            if dist_sq < 1:
                dist_sq = 1  # Prevent division by zero / extreme forces

            dist = math.sqrt(dist_sq)
            force = G * body1.mass * body2.mass / dist_sq

            fx += force * dx / dist
            fy += force * dy / dist

        body1.vx += fx / body1.mass
        body1.vy += fy / body1.mass


# Game variables
bodies = []
dragging = False`      
drag_start = None
current_body = None

# Create central Sun
sun = Body(WIDTH//2, HEIGHT//2, mass=2000, color=YELLOW, radius=25, is_star=True)
bodies.append(sun)

running = True
show_help = True

while running:
    dt = clock.tick(FPS)
    screen.fill((5, 5, 20))  # Deep space background

    for event in pygame.event.get():
        if
