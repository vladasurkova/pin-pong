from pygame import *


display.set_caption("pingpong")
window = display.set_mode((700, 500))
background = transform.scale(image.load("galaxy.jpg"), (700,500))
font.init()

font1 = font.Font(None, 35)
lose1 = font1.render('PLAYER 1 LOSER!', True, (180, 0, 0))
font2 = font.Font(None, 35)
lose2 = font2.render('PLAYER 2 LOSER!', True, (180, 0, 0))


class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (size_x, size_y) )
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
        self.direction = "right"


    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < 400:
            self.rect.y += self.speed
    def update_l(self):
        keys = key.get_pressed()
        if keys[K_s] and self.rect.y < 400 :
            self.rect.y += self.speed
        if keys[K_w] and self.rect.y > 5 :
            self.rect.y -= self.speed
    
left_r = Player('rokenka.jpg', 5, 100, 100, 100, 5)
right_r = Player('rokenka.jpg', 600, 100, 100, 100, 5)
ball = GameSprite('ball.jpg', 250, 180, 100, 100, 5)
    
finish = False
clock = time.Clock()
game = True

speed_x = 3

speed_y = 3
   


while game:
    # window.blit(background,(0, 0))


    for e in event.get():
        if e.type == QUIT:
            game = False

    if finish != True: 
        window.blit(background, (0,0))
        if ball.rect.x > 650:
            finish = True
            window.blit(lose1, (250, 100))
        if ball.rect.x < 0:
            finish = True
            window.blit(lose2, (250, 100))
        #text =  font2.render('Счёт:' + str(score), 1, (255, 255, 255))
        # window.blit(text, (10, 20))
        # text_lose =  font2.render('Пропущено:' + str(lost), 1, (255, 255, 255))
        # window.blit(text_lose, (10, 50))
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        if ball.rect.y > 400 or ball.rect.y < 0:
            speed_y *= -1
        if sprite.collide_rect(right_r, ball) or sprite.collide_rect(left_r, ball):
           speed_x *= -1
        ball.reset()
        ball.update()
        left_r.reset()
        right_r.reset()
        left_r.update_r()
        right_r.update_l()
    display.update()
    clock.tick()
