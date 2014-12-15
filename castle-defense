# -*- coding: utf-8 -*-
"""
003_static_blit_pretty.py
static blitting and drawing (pretty version)
url: http://thepythongamebook.com/en:part2:pygame:step003
author: horst.jens@spielend-programmieren.at
licence: gpl, see http://www.gnu.org/licenses/gpl.html

works with pyhton3.4 and python2.7

Blitting a surface on a static position
Drawing a filled circle into ballsurface.
Blitting this surface once.
introducing pygame draw methods
The ball's rectangular surface is black because the background
color of the ball's surface was never defined nor filled."""


import pygame 
import math
import random



class PygView(object):

    screenx = 1024
    screeny = 640
    def __init__(self, width=screenx, height=screeny, fps=30):
        """Initialize pygame, window, background, font,...
           default arguments 
        """
        PygView.screenx = width
        PygView.screeny = height
        pygame.init()
        pygame.display.set_caption("Press ESC to quit")
        self.width = width
        self.height = height
        self.screen = pygame.display.set_mode((self.width, self.height), pygame.DOUBLEBUF)
        self.background = pygame.Surface(self.screen.get_size()).convert()  
        self.background.fill((255,255,255)) # fill background white
        self.clock = pygame.time.Clock()
        self.fps = fps
        self.playtime = 0.0
        self.font = pygame.font.SysFont('mono', 24, bold=True)
        
        self.cannongroup=pygame.sprite.Group()
        self.allgroup=pygame.sprite.Group()
        self.archergroup=pygame.sprite.Group()
        self.crosshairgroup=pygame.sprite.Group()
        self.cannonballgroup=pygame.sprite.Group()
        self.bargroup=pygame.sprite.Group()
        self.monstergroup=pygame.sprite.Group()
        
        
        Cannon.groups=self.cannongroup, self.allgroup
        Archer.groups=self.archergroup, self.allgroup
        Crosshair.groups=self.crosshairgroup, self.allgroup 
        Cannonball.groups=self.cannonballgroup, self.allgroup
        Reloadingbar.groups=self.bargroup,self.allgroup
        Hitpointbar.groups=self.bargroup,self.allgroup
        Monster.groups=self.monstergroup,self.allgroup
        
        Cannon(750,240,180)
        Cannon(750,440,180)
        Cannon(950,440,180)
        Cannon(950,240,180)
        
        self.archer1=Archer(500,100)
        
        self.cross1=Crosshair()     

    def paint(self):
        """painting on the surface"""
        #------- try out some pygame draw functions --------
        # pygame.draw.rect(Surface, color, Rect, width=0): return Rect
        # burg
        pygame.draw.rect(self.background, (0,255,0), (750,240,200,200)) # rect: (x1, y1, width, height)
        # pygame.draw.circle(Surface, color, pos, radius, width=0): return Rect
        # turm
        pygame.draw.circle(self.background, (0,200,0), (750,240), 50) # turm links oben
        pygame.draw.circle(self.background, (0,200,0), (750,440), 50) # turm links oben
        pygame.draw.circle(self.background, (0,200,0), (950,440), 50) # turm links oben
        pygame.draw.circle(self.background, (0,200,0), (950,240), 50) # turm links oben
        # pygame.draw.arc(Surface, color, Rect, start_angle, stop_angle, width=1): return Rect
        # mauer
        pygame.draw.arc(self.background, (0,150,0),(600,0,150,600), math.pi/2,math.pi*1.5,10) 
        # wasser
        pygame.draw.arc(self.background, (5,66,156),(570,-20,150,620), math.pi/2,math.pi*1.5,20) 
        # ------------------- blitting a Ball --------------
        # pygame.draw.polygon(Surface, color, pointlist, width=0): return Rect
        pygame.draw.polygon(self.background, (0,180,0), ((500,20),
                                                         (400,100),
                                                         (500,180),
                                                         (420,100),
                                                         (500,20)
                                                         ))
        pygame.draw.polygon(self.background, (0,180,0), ((500,220),
                                                         (400,300),
                                                         (500,380),
                                                         (420,300),
                                                         (500,220
                                                         )
                                                         ))   
        pygame.draw.polygon(self.background, (0,180,0), ((500,390),
                                                         (400,470),
                                                         (500,550),
                                                         (420,470),
                                                         (500,390
                                                         )
                                                         ))                                                                                                                    
                                                                 
        
        
        
        #myball = Ball() # creating the Ball object
        #myball.blit(self.background) # blitting it

    def run(self):
        """The mainloop
        """
        self.paint() 
        running = True
        while running:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    running = False 
                elif event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_ESCAPE:
                        running = False
            # mouse button pressed ?
            if pygame.mouse.get_pressed()[0]:
                for cannon in self.cannongroup:
                    cannon.shoot(
                                 pygame.mouse.get_pos()[0],
                                 pygame.mouse.get_pos()[1])
            
            pressedkeys = pygame.key.get_pressed()
            if pressedkeys[pygame.K_r]:
                for cannon in self.cannongroup:
                    pygame.draw.circle(self.screen,(0,244,45),
                                      (cannon.x,cannon.y),
                                      cannon.maxrange,1)
                    pygame.draw.circle(self.screen,(134,39,124),
                                      (cannon.x,cannon.y),
                                      cannon.minrange,1)
                                      
            if random.random()<0.01:
                Monster(0,random.randint(0,PygView.screeny))                          
                
            
            milliseconds = self.clock.tick(self.fps)
            seconds = milliseconds/1000.0
            self.playtime += seconds
            self.playtime += milliseconds / 1000.0
            self.draw_text("FPS: {:6.3}{}PLAYTIME: {:6.3} SECONDS".format(
                           self.clock.get_fps(), " "*5, self.playtime))

            
            #cannonball vs Monster
            for ball in self.cannonballgroup:
                crashgroup = pygame.sprite.spritecollide(ball, self.monstergroup,
                                                          False)
                for crashmonster in crashgroup:
                    print("teste:", ball.z, ball.killzone)
                    # kugel tief genug?
                    if ball.z < ball.killzone:
                       crashmonster.hitpoints -= 50 
            
            
            pygame.display.flip()
            self.screen.blit(self.background, (0, 0))
            
            self.allgroup.update(seconds)
            self.allgroup.draw(self.screen)
            
        pygame.quit()


    def draw_text(self, text):
        """Center text in window
        """
        fw, fh = self.font.size(text)
        surface = self.font.render(text, True, (0, 0, 0))
        self.screen.blit(surface, (50,150))
        
class Monster(pygame.sprite.Sprite):
    
    def __init__(self,x,y,winkel=0):
        pygame.sprite.Sprite.__init__(self,self.groups)
        self.image=pygame.Surface((100,100))
        self.image.set_colorkey((0, 0, 0))
        #body
        pygame.draw.circle(self.image,(33, 55, 33),(50,50),50)
        #head 
        pygame.draw.circle(self.image,(0, 35, 0),(45,50),20)
        #left arm
        pygame.draw.rect(self.image,(113, 9, 9),(50,20,50,10))
        # right arm
        pygame.draw.rect(self.image,(113, 9, 9),(50,80,50,10))
        # left eye
        pygame.draw.rect(self.image,(255,0,8),(50,40,10.10,5))
        pygame.draw.rect(self.image,(255,0,8),(50,60,10.10,5))
        #self.image = pygame.transform.rotate(self.image,-90)
        self.image.convert_alpha()
        self.rect = self.image.get_rect()
        self.dx = random.randint(10,15)
        self.dy = 0
        #self.maxspeed = 1.5
        self.x = x
        self.y = y
        self.rect.centerx = x
        self.rect.centery = y
        self.hitpoints=random.randint(50,80)*1.0
        self.maxhitpoints=100.0
        Hitpointbar(self)
     
    def update(self, seconds):
         self.x += self.dx * seconds
         self.y += self.dy * seconds
         self.rect.centerx = round(self.x,0)
         self.rect.centery = round(self.y,0)
         #self.dx*=self.maxspeed
         #self.dy*=self.maxspeed
         if self.hitpoints<0:
             self.kill()	
             
            
            
      
         
         
    
  
                   

class Archer (pygame.sprite.Sprite):
    def __init__(self,x,y):
        pygame.sprite.Sprite. __init__(self,self.groups)
        self.image=pygame.Surface((100,100))
        self.image.fill((1, 2, 3))
        self.image.set_colorkey((1,2,3))
        pygame.draw.circle(self.image,(16,67,29),(50,50),40)
        pygame.draw.circle(self.image,(20,124,123),(50,50),30)
        pygame.draw.circle(self.image,(1,200,50),(50,50),15)
        pygame.draw.rect(self.image,(20,180,123),(20,22,50,10))
        pygame.draw.rect(self.image,(20,180,123),(20,65,50,10))
        pygame.draw.arc(self.image,(18,11,11),(0,10,150,150),math.pi,20,5)
        pygame.draw.circle(self.image,(95,13,3),(45,45),5)
        pygame.draw.circle(self.image,(95,13,3),(45,55),5)
        
        
        self.rect=self.image.get_rect()
        self.x=x
        self.y=y
        self.rect.centerx=self.x
        self.rect.centery=self.y
        
        
    def update(self,seconds):
        self.rect.centerx=self.x
        self.rect.centery=self.y
        
        
class Crosshair(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite. __init__(self,self.groups) # !!!!!!!!!
        self.image=pygame.Surface((100,100))
        pygame.draw.circle(self.image,(255,0,0),(50,50),50,1)
        pygame.draw.line(self.image,(255,0,0),(50,0),(50,100))
        pygame.draw.line(self.image,(255,0,0),(0,50),(100,50))
        self.image.set_colorkey((0,0,0))
        self.image=self.image.convert_alpha()
        self.rect = self.image.get_rect()
        self.rect.center = pygame.mouse.get_pos()
        
    def update(self,seconds):
        self.rect.center=pygame.mouse.get_pos()
        
        #print(self.rect.center)
        #self.rect.centerx=self.x
        #self.rect.centery=self.y
        
class Cannonball(pygame.sprite.Sprite):
     def __init__(self, x,y,targetx,targety):
         pygame.sprite.Sprite.__init__(self,self.groups) #!!!!!!!!!!
         self.image0=pygame.Surface((20,20))
         pygame.draw.circle(self.image0,(100,83,81),(10,10),10)
         self.image0.set_colorkey((0,0,0))
         self.image0=self.image0.convert_alpha()
         self.rect = self.image0.get_rect()
         self.image=self.image0.copy()
         self.gravity=-9.81
         self.x=x
         self.y=y
         self.z=0
         self.dx=0
         self.dy=0
         self.dz=0.7
         self.killzone = 300.0 # Höhe, ab wann kugel monster tötet
         self.maxspeed = 10.5
         self.targetx = targetx
         self.targety = targety
         distancex = self.targetx - self.x
         distancey = self.targety - self.y
         distance = ( distancex**2 + distancey**2)**0.5
         try:
             self.dz = 10.3* ((self.gravity*-1)/(2*self.maxspeed**2 )) * distance
             #self.dz=0.7
         except:
             print("division by zero?")
         self.dx = distancex / distance
         self.dy = distancey / distance
         self.dx *= self.maxspeed
         self.dy *= self.maxspeed  
         
     def update(self, seconds):
         self.x=self.x + self.dx
         self.y=self.y + self.dy
         self.z=self.z + self.dz
         if self.x < 0:
             self.kill()
         if self.y < 0:
             self.kill()
         if self.x> PygView.screenx:
             self.kill()
         if self.y> PygView.screeny:
             self.kill()     
         if self.z < 0:
             self.kill()             
         self.image=pygame.transform.rotozoom(self.image0, 0, 1+self.z/1000.0)   
         self.rect = self.image.get_rect()
         self.rect.centerx = self.x
         self.rect.centery = self.y
         self.dz+=self.gravity
         
         
     
class Reloadingbar(pygame.sprite.Sprite):
        """shows a bar with the hitpoints of a Bird sprite
           with a given bossnumber, the Lifebar class can 
           identify the boos (Bird sprite) with this codeline:
           Bird.birds[bossnumber] """
        def __init__(self, boss):
            #self.groups = allgroup
            self.boss = boss
            #self._layer = self.boss._layer
            pygame.sprite.Sprite.__init__(self, self.groups)
            self.oldpercent = 0
            self.paint()
            
        def paint(self):
            self.image = pygame.Surface((self.boss.rect.width,7))
            self.image.set_colorkey((0,0,0)) # black transparent
            pygame.draw.rect(self.image, (1,1,1), (0,0,self.boss.rect.width,7),1)
            self.rect = self.image.get_rect()
 
        def update(self, time):
            self.percent = self.boss.reloading / self.boss.reloadtime * 1.0
            if self.percent != self.oldpercent:
                self.paint() # important ! boss.rect.width may have changed (because rotating)
                pygame.draw.rect(self.image, (0,0,0), (1,1,self.boss.rect.width-2,5)) # fill black
                pygame.draw.rect(self.image, (255,0,0), (1,1,
                                 int(self.boss.rect.width * self.percent),5),0) # fill green
            self.oldpercent = self.percent
            self.rect.centerx = self.boss.rect.centerx
            self.rect.centery = self.boss.rect.centery - self.boss.rect.height /2 - 10
            if self.boss.hitpoints < 1:   #check if boss is still alive
                self.kill() # kill the hitbar        
     
        
     
        
            
class Hitpointbar(pygame.sprite.Sprite):
        """shows a bar with the hitpoints of a Bird sprite
           with a given bossnumber, the Lifebar class can 
           identify the boos (Bird sprite) with this codeline:
           Bird.birds[bossnumber] """
        def __init__(self, boss):
            #self.groups = allgroup
            self.boss = boss
            #self._layer = self.boss._layer
            pygame.sprite.Sprite.__init__(self, self.groups)
            self.oldpercent = 0
            self.paint()
        def paint(self):
            self.image = pygame.Surface((self.boss.rect.width,7))
            self.image.set_colorkey((0,0,0)) # black transparent
            pygame.draw.rect(self.image, (1,1,1), (0,0,self.boss.rect.width,7),1)
            self.rect = self.image.get_rect()
       
        def update(self, time):
            self.percent = self.boss.hitpoints / self.boss.maxhitpoints * 1.0
            if self.percent != self.oldpercent:
                self.paint() # important ! boss.rect.width may have changed (because rotating)
                pygame.draw.rect(self.image, (0,0,0), (1,1,self.boss.rect.width-2,5)) # fill black
                pygame.draw.rect(self.image, (30,240,26), (1,1,
                                 int(self.boss.rect.width * self.percent),5),0) # fill green
            self.oldpercent = self.percent
            self.rect.centerx = self.boss.rect.centerx
            self.rect.centery = self.boss.rect.centery - self.boss.rect.height /2 - 10
            if self.boss.hitpoints < 1:   #check if boss is still alive
                self.kill() # kill the hitbar        
     
        




class Cannon (pygame.sprite.Sprite):
    def __init__(self,x,y, angle):
        pygame.sprite.Sprite. __init__(self,self.groups) # !!!!!!!!!
        
        self.image=pygame.Surface((100,100))
        self.image.fill((1, 2, 3))
        self.image.set_colorkey((1,2,3))
        pygame.draw.rect(self.image,(107,30,0),(30,30,10,60))
        pygame.draw.rect(self.image,(58,19,5),(20,20,30,10))
        pygame.draw.rect(self.image,(58,19,5),(20,80,30,10))
        pygame.draw.rect(self.image,(17,11,9),(20,45,60,15))
        self.image=self.image.convert_alpha()
        self.original=self.image.copy()
        self.oldrect = self.original.get_rect()
        self.image=pygame.transform.rotozoom(self.original, angle, 1.0)
        self.rect=self.image.get_rect()
        self.maxrange = random.randint(200,700)
        self.minrange = random.randint(50,100)
        self.hitpoints = 20
        self.x=x
        self.y=y
        self.dx=0.0
        self.dy=0.0
        self.oldrect.centerx = self.x
        self.oldrect.centery = self.y
        self.rect.centerx = self.x
        self.rect.centery = self.y
        self.reloadtime=0.5 + random.random()*5
        self.reloading= 0
        Reloadingbar(self)
    
    def update(self,seconds):
        # rotate toward mouse
        dx = pygame.mouse.get_pos()[0] - self.rect.centerx 
        dy = pygame.mouse.get_pos()[1] - self.rect.centery
        self.angle = math.atan2(-dx, -dy)/math.pi*180.0 
        self.image = pygame.transform.rotozoom(self.original,self.angle+90,1.0)
        self.rect = self.image.get_rect()
        #self.rect.centerx = self.oldrect.centerx - self.rect.centerx
        #self.y += self.oldrect.centery - self.rect.centery       
        self.rect.centerx=self.x
        self.rect.centery=self.y
        if self.reloading > 0:
            self.reloading -= seconds
        else:
            self.reloading = 0
                        
    def checkrange(self, x, y):
        distancex = x - self.x
        distancey = y - self.y
        distance = ( distancex**2 + distancey**2)**0.5
        if distance<self.maxrange and distance>self.minrange:
            return True
        else:
            return False
            
    def shoot(self, x, y):
        if self.checkrange(x, y):
            if self.reloading <= 0:
                Cannonball( self.x, self.y, x, y)
                self.reloading=self.reloadtime  
       
class Ball(object):
    """this is not a native pygame sprite but instead a pygame surface"""
    def __init__(self, radius = 50, color=(0,0,255), x=320, y=240):
        """create a (black) surface and paint a blue ball on it"""
        self.radius = radius
        self.color = color
        self.x = x
        self.y = y
        # create a rectangular surface for the ball 50x50
        self.surface = pygame.Surface((2*self.radius,2*self.radius))    
        # pygame.draw.circle(Surface, color, pos, radius, width=0) # from pygame documentation
        pygame.draw.circle(self.surface, color, (radius, radius), radius) # draw blue filled circle on ball surface
        self.surface = self.surface.convert() # for faster blitting. 
        # to avoid the black background, make black the transparent color:
        # self.surface.set_colorkey((0,0,0))
        # self.surface = self.surface.convert_alpha() # faster blitting with transparent color
        
    def blit(self, background):
        """blit the Ball on the background"""
        background.blit(self.surface, ( self.x, self.y))


    
####

if __name__ == '__main__':

    # call with width of window and fps
    PygView().run()
