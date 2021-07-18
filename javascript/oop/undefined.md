---
description: 객체지향 문법 예제
---

# 예제 코드

## Encapsulation, Abstraction

> 접근 허용: public\(기본설정\), protected\(자식클래스에서는 허용\), private\(내부에서만 허용\)
>
> 캡슐화, 정보은닉: 외부에서 접근이 가능하고, 필요한 것만 노출하기\(데이터를 보호\)
>
> 추상화: 외부에서 어떻게 이 클래스를 사용할 수 있는 지 생각하 인터페이스나 상속 등을 이용하여 간단하게 사용할 수 있게 만드는 것
>
> static -&gt; 클래스 레벨에서 필요한 변수와 메서드에 쓰인다.

```typescript
  type CoffeeCup = {
    shots: number;
    hasMilk: boolean;
  };

  // M: 클래스 메서드에 대한 구현부를 명세할 수 있다.
  interface CoffeeMaker {
    makeCoffee(shots: number): CoffeeCup;
  }

  interface CommercialCoffeeMaker {
    makeCoffee(shots: number): CoffeeCup;
    fillCoffeeBeans(beans: number): void;
    clean(): void;
  }
  
  // CoffeeMaker, CommercialCoffeeMaker에 정의된 메서드를 구현해야 함
  class CoffeeMachine implements CoffeeMaker, CommercialCoffeeMaker {
    private static BEANS_GRAMM_PER_SHOT: number = 7; // class level
    private coffeeBeans: number = 0; // instance (object) level
    
    // 기본 생성자 대신 static 메서드를 이용하여 인스턴스를 생성하도록 유도
    // 상속하는 자식 클래스가 있을 경우 생성자는 protected 이어야 한다.(자식클래스에서 자동 호출)
    protected constructor(coffeeBeans: number) {
      this.coffeeBeans = coffeeBeans;
    }

    static makeMachine(coffeeBeans: number): CoffeeMachine {
      return new CoffeeMachine(coffeeBeans);
    }
    // 공개 메서
    fillCoffeeBeans(beans: number) {
      if (beans < 0) {
        throw new Error('value for beans should be greater than 0');
      }
      this.coffeeBeans += beans;
    }

    clean() {
      console.log('cleaning the machine...🧼');
    }

    // private 함수로 내부 로직 함수를 감춰서 추상화 레벨 높이기(필요한 함수만 노출하기)
    private grindBeans(shots: number) {
     // 새부구현 생략..
    }

    private preheat(): void {
      // 새부구현 생략..
    }

    private extract(shots: number): CoffeeCup {
      // 새부구현 생략..
    }
    // 공개 메서
    makeCoffee(shots: number): CoffeeCup {
      this.grindBeans(shots);
      this.preheat();
      return this.extract(shots);
    }
```

## getter, setter

> get으로 멤버변수를 조합하여 반환하기, set으로 유효성 검사 이후 값을 할당하기

```typescript
class User {
    // M: getter 로 멤버 변수를 조합하여 반환받을 수 있다. (fullName 멤버변수가 필요 없어짐)
    get fullName(): string {
      return `${this.firstName} ${this.lastName}`;
    }

    private internalAge = 4;

    get age(): number {
      return this._age;
    }
    // M: setter 로 유효성 검사 후 값을 할당할 수 있다.
    set age(num: number) {
      if (num < 0) {}
      this._age = num;
    }
    // M: 멤버변수 깔끔하게 할당하기
    constructor(private firstName: string, public lastName: string) {}
  }

  const user = new User('Steve', 'Jobs');
  user.age = 6;
  console.log(user.fullName);
```

## 인터페이스 타입으로 사용가능한 메서드를 제한하기

> 인터페이스로 지원하는 메서드의 범주를 알 수 있고 컨트롤 할 수 있다. 아래의 아마추어 클래스는 CoffeeMaker 인터페이스에 명시된 메서드만 사용이 가능하다.

```typescript
  class AmateurUser {
    constructor(private machine: CoffeeMaker) {}
    // CoffeeMaker 인터페이스에 규약된 메서드를 사용하면 된다. (무엇이 넘어오는지는 상관하지 않아도 된다.)
    makeCoffee() {
      const coffee = this.machine.makeCoffee(2);
      console.log(coffee);
    }
  }

  class ProBarista {
    constructor(private machine: CommercialCoffeeMaker) {}
    makeCoffee() {
      const coffee = this.machine.makeCoffee(2);
      console.log(coffee);
      this.machine.fillCoffeeBeans(45);
      this.machine.clean();
    }
  }
```

## 부모 클래스의 로직 재사용하기

```typescript
  class CaffeLatteMachine extends CoffeeMachine {
    constructor(beans: number, public readonly serialNumber: string) {
      super(beans);
    }
    
    private steamMilk(): void {
      console.log('Steaming some milk... 🥛');
    } // CODE: 부모 클래스의 로직을 재사용하기
    
    makeCoffee(shots: number): CoffeeCup {
      // 자식에서 부모클래스의 메서드를 이용하고 싶을 때(완전히 새롭게 오버라이딩하는 것이 아니라 특정 로직만 추가할 때)
      const coffee = super.makeCoffee(shots);
      this.steamMilk();
      
      return {
        ...coffee,
        hasMilk: true,
      };
    }
  }
```

## 컴포지션

상속의 문제점을 컴포지션으로 해결한다.

* 두 개 이상의 부모클래스를 가질 수 없다. 
* 상속관계의 복잡성은 계속 증가한다.
* 부모클래스의 코드 수정이 자식 클래스에 영향을 미칠 수 있다.\(사이드이펙트\)

#### 클래스 간의 관계 피하고\(상속의 문제점 중 하나인 사이드이펙트가 때문\), 인터페이스로 통신하기

> 인터페이스로 통신한다 -&gt; 인터페이스를 구현하는 모든 구현체를 이해할 수 있다.

```typescript
  type CoffeeCup = {
    shots: number;
    hasMilk?: boolean;
    hasSugar?: boolean;
  };
  
  interface MilkFrother {
    makeMilk(cup: CoffeeCup): CoffeeCup;
  }

  interface SugarSource {
    addSugar(cup: CoffeeCup): CoffeeCup;
  }
 
  class NoMilk implements MilkFrother {
    makeMilk(cup: CoffeeCup): CoffeeCup {
      return {
        ...cup,
        hasMilk: false,
      };
    }
  }

  class NoSugar implements SugarSource {
    addSugar(cup: CoffeeCup): CoffeeCup {
      return {
        ...cup,
        hasSugar: false,
      };
    }
  }
  
  class FancyMilkSteamer implements MilkFrother {
    makeMilk(cup: CoffeeCup): CoffeeCup {
      console.log(`Fancy!!!! Steaming some milk🥛...`);
      return {
        ...cup,
        hasMilk: true,
      };
    }
  }

  class AutomaticSugarMixer implements SugarSource {
    addSugar(cuppa: CoffeeCup): CoffeeCup {
      console.log(`Adding sugar...`);
      return {
        ...cuppa,
        hasSugar: true,
      };
    }
  }
```

#### 상속이 아닌 인터페이스 타입의 구현체를 주입 받아 로직을 사용한다.

> 인터페이스를 구현하는 클래스의 구현체들은 전부 주입할 수 있다. -&gt; 상속받아 기능을 확장하는 것이 아닌 주입을 받아 기능을 결정한다.
>
>  주입 받는 구현체에 따라 다양한 동작을 할 수 있고, 메인 클래스 하나와 여러 부품들을 컴포지션하는 것만으로 상속의 복잡성 문제를 피할 수 있다.

```typescript
  interface CoffeeMaker {
    makeCoffee(shots: number): CoffeeCup;
  }
  
  class CoffeeMachine implements CoffeeMaker {
    private static BEANS_GRAMM_PER_SHOT: number = 7; // class level

    constructor(
      private coffeeBeans: number = 0,
      private milkMaker: MilkFrother,
      private sugarMaker: SugarSource
    ) {
      this.coffeeBeans = coffeeBeans;
    }
    
    // 내부 로직 함수(private)는 생략
    
    makeCoffee(shots: number): CoffeeCup {
      this.grindBeans(shots);
      this.preheat();
      const coffee = this.extract(shots);
      const addedMilk = this.milkMaker.makeMilk(coffee);
      return this.sugarMaker.addSugar(addedMilk);
    }
    
  const americanoMachine = new CoffeeMachine(32, new NoMilk(), new NoSugar());
  
  const latteMachine = new CoffeeMachine(
    12,
    new FancyMilkSteamer(),
    new NoSugar()
  );
  
  const sugarAmericanoMachine = new CoffeeMachine(
    24,
    new NoMilk(),
    new AutomaticSugarMixer()
  );

  [americanoMachine, latteMachine, sugarAmericanoMachine].forEach(machine => {
    console.log(machine.makeCoffee(1)); // 다형성
  });
```

## abstract

> 자식클래스에게 공통기능과 메서드 명세를 제공하기, abstract class는 인스턴스를 만드는 것이 불가능하

```typescript
abstract class CoffeeMachine implements CoffeeMaker {
  // private functions...
  protected abstract extract(shots: number): CoffeeCup;
  // 공개 메서
  makeCoffee(shots: number): CoffeeCup {
      this.grindBeans(shots); // 공통 기능
      this.preheat(); // 공통 기능 
      return this.extract(shots); // 자식에서 정의된 대로 실행
  }
}

class SweetCoffeeMaker extends CoffeeMachine {
    protected extract(shots: number): CoffeeCup {
      return {
        shots,
        hasSugar: true,
      };
    }
  }
  
const machines: CoffeeMaker[] = [
    new CaffeLatteMachine(16, '1'),
    new SweetCoffeeMaker(16),
    new CaffeLatteMachine(16, '1'),
    new SweetCoffeeMaker(16),
  ];
  
  machines.forEach(machine => {
    console.log('-------------------------');
    machine.makeCoffee(1);
  });
```



