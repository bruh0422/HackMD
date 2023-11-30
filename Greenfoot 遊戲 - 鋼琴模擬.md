# 20120劉鈞淵_Greenfoot遊戲(鋼琴模擬)

:::info
遊戲重點功能
- [x] 12個白鍵+8個黑鍵
- [x] 鋼琴面版文字
- [x] 琴鍵與鍵盤對應文字
- [ ] 自動彈奏曲目
- [ ] 七彩炫光效果
:::

## 遊戲情境(擷圖)
![piano](https://hackmd.io/_uploads/SyBa7tBSp.png)

## 程式碼整理
- 場景(鋼琴)
```java=
import greenfoot.*;

public class Piano extends World
{
    private String[] whiteKeys = {"a", "s", "d", "f", "g", "h", "j", "k", "l", ";", "'", "enter"};
    private String[] whiteNotes = {"3c", "3d", "3e", "3f", "3g", "3a", "3b", "4c", "4d", "4e", "4f", "4g"};
    private String[] blackKeys = {"W", "E", "", "T", "Y", "U", "", "O", "P", "", "]"};
    private String[] blackNotes = {"3c#", "3d#", "", "3f#", "3g#", "3a#", "", "4c#", "4d#", "", "4f#"};
    
    public void showMessage()
    {
        GreenfootImage bg = getBackground();
        bg.setColor(Color.WHITE);
        bg.drawString("S T E I N W A Y  &  S O N S", 325, 360);
    }
    public void showWhiteKey()
    {
        GreenfootImage bg = getBackground();
        bg.setColor(Color.GREEN);
        bg.drawString("　　a　　　　   s　　　　   d　　　　   f　　　　   g　　　　   h　　　　   j　　　　   k　　　　   l　　　　    ;　　　　     '　　　　  enter", 25, 340);
    }
    public void showBlackKey()
    {
        GreenfootImage bg = getBackground();
        bg.setColor(Color.GREEN);
        bg.drawString("　　　　  W　　　　   E　　　　　　　   　   T　　　　   Y　　　　   U　　　　　　   　　   O　　　　   P　　　　　　　　        ]", 25, 25);
    }
    public Piano() 
    {
        super(800, 400, 1);
        for (int i=0; i<12; i++){
            addObject(new Key(whiteKeys[i], whiteNotes[i]+".wav"), 54+i*63, 180);
        }
        for (int i=0; i<11; i++){
            if (!blackKeys[i].equals(""))
                addObject(new blackKey(blackKeys[i], blackNotes[i]+".wav"), 85+i*63, 126);
        }
        showMessage();
        showWhiteKey();
        showBlackKey();
    }
}
```

- 白鍵
```java=
import greenfoot.*;

public class Key extends Actor
{
    private String key;
    private String sound;
    public Key(String keyName, String soundFile)
    {
        key = keyName;
        sound = soundFile;
    }
    
    private boolean isDown;
    public void act(){
        if (!isDown && Greenfoot.isKeyDown(key)){
            setImage("white-key-down.png");
            Greenfoot.playSound(sound);
            isDown = true;
        } 
        if (isDown && !Greenfoot.isKeyDown(key)) {
            setImage("white-key.png");  
            isDown = false;
        }
    }
}
```

- 黑鍵
```java=
import greenfoot.*;

public class blackKey extends Actor
{
    private String key;
    private String sound;
    public blackKey(String keyName, String soundFile)
    {
        key = keyName;
        sound = soundFile;
    }
    
    private boolean isDown;
    public void act(){
        if (!isDown && Greenfoot.isKeyDown(key)){
            setImage("black-key-down.png");
            Greenfoot.playSound(sound);
            isDown = true;
        } 
        if (isDown && !Greenfoot.isKeyDown(key)) {
            setImage("black-key.png");  
            isDown = false;
        }
    }
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/32427

## 開始玩遊戲 (embed嵌入)
- 使用鍵盤彈奏
<iframe src="https://www.greenfoot.org/scenarios/32427?embed=true" width="800" height="600" frameborder="0"></iframe>