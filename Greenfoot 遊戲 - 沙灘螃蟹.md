# 20120劉鈞淵_Greenfoot遊戲(沙灘螃蟹)

## 遊戲情境(擷圖)
![crab](https://hackmd.io/_uploads/r1ccLFSH6.png)

## 程式碼整理
- 場景(沙灘)
```java=
import greenfoot.*;

import java.util.Random;

public class CrabWorld extends World
{
    public CrabWorld() 
    {
        super(1000, 800, 1);
        addObject(new Crab(), (int)(Math.random()*1000+1), (int)(Math.random()*800+1));
        addObject(new Lobster(), (int)(Math.random()*1000+1), (int)(Math.random()*800+1));
        for (int i=0; i<=10; i+=1){
            addObject(new Worm(), (int)(Math.random()*1000+1), (int)(Math.random()*800+1));
        }
    }
}
```

- 螃蟹
```java=
import greenfoot.*; 

public class Crab extends Animal{
    public void act(){
        if (getImage() == image1){
            setImage(image2);
        }
        else{
            setImage(image1);
        }
        move();
        if (canSee(Worm.class)){
            eat(Worm.class);
            Greenfoot.playSound("slurp.wav");
        }
        if (atWorldEdge()){
            turn(100);
        }
        if (Greenfoot.isKeyDown("left")){
            turn(5);
        }
        if (Greenfoot.isKeyDown("right")){
            turn(-5);
        }
    }
    
    private GreenfootImage image1, image2;
    public Crab() {
        image1 = new GreenfootImage("crab.png");
        image2 = new GreenfootImage("crab2.png");
        setImage(image1);
    }
}
```

- 龍蝦
```java=
import greenfoot.*;

public class Lobster extends Animal{
    public void act() {
        move();
        if (atWorldEdge()){
            turn(100);
        }
        if (canSee(Crab.class)){
            eat(Crab.class);
            Greenfoot.playSound("au.wav");
            Greenfoot.stop();
        }
    }    
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/32429

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/32429?embed=true" width="800" height="600" frameborder="0"></iframe>