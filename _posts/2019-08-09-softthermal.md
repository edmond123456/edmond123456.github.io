---
layout:     post
title:      Thermal display with softness
subtitle:    
date:       2019-08-09
author:     Junjie Hua
header-img: img/post-bg-o.jpg
catalog: true
tags:
    - Research
---

## Soft thermal display and fireball game

I am going to develop a brand-new thermal display with softness.
This display will be used for our fireball game.

The fireball game is a brand-new game which is different from any current game.

-->Current game is based on the visual stimulation-players act according to the visual changes in the screen or HMD(headmount display).

-->Our game is no longer based on visual stimulation any more. We foucus on thermal stimulation. In our idea, players will act according to the thermal changes on palms.

-->In our game, players sense the thermal change at the hands, which is related with  power of the fireball. The hotter players sense, more powerful the fireball will be created. However, temperature of hand(or thermal display) does not change in linear relation--it changes nolinearly.  Pl
Players decide the timing to fire the fireball in their hands.

Our project will be devided into two parts--hardware and softwarre. I will develop the hardware and help my collegue with the development of software. Details will be available at this github repository [https://github.com/mashiro1204/ec2019](https://github.com/mashiro1204/ec2019)

Our project will be demonstrated at [Entertainment Computing 2019](http://ec2019.entcomp.org/).

### Development of soft thermal display

I am going to develop a brand-new soft thermal display. Traditoinal thermal displays usually use several peltier modules on a flat surface to realize display of the temperature. They are used mainly for the thermal experiment but can hardly applied into real world. So I am going to develop a new one that could be applied to real world.

And then I did

![ec2019](https://edmond123456.github.io/img/ec2019/IMG_6270.png)

### Development of the fireball experience



![ec2019](https://edmond123456.github.io/img/ec2019/IMG_6281.png)

![ec2019](https://edmond123456.github.io/img/ec2019/IMG_6282.png)

![ec2019](https://edmond123456.github.io/img/ec2019/IMG_6298.png)

According to feedback from the players, the thermal display can randomly adjust the temperature normally. The random change of the temparature offers some entertainment but since temperature changes in a small range, thermal experience is not very obvious.


### Important Update of Hardware Part(2019/12/18)
The original thermal display could only be utilised to warm up human hands but not to cool down.The main reason is that I did not know why the peltier module cannot work if I change the direction of electric current.Truth is, if I change the direction of the current, at the very first, around 2 seconds,peltier can cool down at a quick speed. But then peltier will soon warm up very quickly.

I did not realize that the problem was lack of the cooling system. When I tried to improve the display which was uesd in EC2019, I realized this problem and fixed it.

I used copper board to conduct heat. Also, considering that this display should be held in hand, heat which has beed tranported to the hot surface(the opposite side of the display surface) cannot do the heat exchange with air well, I used the copper board to transport heat from hot surface to back of the hand so that heat exchange can work well with air.

Thus the cooling problem is solved. Now the new display is able to offer both hot and cold pattern normally, and adjust temperature much more quickly than before.This improvement allows pretty much more interactive content of the tactile game.

![ec2019](https://edmond123456.github.io/img/ec2019/newdisplay.jpg)

### New Possible Content of the Game
Since the new display is able to display much more quickly than before, new content can be realized in the game.

Currently, I am going to develop a game in which players need to switch hot and cold pattern to switch attack pattern--in order to defeat the mob! Cold pattern allows players to cast iceball to slow down movement of the mob while hot pattern allows them to cast fireball to give a destructive damage on the mob.