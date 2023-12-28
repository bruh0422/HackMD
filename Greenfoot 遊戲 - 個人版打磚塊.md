# 20120劉鈞淵_Greenfoot遊戲(個人版打磚塊)

:::info
* 遊戲重點功能
- [x] 畫面大小為 800\*600
- [x] 更新背景、磚塊、球、板子四種角色
- [x] 球碰到磚塊及板子時，加入音效
- [x] 所有磚塊打完時會出現過關畫面
- [x] 板子沒接到球時出現遊戲結束訊息

* 額外功能
- [ ] 背景音樂
- [ ] 關卡切換
- [ ] 計分功能
:::

## 遊戲情境(擷圖)
![brick](https://hackmd.io/_uploads/ByNo_v9v6.png)

## 程式碼整理
- 場景
```java=
import greenfoot.*;

public class BrickWorld extends World
{
    private int random;
    
    public BrickWorld()
    {    
        super(1080, 720, 1); 
        addObject(new Ball(), 540, 400);
        addObject(new Paddle(), 540, 680);
        for (int i=0; i<21; i++){
            for (int j=0; j<5; j++){
                Brick brick = new Brick();
                random=(int)(Math.random()*25)+1;
                brick.setImage("brick" + random + ".png");
                
                addObject(brick, 50+i*49, 50+j*50); // 新增磚塊
            }
        }
    }
}
```

- 球
```java=
import greenfoot.*;

public class Ball extends Actor{
    private int motionX=2, motionY=2, brickHit=0, turn=3, newX, newY;
    
    private boolean touchPaddle; // 偵測球是否碰到板子(避免板子卡住球)
    public void act() 
    {
        Actor brick = getOneIntersectingObject(Brick.class);
        if (brick != null){
            motionY = -motionY;
            brickHit++;
            Greenfoot.playSound("hit_brick.wav");
            getWorld().removeObject(brick);    
        } // 撞到磚塊就移除
        if (brickHit == 105){ // 磚塊打完
            getWorld().addObject(new Win(), 540, 360); // 顯示結束畫面
            Greenfoot.stop(); // 結束遊戲
        }
        
        Actor paddle = getOneIntersectingObject(Paddle.class);
        if (paddle != null && !touchPaddle){
            motionY = -motionY;
            Greenfoot.playSound("Paddle.wav");
            touchPaddle = true;
        } // 碰到板子反彈
        else touchPaddle = false;
        
        newX = getX() + motionX;
        newY = getY() + motionY;
        
        if (newX > 1080){ // 碰到右邊緣
            motionX = -2; // 反彈
            turn = -3; // 讓球逆時針轉
        }
        else if (newX < 0){
            motionX = 2; // 反彈
            turn = 3; // 讓球順時針轉
        }
        
        if (newY > 680){ // 板子沒接到球
            getWorld().addObject(new Loss(), 540, 360); // 顯示結束畫面
            Greenfoot.stop(); // 停止遊戲
        }
        else if (newY < 0) motionY = 2; // 碰到上邊緣反彈
        
        turn(turn); // 讓球轉向
        setLocation(newX, newY); // 移動球到新位置
    }
}
```

- 板子
```java=
import greenfoot.*;

public class Paddle extends Actor
{
    /**
     * Act - 隨便 Paddle 想做什麼。
     * 每次按下「單步執行」或「執行」按鈕，都會呼叫這個方法。
     */
    public void act() 
    {
        MouseInfo mouse = Greenfoot.getMouseInfo();;
        if (mouse != null) setLocation(mouse.getX(), getY()); // 讓板子隨著滑鼠移動
    }    
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/32619

## 遊戲玩法
移動滑鼠來控制板子接球

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/32619?embed=true" width="1920" height="1080" frameborder="0"></iframe>