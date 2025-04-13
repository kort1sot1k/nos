#создание игры (шутер)
from pygame import *
from random import randint
from time import time as timer

#window
win_width = 700
win_height = 500
display.set_caption("Shooter")
window = display.set_mode((win_width, win_height))

#music space
mixer.init()
mixer.music.load('space.ogg')

#music fire
mixer.music.play()
fire_sound = mixer.Sound('fire.ogg')

#picture
img_back = "galaxy.jpg" 
img_bullet = "bullet.png" 
img_hero = "rocket.png" 
img_enemy = "ufo.png" 
img_ast = "asteroid.png" 

#class gamesprite
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_speed):
        sprite.Sprite.__init__(self)
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

#class player
class Player(GameSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_LEFT] and self.rect.x > 5:
            self.rect.x -= self.speed
        if keys[K_RIGHT] and self.rect.x < win_width - 80:
            self.rect.x += self.speed

#class enemy
class Enemy(GameSprite):
    def update(self):
        self.rect.y += self.speed
        global lost
        if self.rect.y > win_height:
            self.rect.x = randint(80, win_width - 80)
            self.rect.y = 0
            lost = lost + 1


#class bullets
class Bullet(GameSprite):
    def update(self):
        self.rect.y += self.speed
        if self.rect.y < 0:
            self.kill()

#fire
def fire(self):
    bullet = Bullet(img_bullet, self.rect.centerx, self.rect.top, 15, 20, -15)
    bullets.add(bullet)

#the event of clicking on the "Close" button
    for e in event.get():
        if e.type == QUIT:
            run = False
        elif e.type == KEYDOWN:
            if e.key == K_SPACE:
                if num_fire < 5 and rel_time == False:
                    num_fire = num_fire + 1
                    fire_sound.play()
                    ship.fire()
                    
                    if num_fire  >= 5 and rel_time == False : 
                        last_time = timer() 
                        rel_time = True 

#movment
ship.update()
monsters.update()
asteroids.update()
bullets.update()

#проигрыш и поражение
sprite_list = sprite.groupcollide(
    monster, bullet, True, True
)

sprite_list = sprite.groupcollide(
    ship, bullet, False
)

#points
if score >= goal:
    finish = True
    window.blit(win, (200, 200))

display.update()
