#05. PyGameStudy
--------------------------------
####1. 네임드 튜플 사용
####2. pygame.sprite.Sprite와 Rect의 충돌 처리
-------------------------------


	import pygame
	import collections
	
	
	class SpriteSheet(object):
	    sprite_sheet = None
	
	    def __init__(self, file_name):
	        self.sprite_sheet = pygame.image.load(file_name).convert()
	
	    def get_image(self, x, y, width, height):
	        image = pygame.Surface([width, height]).convert()
	        image.blit(self.sprite_sheet, (0, 0), (x, y, width, height))
	
	        image.set_colorkey((0, 184, 64))
	        return image
	
	
	class Block(pygame.sprite.Sprite):
	    def __init__(self):
	        pygame.sprite.Sprite.__init__(self)
	
	        sprite_sheet = SpriteSheet("block.png")
	        self.image = sprite_sheet.get_image(0, 0, 70, 70);
	        self.rect = self.image.get_rect()
	
	
	class Player(pygame.sprite.Sprite):
	    change_x = 0
	    walking_frames_r = []
	    walking_frames_l = []
	    direction = 'R'
	
	    def __init__(self):
	        super(Player, self).__init__()
	
	        sprite_sheet = SpriteSheet("hero.png")
	
	        frame_width = 30
	        frame_height = 40
	        for x in (0, 40, 80):
	            image = sprite_sheet.get_image(x, 90, frame_width, frame_height)
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
	
	
	def collideBlock(player, block_list, vb):
	    # drawblock vs player collide
	    if vb.rect.colliderect(player.rect):
	        if player.change_x > 0:
	            player.rect.right = vb.rect.left
	        elif player.change_x < 0:
	            player.rect.left = vb.rect.right
	
	    # spriteblock vs player collide
	    block_hit_list = pygame.sprite.spritecollide(player, block_list, False)
	    for hit_block in block_hit_list:
	        if player.change_x > 0:
	            player.rect.right = hit_block.rect.left
	        elif player.change_x < 0:
	            player.rect.left = hit_block.rect.right
	
	
	def rendering(screen, active_sprite_list, block_list, virtual_block ) :
	     # 1. clear screen
	    screen.fill((100, 100, 255))
	
	    # 2. draw player, block
	    active_sprite_list.draw(screen)
	    block_list.draw(screen)
	
	    # 3. draw red rect
	    virtual_block.surface.fill((255, 0, 0))
	    screen.blit(virtual_block.surface, virtual_block.rect)
	
	
	def main():
	    pygame.init()
	
	    screen = pygame.display.set_mode((300, 240))
	    pygame.display.set_caption("player animating")
	
	    player = Player()
	
	    block = Block()
	    block.rect.x = 50
	    block.rect.y = 170
	
	    player.rect.x = 150
	    player.rect.y = 240 - player.rect.height
	
	    active_sprite_list = pygame.sprite.Group()
	    active_sprite_list.add(player)
	
	    block_list = pygame.sprite.Group()
	    block_list.add(block)
	
	    virtual_block = collections.namedtuple('virtual_block', 'surface rect')
	    vb = virtual_block(pygame.Surface([10, 50]), pygame.Rect(230, 200, 10, 50))
	
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
	        block_list.update()
	
	        collideBlock(player, block_list, vb )
	
	        rendering(screen, active_sprite_list, block_list, vb)
	
	        clock.tick(60)
	        pygame.display.flip()
	
	    pygame.quit()
	
	if __name__ == "__main__":
	    main()

##결과 화면

###출력
![](https://github.com/RealChaser/PygameStudy/blob/master/image/rect_collide.png?raw=true)