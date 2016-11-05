#03. PyGameStudy
--------------------------------
#### convert_alpha()와 convert() 함수 차이


	import pygame, sys
	
	pygame.init()
	window = pygame.display.set_mode((400, 200))
	background = pygame.Surface((window.get_size()))
	background.fill((255, 0, 0))
	image = image2 = pygame.image.load('image/alpha_image.png')
	
	image = image.convert()
	rect = image.get_rect()
	
	image2 = image2.convert_alpha()
	rect2 = image2.get_rect()
	
	rect2.left = 200
	
	i = 0
	while True:
	  for event in pygame.event.get():
	    if event.type == 12:
	      pygame.quit()
	      sys.exit()
	
	  image.set_alpha(i)
	  image2.set_alpha(i)
	
	  window.fill((255, 255, 255))
	  window.blit(background, background.get_rect())
	  window.blit(image, rect)
	  window.blit(image2, rect2)
	
	  if i == 255:
	    i = 0
	  else:
	    i += 1
	
	  pygame.display.update()
	  pygame.time.Clock().tick(60)

##결과 화면

###원본
![](https://github.com/RealChaser/PygameStudy/blob/master/image/alpha_image.png?raw=true)

###알파 : 0   
![](https://github.com/RealChaser/PygameStudy/blob/master/image/pygame_alpha_0.png?raw=true)

###알파 : 155   
![](https://github.com/RealChaser/PygameStudy/blob/master/image/pygame_alpha_155.png?raw=true)

###알파 : 255  
![](https://github.com/RealChaser/PygameStudy/blob/master/image/pygame_alpha_255.png?raw=true)