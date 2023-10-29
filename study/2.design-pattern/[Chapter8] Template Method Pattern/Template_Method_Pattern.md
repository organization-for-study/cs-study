# Template Method Pattern (템플릿 메소드 패턴)

## OverView

> 클래스와 메소드등 다양한 요소들을 캡슐화 하며 패턴화를 했다.<br>
> 탬플릿 메소드 패턴은 기능의 프레임(템플릿)와 실제 구현을 분리하는 패턴이다.
>
> <img src="img.png" width="600px" style="border: 4px solid orange"></img>

전체적인 알고리즘은 유사하지만 몇몇부분이 다른 경우에 사용한다.<br>
책에서 예시로 하듯이 동인한 음료 제작 과정이지만 커피와 차의 경우에는 다른 부분이 있다.<br>
이런 경우에는 템플릿 메소드 패턴을 사용하여 공통된 부분을 추상화 하고 다른 부분은 서브클래스에서 구현하도록 한다.<br>

## 구조

```java
public abstract class CaffeineBeverage {
    // 상위에서 제어하는 메소드
    final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    abstract void brew(); // 탬플릿화된 메소드

    abstract void addCondiments();// 탬플릿화된 메소드

    void boilWater() {
        System.out.println("물 끓이는 중");
    }

    void pourInCup() {
        System.out.println("컵에 따르는 중");
    }
}
```

```java
public class Coffee extends CaffeineBeverage {
    @Override
    void brew() {
        System.out.println("필터를 통해서 커피를 우려내는 중");
    }

    @Override
    void addCondiments() {
        System.out.println("설탕과 우유를 추가하는 중");
    }
}
```


-----


