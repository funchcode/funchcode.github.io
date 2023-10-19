---
layout: default
title: "[Design Pattern] 전략패턴"
tags: Design Pattern
icon: group.ico 
---

## 전략패턴 개념 정리

디자인 패턴 중 하나인 전략패턴(Strategy Pattern)을 정리한다.  

### 인터페이스 - 구현체

인터페이스(interface)로 상세 구현은 없지만 어떤 행위를 정의할 수 있다.  
사용자는 인터페이스를 사용할 때 어떤 행위를 할 것이라는 추측을 하여 사용할 수 있다.  

> 사용자는 많은 것(구현 세부사항)을 알고 싶지 않다.  

구현체는 인터페이스를 implements 한다.  
인터페이스가 선언하고 있는 추상적인 행위에 대해서 세부적인 구현 내용을 작성한다.  

```java
// 사용자는 공격 행위를 하는 인터페이스라고 예측할 수 있다.
interface Weapon {

    public void attack();

}

// 사용자 입장에서 Ax 클래스와 Bow 클래스는 Weapon을 구현하고 있으니 공격 행위를 갖는다고 예측할 수 있다.
final class Ax implements Weapon {

    @Override
    public void attack() {
        // 복잡한 구현 내용...
        System.out.println("내려찍다.");
        // ...복잡한 구현 내용
    }

}
final class Bow implements Weapon {

    @Override
    public void attack() {
        // 복잡한 구현 내용...
        System.out.println("쏘다.");
        // ...복잡한 구현 내용
    }

}
```

### 전략

알고리즘(구현)을 정의하고 교체할 수 있도록 하는 것을 말한다.  

여기서 3가지 구성요소가 발생하는데,  

1. 콘텍스트(Context)
2. 전략(Strategy)
3. 구체적인 전략(ConcreteStrategy)

__콘텍스트(Context)__  
전략 패턴의 주체 클래스로 외부 클라이언트로부터 전략을 주입받아 실행하는 대상이다.  

__전략(Strategy)__  
어떤 행위만 가진 인터페이스(interface) 또는 추상 클래스(abstract class)를 말한다.

__구체적인 전략(ConcreteStrategy)__  
Strategy 인터페이스를 구현하는 구현 클래스로 실제 행위를 구현한 대상을 말한다.

```java
// 구성요소 중 콘텍스트(Context)에 해당하는 클래스이다.
final class Character {

    private Weapon weapon;

    // 인자로 Weapon 구현체를 주입받는다.
    // setter 또는 생성자를 통해 Weapon 구현체를 주입할 수도 있다.
    public void doAttack(Weapon weapon) {
        weapon.attack();
    }

}

```java
class Game {

    public static void play() {
        Character someUser = new Character();
        
        // 도끼 사용
        someUser.doAttack(new Ax()); // 내려찍다.

        // 활 사용
        someUser.doAttack(new Bow()); // 쏘다.
    }

}
```

### 장점

01) 알고리즘 교체가 용이하다.  

- 위와 같이 '도끼', '활' 등 Weapon을 구현한 구현체를 동적으로 변경할 수 있다.

02) 개방-폐쇄 원칙(Open-Closed Principle)을 준수한다.

- 새로운 전략을 추가하거나 기존 전략을 수정하지 않고도 시스템을 확장할 수 있다.

```java
// "새로운 전략 추가"
// 새로운 전략을 추가할 때 Character 콘텍스트 클래스를 변경이 발생하지 않는다.
final class Sword implements Weapon {

    @Override
    public void attack() {
        // 복잡한 구현 내용...
        System.out.println("썰다.");
        // ...복잡한 구현 내용
    }

}

// "기존 전략 수정"
// 핵심 로직이 한 곳에 응집되어 있기 때문에 다른 곳에 수정이 발생하지 않는다.
final class Bow implements Weapon {

    @Override
    public void attack() {
        // 복잡한 구현 내용...
        // System.out.println("쏘다.");
        System.out.println("마구 쏘다.");
        // ...복잡한 구현 내용
    }

}
```

3) 테스트하기 용이하다.

- 각 개별 전략 별로 테스트를 진행할 수 있다.