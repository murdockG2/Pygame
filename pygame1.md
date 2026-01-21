
```bash
pip install pygame-ce
```

[Pygame Docs](https://pyga.me/)
###### 1. Creating a window

```python
import pygame

# initializing pygame

pygame.init()


SCREEN_WIDTH,SCREEN_HEIGHT = 800,500

screen = pygame.display.set_mode((SCREEN_WIDTH,SCREEN_HEIGHT))

# Setting the application name

pygame.display.set_caption("Hello mamamama")

# creating a while loop

running = True

while running:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            running = False

    # to update continously

    pygame.display.update()

    # pygame.display.flip() [Same thing]

pygame.quit()

```

This is a basic window not a optimized one. Now, We will create a window using the sys module

```python
import pygame,sys

pygame.init()

SCREEN_WIDTH,SCREEN_HEIGHT = 800,500

screen = pygame.display.set_mode((SCREEN_WIDTH,SCREEN_HEIGHT))

pygame.display.set_caption("Hello mamamama")

while True:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            sys.exit()

    pygame.display.flip()
pygame.quit()
```

Creating a window using a class 
```python
import sys

import pygame

  

class AlienInvasion:

    def __init__(self):

        pygame.init()

        self.screen = pygame.display.set_mode((1200,800))

        pygame.display.set_caption("Alien Invasion")

    def run_game(self):

        while True:

            for event in pygame.event.get():

                if event.type == pygame.QUIT:

                    sys.exit()

            pygame.display.flip()

  

if __name__ == '__main__':

    ai = AlienInvasion()

    ai.run_game()

pygame.quit()
```


###### For background color 
```python
screen.fill('red')
```

It will be inside the While loop and screen is the surface which you created with pygame.display
for the color there are many colors to choose, check the docs for that.
[All the pygame colors](https://pyga.me/docs/ref/color_list.html)

###### 2. Display Surface/Graphics

Pygame can display graphics in a 2 ways:
Show an image or text via a surface & draw pixels

Show an image/text (surface)
A surface in pygame is usually an image(png,jpg) a plain area or rendered text

creating a surface:

```python
plain_surface = pygame.Surface((width,height))
```

importing a surface:

```python
imported_surface = pygame.image.load(path)
```

Text Surface:

```python
text_surface = font.render(text,AntiAlias,Color)
```

**Display Surface vs  Surface**

The Display surface is the main surface that we draw on and there can only be one and it is always
visible 

A regular surface is an image of some kind, you can have any number but they only visible when 
attached to the display surface

other than that, they share some attributes and methods. For example, you can fill both with a color via surface.fill(color)


**pygame co-ordinate system**

![[rect_coordinates.png]] 

Basic surface example:

```python
# creating a surface

surf = pygame.Surface((50,50))

surf.fill('red')

while True:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            sys.exit()

    screen.fill('skyblue')

    # putting that surface inside the display surface

    screen.blit(surf,(200,250))

    pygame.display.flip()
```

Basic animation example:

```python
import pygame,sys

pygame.init()

screen = pygame.display.set_mode((1000,500))

pygame.display.set_caption("Amar game")

surf = pygame.Surface((50,50))

surf.fill('red')

# creating x and y co-ordinate for surf

surf_x = 400

surf_y = 0

running = True

while running:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            running = False

    screen.fill('skyblue')

    screen.blit(surf,(surf_x,surf_y))

    # updating the y co-ordinate

    surf_y += 0.1

    if surf_y > 500:

        surf_y = 0

    pygame.display.update()

pygame.quit()
sys.exit()
```

Note: We generally dont use co-ordinate system this way, we have rect for that. We will get to know about it later on.

Importing an image -> always convert of alpha

```python
player_surf = pygame.image.load("player_stand.png").convert_alpha()
```

# Creating the Space Invader game

```python
import pygame,sys

from random import randint

pygame.init()

WINDOW_WIDTH,WINDOW_HEIGHT = 1000,500

screen = pygame.display.set_mode((WINDOW_WIDTH,WINDOW_HEIGHT))

pygame.display.set_caption("Amar game")

  

player_surf = pygame.image.load("images/player.png").convert_alpha()

star_surf = pygame.image.load("images/star.png").convert_alpha()

x = 100

running = True

while running:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            running = False

    screen.fill('skyblue')

    screen.blit(player_surf,(x,200))

    for i in range(20):

        screen.blit(star_surf,(randint(0,WINDOW_WIDTH),randint(0,WINDOW_HEIGHT)))

    x += 0.1

    pygame.display.update()

pygame.quit()

sys.exit()
```

problem with this code is that the stars will come at randomly and recreate again and again. which is a problem

```python
import pygame,sys

from random import randint

pygame.init()

WINDOW_WIDTH,WINDOW_HEIGHT = 1000,500

screen = pygame.display.set_mode((WINDOW_WIDTH,WINDOW_HEIGHT))

pygame.display.set_caption("Amar game")

player_surf = pygame.image.load("images/player.png").convert_alpha()

star_surf = pygame.image.load("images/star.png").convert_alpha()

star_pos = [(randint(0,WINDOW_WIDTH),randint(0,WINDOW_HEIGHT)) for i in range(20)]

x = 100

running = True

while running:

    for event in pygame.event.get():

        if event.type == pygame.QUIT:

            running = False

    screen.fill('darkgray')

    # the changed loop

    for pos in star_pos:

        screen.blit(star_surf,pos)

    x += 0.1

    screen.blit(player_surf,(x,150))

    pygame.display.update()

pygame.quit()

sys.exit()
```

###### Placing surfaces via rects

using rects is more convienient.
because Rects Place surfaces more elegantly, can detect collisions, can be drawn

![[Screenshot 2026-01-21 211543 1.png]]

There are 2 kinds of rects => 1. Rects and FRects(Floating point Rectangle)

They are nearly identical, the only difference is that FRects store data as floating point values while rect use integers
So FRects are more precise and people mostly use FRects

Creating rects in two ways:

You can create a rect from scratch -> 

```python
pygame.Rect(pos,size)
pygame.FRect(pos,size)
```

