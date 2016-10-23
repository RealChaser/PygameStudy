#00. PyGameStudy
--------------------------------
####1. pygame 초기화 방법
####2. displaySurface 사용하기
####3. 이미지 사운드 폰트 로드 및 사용법
####4. 이벤트 처리


	import sys
	import pygame
	from pygame.locals import *
	
	GREEN = (0, 255, 0)	
	WHITE = (255, 255, 255)
	
	
	def main():
	
	    # 초기화
	    pygame.init()
	    pygame.display.set_caption('baseStudy00')
	
	    icon = pygame.image.load('gameicon.png')
	    pygame.display.set_icon(icon)
	
	    fps_clock = pygame.time.Clock()
	
	    display_surface = pygame.display.set_mode((400, 300))
	
	    # 폰트 생성
	    freesansbold_font = pygame.font.Font('freesansbold.ttf', 32)
	
	    # 글씨 생성
	    text_surf = freesansbold_font.render('test show string', True, GREEN)
	    text_rect = text_surf.get_rect()
	    text_rect.center = (200, 150)
	
	    # 이미지 로드
	    player_image_left = pygame.image.load('squirrel.png')
	    player_image_right = pygame.transform.flip(player_image_left, True, False) # 좌우 반전
	
	    # 이미지 이동 관련 변수
	    posx = 0
	    step = 3
	
	    while True:
	        # 화면 클리어
	        display_surface.fill(WHITE)
	
	        # 글씨 렌더링
	        display_surface.blit(text_surf, text_rect)
	
	        # 이미지 렌더링
	        display_surface.blit(player_image_left, (posx, 10))
	        display_surface.blit(player_image_right, (posx, 110))
	
	        # 이미지 이동을 위한 update 로직
	        posx += step
	        if not 0 < posx < 200:
	            step = -step
	
	        # 이벤트 처리 루프
	        for event in pygame.event.get():
	            if event.type == QUIT:
	                pygame.quit()
	                sys.exit()
	
	        pygame.display.update()
	        fps_clock.tick(60)
	
	
	if __name__ == '__main__':
	    main()

	'''
	    # 일회용 짧은 소리 로드
	    short_sound = pygame.mixer.Sound('beep1.wav')
	    short_sound.play()
	    short_sound.stop()
	
	    # 배경음악용 소리 로드
	    pygame.mixer.music.load('backgroundmusic.mp3')
	    pygame.mixer.music.play(-1, 0)
	    pygame.mixer.music.stop()
	'''


