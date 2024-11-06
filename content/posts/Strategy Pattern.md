+++
title = 'Strategy Pattern'
date = 2024-11-06T11:42:58+08:00
draft = false
+++

賽爾號是多數人的童年回憶，遊戲中的戰鬥機制是每回合玩家從精靈的四招技能中挑選一招，攻擊對手或強化自己。
如果想模擬陽春版的戰鬥機制，沒學過設計模式的我會先做一個抽象的精靈類別來讓後來新增的精靈繼承，並在類別中的實作施放方法。

```java
abstract class 精靈 {
    private final String 姓名;

    public 精靈() {
        姓名 = this.getClass().getSimpleName();
        System.out.println("召喚" + 姓名);
    }

    protected void 施放(String 技能) {
        switch (姓名) {
            case "魔焰猩猩" -> {
                if (技能.equals("魔焰裂空擊")) {
                    // 做很多事...
                    System.out.println("使用魔焰裂空擊");
                } else {
                    System.out.println("沒有這招！");
                }
            }
            case "麗莎布布" -> {
                if (技能.equals("金光綠葉")) {
                    // 做很多事...
                    System.out.println("使用金光綠葉");
                } else {
                    System.out.println("沒有這招！");
                }
            }
            case "魯斯王" -> {
                if (技能.equals("湍流龍擊打")) {
                    // 做很多事...
                    System.out.println("使用湍流龍擊打");
                } else {
                    System.out.println("沒有這招！");
                }
            }
            default -> System.out.println("你誰？");
        }
    }
}

class 魔焰猩猩 extends 精靈 {}
class 麗莎布布 extends 精靈 {}
class 魯斯王 extends 精靈 {}

public class 賽爾號 {
    public static void main(String[] args) {
        精靈 魔焰猩猩 = new 魔焰猩猩(); // 召喚魔焰猩猩
        精靈 麗莎布布 = new 麗莎布布(); // 召喚麗莎布布
        精靈 魯斯王 = new 魯斯王(); // 召喚魯斯王

        魔焰猩猩.施放("魔焰裂空擊"); // 使用魔焰裂空擊
        麗莎布布.施放("金光綠葉"); // 使用金光綠葉
        魯斯王.施放("湍流龍擊打"); // 使用湍流龍擊打
    }
}
```

看起來好像不錯，結果也正確，但要是當精靈學習新技能，或是要新增精靈時，就必須要修改精靈類別，條件判斷語句就會越寫越長，邏輯也會變得超複雜，程式碼就會很難維護與擴充。
因此，可以創建一個技能介面，讓精靈類別中的技能依賴於此介面，將實作的部分外包出去，日後若要使用其他新技能時，只要建立新的技能類別並實作技能介面就可以在不改動原有的類別下擴充新類別。

```java
abstract class 精靈 {
    public 精靈() {
        String 姓名 = this.getClass().getSimpleName();
        System.out.println("召喚" + 姓名);
    }

    protected void 施放(技能 技能) {
        技能.施放();
    }
}

class 魔焰猩猩 extends 精靈 {}
class 麗莎布布 extends 精靈 {}
class 魯斯王 extends 精靈 {}

interface 技能 {
    void 施放();
}

class 魔焰裂空擊 implements 技能 {
    @Override
    public void 施放() {
        System.out.println("使用魔焰裂空擊");
    }
}

class 金光綠葉 implements 技能 {
    @Override
    public void 施放() {
        System.out.println("使用金光綠葉");
    }
}

class 湍流龍擊打 implements 技能 {
    @Override
    public void 施放() {
        System.out.println("使用湍流龍擊打");
    }
}

public class 賽爾號 {
    public static void main(String[] args) {
        精靈 魔焰猩猩 = new 魔焰猩猩(); // 召喚魔焰猩猩
        精靈 麗莎布布 = new 麗莎布布(); // 召喚麗莎布布
        精靈 魯斯王 = new 魯斯王(); // 召喚魯斯王

        魔焰猩猩.施放(new 魔焰裂空擊()); // 使用魔焰裂空擊
        麗莎布布.施放(new 金光綠葉()); // 使用金光綠葉
        魯斯王.施放(new 湍流龍擊打()); // 使用湍流龍擊打
    }
}

```

## 總結

策略模式提高了程式碼的擴充性和可維護性，因為它允許在不修改原有類別的情況下新增新的技能或精靈。
透過將精靈與技能的實作分離，降低了耦合度，使精靈類別依賴於技能介面而非具體實現，提高了程式的靈活性。
