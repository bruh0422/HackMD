# 20120劉鈞淵_Greenfoot遊戲(小蜜蜂改版)

:::info
遊戲重點功能
- [x] 隨機掉落物件
- [x] 更新三種角色
- [x] 加入音效
- [x] 加入計分功能
- [x] 增加啟始遊戲說明畫面
:::

## 遊戲情境(擷圖)
![shoot](https://hackmd.io/_uploads/S1o4pSRlC.png)


## 程式碼整理
- 場景
```java=
import greenfoot.*;

public class ShootWorld extends World{
    public static Counter counter = new Counter("Score: "); // 分數計算
    public static Counter heart = new Counter("Heart: "); // 生命計算

    private boolean pause = true;

    public ShootWorld(){    
        super(1080, 720, 1, true);
        addObject(new Gun(), 540, 680);

        addObject(heart, 100, 650);
        heart.set(3);
        addObject(counter, 100, 680);
        counter.set(0);

        addObject(new Help(), 540, 360);
    }

    public boolean getPause()
    {
       return pause;
    }
    public void pauseGame(boolean paused)
    {
       pause = paused;
    }

    public void act(){
        if(getPause() == false){
            if(Greenfoot.getRandomNumber(100) < 3){ // 生成怪物
                Monster monster = new Monster();
                monster.setImage("Monster" + (1+Greenfoot.getRandomNumber(3)) + ".png");
                addObject(monster, Greenfoot.getRandomNumber(getWidth()), 0);   
            }
        }
    }
}
```

- 槍
```java=
import greenfoot.*;

public class Gun extends Actor{
    private boolean bulletExistsInWorld(){ // 子彈是否存在於世界
        return !(getWorld().getObjects(Bullet.class).size() == 0);
    }

    public void click(){
        if(Greenfoot.mouseClicked(null)){
            if (((ShootWorld) getWorld()).getPause() == false){
                ((ShootWorld) getWorld()).pauseGame(true);
                getWorld().addObject(new Help(), 540, 360);
            }
        }
    }

    private boolean isDown;

    public void act(){
        click();

        if(((ShootWorld) getWorld()).getPause() == false){
            if (Greenfoot.isKeyDown("a") || Greenfoot.isKeyDown("left")) setLocation(getX()-10, getY()); // 向左移動
            if (Greenfoot.isKeyDown("d") || Greenfoot.isKeyDown("right")) setLocation(getX()+10, getY()); // 向右移動
            if (Greenfoot.isKeyDown("space") && !bulletExistsInWorld()){
                Greenfoot.playSound("shoot.mp3");
                getWorld().addObject(new Bullet(), getX(), getY()-40); // 一次只能存在一顆子彈
            }
            if (Greenfoot.isKeyDown("x")){
                Greenfoot.playSound("shoot.mp3");
                getWorld().addObject(new Bullet(), getX(), getY()-40); // 無敵版
            }

            // 連續發射
            if (Greenfoot.isKeyDown("z") && !isDown){
                Greenfoot.playSound("shoot.mp3");
                getWorld().addObject(new Bullet(), getX(), getY()-40);
                isDown = true;
            }
            else if (!Greenfoot.isKeyDown("z") && isDown) isDown = false;
        }    
    }
}
```

- 子彈
```java=
import greenfoot.*;

public class Bullet extends Actor{
    private int newY;

    public void act(){
        if(((ShootWorld) getWorld()).getPause() == false){
            newY = getY()-10;
            if (newY < 10) getWorld().removeObject(this); // 超出螢幕就移除
            else{
                setLocation(getX(), getY()-10); // 移動
                Actor hitMonster = getOneIntersectingObject(Monster.class); // 是否碰觸到怪物
                if (hitMonster != null){ // 如果碰觸就移除
                    Greenfoot.playSound("hit.mp3");
                    getWorld().removeObject(hitMonster);
                    ((ShootWorld) getWorld()).counter.add(1);
                    getWorld().removeObject(this);
                }
            }
        }    
    }
}
```

- 敵人
```java=
import greenfoot.*;

public class Monster extends Actor{
    private int newY;
    private ShootWorld world = ((ShootWorld) getWorld());

    public void act(){
        if(((ShootWorld) getWorld()).getPause() == false){
            newY = getY()+(1+((ShootWorld) getWorld()).counter.getValue()/10); // 計算新座標
            Actor touchGun = getOneIntersectingObject(Gun.class); // 是否碰觸到武器
            if (newY > getWorld().getHeight()-10 || touchGun != null){ // 超出螢幕或碰到武器
                Greenfoot.playSound("hurt.mp3");
                ((ShootWorld) getWorld()).heart.subtract(1); // 減去生命值
                getWorld().removeObject(this); // 移除
            }
            else setLocation(getX(), newY); // 移動到新座標
            if (((ShootWorld) getWorld()).heart.getValue() <= 0){
                getWorld().addObject(new Loss(), 540, 360); // 顯示結束畫面
                Greenfoot.stop(); // 結束遊戲
            }
        }    
    }
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/33200

## 遊戲玩法
左右移動｜A D ⬅️ ➡️
單次發射｜Z Space
連續發射｜X

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/33200?embed=true" width="1080" height="720" frameborder="0"></iframe>