# 20120劉鈞淵_Greenfoot遊戲(撲克牌)

## 遊戲情境(擷圖)
![poker](https://hackmd.io/_uploads/Skw95LnfA.png)


## 程式碼整理
- 場景
```java=
import greenfoot.*;
import java.util.Collections;
import java.util.List;

public class Table extends World{
    private Deck deck, shuffle_deck, auto_deck, random_deck, clear_deck;
    public Table(){
        super(1080, 720, 1);

        // 依序發牌
        deck = new Deck();
        addObject(deck, 100, 100);

        // 洗牌發牌
        shuffle_deck = new Deck();
        addObject(shuffle_deck, 200, 100);
        Collections.shuffle(shuffle_deck.cards);
        shuffle_deck.setImage("shuffle_deck.png");

        // 自動發牌
        auto_deck = new Deck();
        addObject(auto_deck, 300, 100);
        auto_deck.setImage("auto_deck.png");
        
        // 隨機發牌
        random_deck = new Deck();
        addObject(random_deck, 400, 100);
        Collections.shuffle(random_deck.cards);
        random_deck.setImage("random_deck.png");
        
        // 清空
        clear_deck = new Deck();
        addObject(clear_deck, 550, 100);
        random_deck.setImage("clear_deck.png");
    }

    private int deck_x=100, shuffle_deck_x=100, auto_deck_x=100;
    private boolean auto_deck_clicked=false, random_deck_clicked=false;

    public void act(){
        if (Greenfoot.mouseClicked(deck) && deck.getSize()>0){
            addObject(deck.getCard(), deck_x, 250);
            deck_x += 15;
        }

        if (Greenfoot.mouseClicked(shuffle_deck) && shuffle_deck.getSize()>0){
            addObject(shuffle_deck.getCard(), shuffle_deck_x, 350);
            shuffle_deck_x += 15;
        }

        if (Greenfoot.mouseClicked(auto_deck) && !auto_deck_clicked){
            auto_deck_clicked = true;
        }
        if (auto_deck_clicked && auto_deck.getSize()>0){
            Card card = auto_deck.getCard();
            addObject(card, auto_deck_x, 450);
            auto_deck_x += 15;
        }

        if (Greenfoot.mouseClicked(random_deck) && !random_deck_clicked){
            random_deck_clicked = true;
        }
        if (random_deck_clicked && random_deck.getSize()>0){
            Card card = random_deck.getCard();
            addObject(card, Greenfoot.getRandomNumber(getWidth()), 250+Greenfoot.getRandomNumber(getHeight()-250));
        }
        
        if (Greenfoot.mouseClicked(clear_deck)){
            List objects = getObjects(null);
            removeObjects(objects);
            
            if (deck.getSize()>0) addObject(deck, 100, 100);
            if (shuffle_deck.getSize()>0) addObject(shuffle_deck, 200, 100);
            if (auto_deck.getSize()>0) addObject(auto_deck, 300, 100);
            if (random_deck.getSize()>0) addObject(random_deck, 400, 100);
            
            addObject(clear_deck, 550, 100);
        }
    }
}
```

- 牌堆
```java=
import greenfoot.*;
import java.util.ArrayList;
import java.util.Collections;

public class Deck extends Actor{
    public ArrayList<Card> cards;

    public Deck(){
        cards = new ArrayList<Card>();
        for (Card.Suit suit : Card.Suit.values()){
            for (Card.Value value : Card.Value.values()){
                cards.add(new Card(value, suit));
            }
        }
    }

    public int getSize(){ // 回傳剩餘牌數
        return cards.size();
    }

    public Card getCard(){ // 回傳第一張卡
        if (getSize()==0) return null;

        Card card = cards.get(0);
        cards.remove(card);
        if(getSize() == 0) getWorld().removeObject(this);

        return card;
    }
}
```

- 卡牌
```java=
import greenfoot.*;

public class Card extends Actor{
    public enum Suit {CLUBS, HEARTS, SPADES, DIAMONDS};
    public enum Value {ACE, TWO, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN, JACK, QUEEN, KING};

    private Suit suit;
    private Value value;

    public Card(Value value, Suit suit){
        this.value = value;
        this.suit = suit;
        draw();
    }

    protected void draw(){
        String fileName = "images/cards/";
        fileName += value;
        fileName += suit;
        fileName += ".png";
        fileName = fileName.toLowerCase();
        setImage(new GreenfootImage(fileName));
    }
}
```

## 遊戲連結
https://www.greenfoot.org/scenarios/33276

## 開始玩遊戲 (embed嵌入)
<iframe src="https://www.greenfoot.org/scenarios/33276?embed=true" width="1080" height="720" frameborder="0"></iframe>