# 20120劉鈞淵_Greenfoot遊戲(翻牌記憶)

## 遊戲情境(擷圖)
![memory](https://hackmd.io/_uploads/HJdRaC0V0.png)


## 程式碼整理
- 場景
```java=
import greenfoot.*;
import java.util.Collections;
import java.util.ArrayList;

public class Table extends World{
    private ArrayList<Card> cards = new ArrayList<Card>();
    private ClickCount clickCount = new ClickCount();

    public Table(){
        super(1080, 720, 1); 

        addObject(clickCount, 1000, 100);
        clickCount.getImage().clear();

        for (int i=0; i<36; i++){
            cards.add(new Card(i%18+1));
        }
        Collections.shuffle(cards);

        for (int c=0; c<cards.size(); c++){
            addObject(cards.get(c), 108+(c%9)*108, 144+(c/9)*144);
        }
    }

    public ClickCount getClickCount() 
    {
        return clickCount;   
    }
}
```

- 卡牌
```java=
import greenfoot.*;
import java.awt.Color;

public class Card extends Actor{
    public static int clicked=0;
    private int value;
    private static Card lastCard;
    private static int pairCount=0; // 配對牌數
    private boolean canClick=true; // 能否點擊
    private boolean flipped=false; // 已被翻開

    public Card(int value){
        this.value = value;
        setImage("spawner.png");
    }

    public int getValue(){
        return value;
    }

    public void setCanClick(boolean canClick){
        this.canClick = canClick;
    }

    public void setFlipped(boolean flipped){
        if (!flipped){
            setImage("spawner.png");
        }
        else{
            setImage(value + ".png");
        }        
        this.flipped = flipped;
    }

    public void act(){
        if (Greenfoot.mouseClicked(this) && canClick){
            clicked++;

            Table table = (Table) getWorld();
            GreenfootImage image = table.getClickCount().getImage();
            image.scale(150, 100);
            image.clear();
            image.drawString("翻牌次數: " + clicked, 0, 10);

            if (clicked % 2 == 1){
                lastCard = this;
                setCanClick(false);
                setFlipped(true);
            } 
            else{
                setFlipped(true);
                setCanClick(false);
                if (this.getValue() == lastCard.getValue()){
                    Greenfoot.playSound("correct.wav");
                    this.setCanClick(false);
                    lastCard.setCanClick(false);

                    pairCount++;
                    Greenfoot.delay(100);
                    table.removeObject(this);
                    table.removeObject(lastCard);

                    if (pairCount == 18){
                        Greenfoot.playSound("win.wav");
                        getWorld().addObject(new Win(), 540, 360); // 顯示結束畫面
                        Greenfoot.stop();
                    }
                }
                else{
                    Greenfoot.playSound("incorrect.wav");
                    Greenfoot.delay(100);
                    setCanClick(true);
                    setFlipped(false);
                    lastCard.setFlipped(false);
                    lastCard.setCanClick(true);
                    lastCard = null;
                }
            }
        }
    }
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/33460

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/33460?embed=true" width="1080" height="720" frameborder="0"></iframe>