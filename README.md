import pygame
import random

# تهيئة Pygame
pygame.init()

# إعدادات الشاشة
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))

# الألوان
white = (255, 255, 255)
black = (0, 0, 0)
green = (0, 255, 0)
red = (255, 0, 0)

# إعدادات الثعبان
snake_pos = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50]]
snake_direction = 'RIGHT'
change_to = snake_direction

# إعدادات السرعة
speed = 15
clock = pygame.time.Clock()

# إعدادات الخط
font = pygame.font.SysFont('times new roman', 35)

# النقاط
score = 0

# انتهاء اللعبة
def game_over():
    my_font = pygame.font.SysFont('times new roman', 50)
    GOsurf = my_font.render('Your Score is : ' + str(score), True, red)
    GOrect = GOsurf.get_rect()
    GOrect.midtop = (screen_width / 2, screen_height / 4)
    screen.fill(white)
    screen.blit(GOsurf, GOrect)
    pygame.display.flip()
    pygame.time.sleep(2)
    pygame.quit()
    quit()

# الوظيفة الرئيسية
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_to = 'UP'
            elif event.key == pygame.K_DOWN:
                change_to = 'DOWN'
            elif event.key == pygame.K_LEFT:
                change_to = 'LEFT'
            elif event.key == pygame.K_RIGHT:
                change_to = 'RIGHT'

    if change_to == 'UP' and not snake_direction == 'DOWN':
        snake_direction = 'UP'
    if change_to == 'DOWN' and not snake_direction == 'UP':
        snake_direction = 'DOWN'
    if change_to == 'LEFT' and not snake_direction == 'RIGHT':
        snake_direction = 'LEFT'
    if change_to == 'RIGHT' and not snake_direction == 'LEFT':
        snake_direction = 'RIGHT'

    if snake_direction == 'UP':
        snake_pos[1] -= 10
    if snake_direction == 'DOWN':
        snake_pos[1] += 10
    if snake_direction == 'LEFT':
        snake_pos[0] -= 10
    if snake_direction == 'RIGHT':
        snake_pos[0] += 10

    snake_body.insert(0, list(snake_pos))
    if snake_pos[0] < 0 or snake_pos[0] > screen_width-10 or snake_pos[1] < 0 or snake_pos[1] > screen_height-10:
        game_over()
    
    screen.fill(white)

    pygame.draw.rect(screen, green, pygame.Rect(300, 100, 50, 50))
    pygame.draw.rect(screen, red, pygame.Rect(400, 100, 50, 50))

    for pos in snake_body:
        pygame.draw.rect(screen, black, pygame.Rect(pos[0], pos[1], 10, 10))

    pygame.display.update()

    clock.tick(speed)
