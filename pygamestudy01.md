#01. PyGameStudy
--------------------------------
#### 키보드 입력 처리



	
	import sys
	import random
	import pygame
	from pygame.locals import *
	
	GREEN = (0, 255, 0)
	WHITE = (255, 255, 255)
	MOVE_RATE = 3
	WIN_WIDTH = 600
	WIN_HEIGHT = 480
	HALF_WIN_WIDTH = WIN_WIDTH / 2
	HALF_WIN_HEIGHT = WIN_HEIGHT / 2
	
	px = 250
	py = 250
	
	gx = 200
	gy = 200
	
	def main():
	    global px, py, gx, gy
	    # 초기화
	    pygame.init()
	    pygame.display.set_caption('baseStudy01')
	
	    icon = pygame.image.load('gameicon.png')
	    pygame.display.set_icon(icon)
	
	    fps_clock = pygame.time.Clock()
	
	    display_surface = pygame.display.set_mode((WIN_WIDTH, WIN_HEIGHT))
	
	    # 이미지 로드
	    player_image = pygame.image.load('squirrel.png')
	    player_image = pygame.transform.scale(player_image, (20, 20))
	    grass_image = pygame.image.load('grass1.png')
	
	    # 이미지 이동 관련 변수
	
	    prev_px = px
	    prev_py = py
	    cx = px - HALF_WIN_WIDTH
	    cy = py - HALF_WIN_HEIGHT
	
	    move_up = False
	    move_down = False
	    move_left = False
	    move_right = False
	
	    while True:
	        # 화면 클리어
	        display_surface.fill(WHITE)
	
	        if prev_px != px or prev_py != py:
	            cx = px - HALF_WIN_WIDTH
	            cy = py - HALF_WIN_HEIGHT
	
	            prev_px = px
	            prev_py = py
	
	        display_surface.blit(grass_image, (gx - cx, gy - cy))
	
	        # 이미지 렌더링
	        display_surface.blit(player_image, Rect(HALF_WIN_WIDTH, HALF_WIN_HEIGHT, 30, 30))
	
	
	        # 이벤트 처리 루프
	        for event in pygame.event.get():
	            if event.type == QUIT:
	                pygame.quit()
	                sys.exit()
	            elif event.type == KEYDOWN:
	                if event.key in (K_UP, K_w):
	                    move_down = False
	                    move_up = True
	                if event.key in (K_DOWN, K_s):
	                    move_down = True
	                    move_up = False
	                if event.key in (K_LEFT, K_a):
	                    move_right = False
	                    move_left = True
	                if event.key in (K_RIGHT, K_d):
	                    move_right = True
	                    move_left = False
	            elif event.type == KEYUP:
	                if event.key in (K_UP, K_w):
	                    move_up = False
	                if event.key in (K_DOWN, K_s):
	                    move_down = False
	                if event.key in (K_LEFT, K_a):
	                    move_left = False
	                if event.key in (K_RIGHT, K_d):
	                    move_right = False
	
	        if move_up:
	            py -= MOVE_RATE
	        if move_down:
	            py += MOVE_RATE
	        if move_right:
	            px += MOVE_RATE
	        if move_left:
	            px -= MOVE_RATE
	
	        pygame.display.update()
	        fps_clock.tick(60)
	
	
	if __name__ == '__main__':
	    main()
