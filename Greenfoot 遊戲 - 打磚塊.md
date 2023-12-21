# 20120劉鈞淵_Greenfoot遊戲(打磚塊)

## 遊戲情境(擷圖)
![brick](https://hackmd.io/_uploads/S1deNmWw6.png)

## 程式碼整理
- 場景
```java=
import greenfoot.*;

public class BrickWorld extends World
{
    public BrickWorld()
    {    
        super(400, 300, 1); 
        addObject(new Ball(), 200, 150);
        addObject(new Paddle(), 200, 280);
        for (int i=0; i<8; i++){
            for (int j=0; j<5; j++){
                addObject(new Brick(), 53+i*42, 30+j*20); // 新增磚塊
            }
        }
    }
}
```

- 球
```java=
import greenfoot.*;

public class Ball extends Actor
{
    private int motionX = 2, motionY = 2, brickHit = 0;
    
    public void act() 
    {       
        Actor brick = getOneIntersectingObject(Brick.class);
        if (brick != null){
            motionY = -motionY;
            brickHit++;
            getWorld().removeObject(brick);    
        } // 撞到磚塊就移除
        if (brickHit == 40) Greenfoot.stop(); // 磚塊打完就結束遊戲
        
        Actor paddle = getOneIntersectingObject(Paddle.class);
        if (paddle != null) motionY = -motionY; // 碰到板子反彈
        
        int newX, newY;
        
        newX = getX() + motionX;
        newY = getY() + motionY;
        
        if (newX > 400) motionX = -2;
        else if (newX < 0) motionX = 2; // 碰到左右邊緣反彈
        
        if (newY > 280) Greenfoot.stop(); // 板子沒接到球就停止遊戲
        else if (newY < 0) motionY = 2; // 碰到上邊緣反彈
        
        setLocation(newX, newY); // 移動球到新位置
    }
}
```

- 板子
```java=
import greenfoot.*;

public class Paddle extends Actor
{
    public void act() 
    {
        MouseInfo mouse = Greenfoot.getMouseInfo();;
        if (mouse != null) setLocation(mouse.getX(), getY()); // 讓板子隨著滑鼠移動
    }    
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/32582

## 開始玩遊戲 (embed嵌入)
- 移動滑鼠來控制板子
<iframe src="https://www.greenfoot.org/scenarios/32582?embed=true" width="800" height="600" frameborder="0"></iframe>