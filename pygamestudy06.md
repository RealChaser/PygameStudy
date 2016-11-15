#05. PyGameStudy
--------------------------------
####1. 맵 상하좌우 이동 스크롤
####2. 플레이어 맵 사이즈에 맞춰 이동 제한
-------------------------------

	import pygame
	
	WIN_WIDTH = 300
	WIN_HALF_WIDTH = WIN_WIDTH/2
	WIN_HEIGHT = 240
	WIN_HALF_HEIGHT = WIN_HEIGHT/2
	
	g_camera = pygame.Rect(0, 0, WIN_WIDTH, WIN_HEIGHT)
	g_move_x = 0
	g_move_y = 0
	MAP_SIZE = 512
	
	strUp = 'up'
	strDown = 'down'
	strLeft = 'left'
	strRight = 'right'
	
	
	class SpriteSheet(object):
	    sprite_sheet = None
	
	    def __init__(self, file_name):
	        self.sprite_sheet = pygame.image.load(file_name).convert()
	
	    def get_image(self, x, y, width, height):
	        image = pygame.Surface([width, height]).convert()
	        image.blit(self.sprite_sheet, (0, 0), (x, y, width, height))
	        image.set_colorkey((0, 184, 64))
	        return image
	
	
	class BackGroundMap(pygame.sprite.Sprite):
	    def __init__(self):
	        super().__init__()
	        sprite_sheet = SpriteSheet("map.png")
	        self.image = sprite_sheet.get_image(0, 0, MAP_SIZE, MAP_SIZE);
	        self.rect = self.image.get_rect()
	        self.rect.x = -((MAP_SIZE / 2) - WIN_HALF_WIDTH)
	        self.rect.y = -((MAP_SIZE / 2) - WIN_HALF_HEIGHT)
	
	    def update(self):
	        self.rect.x -= g_move_x
	        self.rect.y -= g_move_y
	
	
	class Player(pygame.sprite.Sprite):
	    def __init__(self):
	        super().__init__()
	        sprite_sheet = SpriteSheet("hero.png")
	        self.walking_frames = {strUp: [], strDown: [], strLeft: [], strRight: []}
	
	        for x in (0, 40, 80):
	            for y, direct in ((0, strDown), (45, strUp), (90, strRight), (135, strLeft)):
	                image = sprite_sheet.get_image(x, y, 40, 40)
	                self.walking_frames[direct].append(image)
	
	        self.image = self.walking_frames[strRight][0]
	        self.move = {'x': 0, 'y': 0, 'direction': strRight} # 애니메이션 프레임 진행을 위해
	        self.rect = self.image.get_rect()
	        self.rect.x, self.rect.y = self.get_center_pos()    # 윈도우 창에서 보일 실제 위치
	
	    def get_center_pos(self):
	        # 플레이어 화면 중앙 위치 (이미지 너비, 높이 고려)
	        return [WIN_HALF_WIDTH - (self.rect.width / 2), WIN_HALF_HEIGHT - (self.rect.height / 2)]
	
	    def update(self):
	        # 플레이어 걷기 프레임 변경을 위한 가상 위치
	        self.move['x'] += g_move_x
	        self.move['y'] += g_move_y
	
	        # 이동거리만큼 프레임 결정
	        if g_move_x != 0:
	            frame = (self.move['x'] // 10) % len(self.walking_frames[self.move['direction']])
	            self.image = self.walking_frames[self.move['direction']][frame]
	        elif g_move_y != 0:
	            frame = (self.move['y'] // 10) % len(self.walking_frames[self.move['direction']])
	            self.image = self.walking_frames[self.move['direction']][frame]
	
	
	def map_move_scroll_limit(player, background):
	    if background.rect.x + background.rect.w < WIN_WIDTH or (WIN_HALF_WIDTH - player.rect.x) < 0:  # 우측 제한
	        background.rect.x = -(background.rect.w - WIN_WIDTH)
	
	    if 0 < background.rect.x or (WIN_HALF_WIDTH - (player.rect.x + player.rect.w/2)) > 0:  # 좌측 제한
	        background.rect.x = 0
	
	    if 0 < background.rect.y or (WIN_HALF_HEIGHT - (player.rect.y + player.rect.h/2)) > 0:  # 위 제한
	        background.rect.y = 0
	
	    if background.rect.y + background.rect.h < WIN_HEIGHT or (WIN_HALF_HEIGHT - player.rect.y) < 0:  # 아래 제한
	        background.rect.y = -(background.rect.h - WIN_HEIGHT)
	
	
	def player_move_map_limit(player, background):
	    if WIN_WIDTH == background.rect.x + background.rect.w and player.rect.x + player.rect.w <= WIN_WIDTH:
	        player.rect.x += g_move_x;
	        if player.rect.x > WIN_WIDTH - player.rect.w:
	            player.rect.x = WIN_WIDTH - player.rect.w
	
	    if background.rect.x == 0 and player.rect.x >= 0:
	        player.rect.x += g_move_x
	        if player.rect.x < 0:
	            player.rect.x = 0
	
	    if background.rect.y == 0 and player.rect.y >= 0:
	        player.rect.y += g_move_y
	        if player.rect.y < 0:
	            player.rect.y = 0
	
	    if WIN_HEIGHT == background.rect.y + background.rect.h and player.rect.y + player.rect.h <= WIN_HEIGHT:
	        player.rect.y += g_move_y
	        if player.rect.y > WIN_HEIGHT - player.rect.h:
	            player.rect.y = WIN_HEIGHT - player.rect.h
	
	
	def move(player, background):
	    # 맵 사이드 끝 이동 제한
	    map_move_scroll_limit(player, background)
	
	    # 플레이어 맵 끝에서 이동 제한
	    player_move_map_limit(player, background)
	
	
	def main():
	    global g_move_x, g_move_y
	    pygame.init()
	
	    screen = pygame.display.set_mode((WIN_WIDTH, WIN_HEIGHT))
	    pygame.display.set_caption("map move")
	
	    background = BackGroundMap()
	    map_sprite_list = pygame.sprite.Group()
	    map_sprite_list.add(background)
	
	    player = Player()
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
	                    player.move['direction'] = strLeft
	                    g_move_x = -3
	                if event.key == pygame.K_RIGHT:
	                    player.move['direction'] = strRight
	                    g_move_x = 3
	                if event.key == pygame.K_UP:
	                    player.move['direction'] = strUp
	                    g_move_y = -3
	                if event.key == pygame.K_DOWN:
	                    player.move['direction'] = strDown
	                    g_move_y = 3
	
	            if event.type == pygame.KEYUP:
	                g_move_x = 0
	                g_move_y = 0
	
	        map_sprite_list.update()
	        active_sprite_list.update()
	
	        move(player, background)
	
	        map_sprite_list.draw(screen)
	        active_sprite_list.draw(screen)
	
	        clock.tick(60)
	        pygame.display.flip()
	
	    pygame.quit()
	
	if __name__ == "__main__":
	    main()

##결과 화면

###출력
![](https://github.com/RealChaser/PygameStudy/blob/master/image/move_scroll.png?raw=true)