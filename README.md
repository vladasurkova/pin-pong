from pygame import *


display.set_caption("pingpong")
window = display.set_mode((700, 500))
background = transform.scale(image.load("galaxy.jpg"), (700,500))

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
while game:
    # window.blit(background,(0, 0))


    for e in event.get():
        if e.type == QUIT:
            game = False

    if finish != True: 
        window.blit(background, (0,0))
        #text =  font2.render('Счёт:' + str(score), 1, (255, 255, 255))
        # window.blit(text, (10, 20))
        # text_lose =  font2.render('Пропущено:' + str(lost), 1, (255, 255, 255))
        # window.blit(text_lose, (10, 50))
        
        ball.reset()
        ball.update()
        left_r.reset()
        right_r.reset()
        left_r.update_r()
        right_r.update_l()
    display.update()
    
