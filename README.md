import pygame
import random

# Farben
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
BLUE = (0, 0, 255)
GREEN = (0, 255, 0)
BROWN = (139, 69, 19)

# Bildschirmgröße
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Blöcke
BLOCK_SIZE = 30

class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([BLOCK_SIZE, BLOCK_SIZE])
        self.image.fill(BLUE)
        self.rect = self.image.get_rect()
        self.rect.x = SCREEN_WIDTH // 2
        self.rect.y = SCREEN_HEIGHT // 2

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.rect.x -= 5
        elif keys[pygame.K_RIGHT]:
            self.rect.x += 5
        elif keys[pygame.K_UP]:
            self.rect.y -= 5
        elif keys[pygame.K_DOWN]:
            self.rect.y += 5

class Block(pygame.sprite.Sprite):
    def __init__(self, color, width, height):
        super().__init__()
        self.image = pygame.Surface([width, height])
        self.image.fill(color)
        self.rect = self.image.get_rect()

pygame.init()

screen = pygame.display.set_mode([SCREEN_WIDTH, SCREEN_HEIGHT])
pygame.display.set_caption("BlockCraft")

all_sprites_list = pygame.sprite.Group()
block_list = pygame.sprite.Group()

for i in range(50):
    block = Block(BROWN, BLOCK_SIZE, BLOCK_SIZE)
    block.rect.x = random.randrange(SCREEN_WIDTH - BLOCK_SIZE)
    block.rect.y = random.randrange(SCREEN_HEIGHT - BLOCK_SIZE)
    block_list.add(block)
    all_sprites_list.add(block)

player = Player()
all_sprites_list.add(player)

clock = pygame.time.Clock()

running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    player.update()

    screen.fill(WHITE)

    blocks_hit_list = pygame.sprite.spritecollide(player, block_list, True)

    all_sprites_list.draw(screen)

    pygame.display.flip()

    clock.tick(60)

pygame.quit()

