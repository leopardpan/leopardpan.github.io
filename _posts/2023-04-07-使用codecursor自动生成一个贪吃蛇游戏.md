---
layout: post
title: 使用codecursor自动生成一个贪吃蛇游戏
date: 2023-04-07 
description: "使用codecursor自动生成一个贪吃蛇游戏"
tag: Python
---


```python
# Here is a basic implementation of the classic game Snake using Python and Pygame library

# Importing necessary libraries
import pygame
import time
import random
 
# Initializing Pygame
pygame.init()
 
# Defining colors
white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)
 
# Setting display size
dis_width = 600
dis_height = 400
 
# Creating display
dis = pygame.display.set_mode((dis_width, dis_height))
pygame.display.set_caption('Snake Game')
 
# Setting clock
clock = pygame.time.Clock()
 
# Setting snake block size
snake_block = 10
 
# Setting font style
font_style = pygame.font.SysFont(None, 30)
 
 
def our_snake(snake_block, snake_List):
    """
    Function to create snake
    """
    for x in snake_List:
        pygame.draw.rect(dis, black, [x[0], x[1], snake_block, snake_block])
 
 
def message(msg, color):
    """
    Function to display message
    """
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 6, dis_height / 3])
 
 
def gameLoop():
    """
    Main game function
    """
    game_over = False
    game_close = False
 
    # Setting starting position of snake
    x1 = dis_width / 2
    y1 = dis_height / 2
 
    # Setting change in position of snake
    x1_change = 0
    y1_change = 0
 
    # Creating initial snake
    snake_List = []
    Length_of_snake = 1
 
    # Generating food at random position
    foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
 
    # Main game loop
    while not game_over:
 
        # If game is over
        while game_close == True:
            dis.fill(blue)
            message("You Lost", red)
            pygame.display.update()
 
            # Asking user to play again or quit
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()
 
        # Changing position of snake
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0
 
        # If snake hits the wall
        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True
 
        # Changing position of snake
        x1 += x1_change
        y1 += y1_change
        dis.fill(blue)
        pygame.draw.rect(dis, green, [foodx, foody, snake_block, snake_block])
 
        # Updating snake
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]
 
        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True
 
        our_snake(snake_block, snake_List)
        pygame.draw.rect(dis, green, [foodx, foody, snake_block, snake_block])
        pygame.display.update()

        # If snake hits the food
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1
        clock.tick(20)

    # Deactivating Pygame library
    pygame.quit()

    # Quitting program
    quit()


if __name__ == '__main__':
    gameLoop()
```