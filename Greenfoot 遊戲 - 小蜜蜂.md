# 20120劉鈞淵_Greenfoot遊戲(小蜜蜂)


## 遊戲情境(擷圖)
![bee](https://hackmd.io/_uploads/rJowsn62a.png)

## 程式碼整理
- 場景
```java=
import greenfoot.*;

public class MyWorld extends World{
    public MyWorld(){
        super(800, 600, 1, false);
        addObject(new Baby(), 350, 550);
        for (int i=0; i<20; i++) addObject(new Bee(), 40-80*i, 50); // 新增蜜蜂
    }
}
```

- 寶寶
```java=
import greenfoot.*;

public class Baby extends Actor{
    private boolean flowerExistsInWorld(){ // 花朵是否存在於世界
        return !(getWorld().getObjects(Flower.class).size() == 0);
    }

    private boolean isDown;

    public void act(){
        if (Greenfoot.isKeyDown("left")) setLocation(getX()-10, getY()); // 向左移動
        if (Greenfoot.isKeyDown("right")) setLocation(getX()+10, getY()); // 向右移動
        if (Greenfoot.isKeyDown("space") && !flowerExistsInWorld()) getWorld().addObject(new Flower(), getX(), getY()-40); // 一次只能存在一朵花
        if (Greenfoot.isKeyDown("x")) getWorld().addObject(new Flower(), getX(), getY()-40); // 無敵版
        
        // 連續發射
        if (Greenfoot.isKeyDown("z") && !isDown){
            getWorld().addObject(new Flower(), getX(), getY()-40);
            isDown = true;
        }
        else if (!Greenfoot.isKeyDown("z") && isDown) isDown = false;
    }
}
```

- 花
```java=
import greenfoot.*;

public class Flower extends Actor{
    private int newY;

    public void act(){
        newY = getY()-10;
        if (newY < 10) getWorld().removeObject(this); // 超出螢幕就移除
        else{
            setLocation(getX(), getY()-10); // 移動
            Actor hitBee = getOneIntersectingObject(Bee.class); // 是否碰觸
            if (hitBee != null){ // 如果碰觸就移除
                getWorld().removeObject(hitBee);
                getWorld().removeObject(this);
            }
        }
    }    
}
```

- 蜜蜂
```java=
import greenfoot.*;

public class Bee extends Actor{
    private int direction = 10, newX;

    public void act(){
        newX = getX()+direction;
        if ((getRotation() == 0 && newX>=getWorld().getWidth()) || (getRotation() == 180 && newX <= 0)){ // 碰到螢幕邊緣轉彎
            setLocation(getX(), getY()+100);
            setRotation(getRotation()+180);
            direction = -direction;
        }
        else setLocation(newX, getY()); // 否則移動到新位置

        Actor hitBaby = getOneIntersectingObject(Baby.class); // 是否碰觸

        if (hitBaby != null) Greenfoot.stop(); // 如果碰觸就移除
    }    
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/32922

## 遊戲玩法
方向鍵控制寶寶
z x space 發射花朵

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/32922?embed=true" width="1920" height="1080" frameborder="0"></iframe>