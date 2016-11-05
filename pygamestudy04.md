#04. PyGameStudy
--------------------------------
1. 플레이어 좌우 이동에 맞춰 프레임 변경
2. set_colorkey(color) : 스프라이트 배경색 지우기

-------------------------------
	import pygame
	
	
	class SpriteSheet(object):
	    sprite_sheet = None
	
	    def __init__(self, file_name):
	        self.sprite_sheet = pygame.image.load(file_name).convert()
	
	    def get_image(self, x, y, width, height):
	        image = pygame.Surface([width, height]).convert()
	        image.blit(self.sprite_sheet, (0, 0), (x, y, width, height))
	
	        image.set_colorkey((0, 184, 64))
	        return image
	
	
	class Player(pygame.sprite.Sprite):
	    change_x = 0
	    walking_frames_r = []
	    walking_frames_l = []
	    direction = 'R'
	
	    def __init__(self):
	        super(Player, self).__init__()
	
	        sprite_sheet = SpriteSheet("hero.png")
	
	        frame_size = 40
	        for x in (0, 40, 80):
	            image = sprite_sheet.get_image(x, 90, frame_size, frame_size)
	            self.walking_frames_r.append(image)
	
	            image = pygame.transform.flip(image, True, False)
	            self.walking_frames_l.append(image)
	
	        self.image = self.walking_frames_r[0]
	        self.rect = self.image.get_rect()
	
	    def update(self):
	        self.rect.x += self.change_x
	        if self.direction == "R":
	            frame = (self.rect.x // 10) % len(self.walking_frames_r)
	            self.image = self.walking_frames_r[frame]
	        else:
	            frame = (self.rect.x // 10) % len(self.walking_frames_l)
	            self.image = self.walking_frames_l[frame]
	
	    def go_left(self):
	        self.change_x = -3
	        self.direction = "L"
	
	    def go_right(self):
	        self.change_x = 3
	        self.direction = "R"
	
	    def stop(self):
	        self.change_x = 0
	
	
	def main():
	    pygame.init()
	
	    screen = pygame.display.set_mode((300, 240))
	    pygame.display.set_caption("player animating")
	
	    player = Player()
	    player.rect.x = 150
	    player.rect.y = 240 - player.rect.height
	
	    active_sprite_list = pygame.sprite.Group()
	    active_sprite_list.add(player)
	
	    done = False
	    clock = pygame.time.Clock()
	
	    while not done:
	        for event in pygame.event.get():
	            if event.type == pygame.QUIT:
	                done = True
	
	            if event.type == pygame.KEYDOWN:
	                if event.key == pygame.K_LEFT:
	                    player.go_left()
	                if event.key == pygame.K_RIGHT:
	                    player.go_right()
	
	            if event.type == pygame.KEYUP:
	                if event.key == pygame.K_LEFT and player.change_x < 0:
	                    player.stop()
	                if event.key == pygame.K_RIGHT and player.change_x > 0:
	                    player.stop()
	
	        active_sprite_list.update()
	
	        screen.fill((100, 100, 255))
	        active_sprite_list.draw(screen)
	
	        clock.tick(60)
	        pygame.display.flip()
	
	    pygame.quit()
	
	if __name__ == "__main__":
	    main()


##결과 화면

###스프라이트 이미지
![](https://github.com/RealChaser/PygameStudy/blob/master/image/hero.png?raw=true)

###출력
![](https://github.com/RealChaser/PygameStudy/blob/master/image/move_animation.png?raw=true)