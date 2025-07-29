# 객체 지향 프로그래밍(OOP)

- Object Oriented Programming, OOP

## 자바스크립트는 프로토타입 기반 OOP

> java, c++ 클래스 기반 OOP와 차이가 있다.

- Javascript는 Host 환경에서 처리를 실행하며, 오브젝트를 조작(제어)한다.

## Host Enviornment

- 자바스크립트와 인터페이스하는 외부 오브젝트의 총칭
- window Object, DOM Object 등
  - DOM Object에는 많은 오브젝트가 있다.
  - Javascript가 제공하는 객체가 아니다.

```
const node = document.getElementById('change');
node.onClick = () => {
    node.style.color = "red";
}

const list = ['id', 'className'];
list.forEach((name)=> {
    console.log(${name}, ${node[name]})
})
```

- getElementById 메소드를 호출하면 id 속성값이 change 인 KeyElementObject를 생성하여 변환한다.

  > DOM의 DOM Object에서 ElementObject를 생성할 수 있는 기능을 제공하는 것

- Javascript는 Host 환경의 Object를 사용하여 제어를 수행한다.

  > Method를 호출하여 ElementObject를 생성한다.

- Element Object를 생성한다.
- id 속성값, class 속성값을 구한다.
- id, className은 DOM Object Property 이름이다.

> 자바스크립트는 이렇게 Host 환경의 Object를 사용하여 제어를 수행하는 것이다.

- 여기서 화살표 함수는 Javascript이다.

  > Host 환경과 Javascript를 연계시킨다.

- Javascript로 제어한 결과를 Host 환경의 Object를 사용하여 Browser에 표현한다.

  > Browser에 표현하는 것은 DOM과 CSSOM이 한다.

  - Node 변수에 할당된 Key Element 객체의 Style Property의 Color Property Red를 할당한다.

  - Javascript 로 Element의 Property값을 변경하고, DOM과 CSSOM이 이를 받아 브라우저에 표현한다.
    > 이것이 Host환경과 Javascript 이다.

## Host 환경은 기능을 가진 Object를 제공하고 Javascript는 이를 사용하여 기능을 제어한다.

- Host환경에서 Javascript가 실행되려면 Interface 하는 기준이 필요하다.
  > 이것을 IDL(Interface Definition Language)라고 한다.
  - DOM String Type이 Javascript String에 Binding 하는 방법이 기준이 스펙으로 정의되어있다.
- Host 환경에는 다수의 Object가 있다.
  Javascript가 다수의 Obejct를 따라가면서 Interface를 맞출 수가 없다는 것이다.
  그래서, 다수의 Object에서 Javascript의 인터페이스를 맞추는 것이다.
  이것이 Web IDL Javascirpt binding

- Host 환경의 모든 Object는 Javascript에 맞추어야 한다.

---

# OOP 특성 - 추상화

- OOP의 주요 4가지 특성; 추상화, 상속, 다형성, 캡슐화

## 추상화(Abstraction)

```
const Book = class {
    constructor(title, price){
        this.title = title;
        this.price = price;
    }
    getTitle(){
        return this.title;
    }

    getPrice(){
        return this.price;
    }
}

const instance = new Book("자바스크립트", 100);
console.log(instance.getTitle())
```

- new 연산자로 Book 생성자 함수를 호출한다.
- getTitle 메서드에서 this가 참조하는 title property값을 반환한다.

- 책을 판매할때도 타이틀과 단가가 필요하다.
- 책을 구매할때도 타이틀과 단가가 필요하다.

  > 책 판매와 책 구매의 공통 속성은 타이틀과 단가이다.

- 또한 타이틀을 구하거나 단가를 구하는 것은 '행위'이다.
  > 공통 속성과 행위를 객체로 정의하는 것이다.
  > -> 이것이 추상화이다.

```
const Book = class {}
```

- Book Class 를 선언한다.
- 책 판매와 책 구매의 공통 속성(title, price)이 있고, 행위가 있다.
- Class는 선언만 한 상태이므로 Class의 Method를 호출할 수 없다.
  - new 연산자로 Book 생성자함수를 호출한다.
  - constructor가 호출된다.
  - this.를 사용하여 InstanceProperty로 설정한다.
  - title 과 price는 Instance Propety 이며, 속성에 해당한다.

```
instance.getTitle()
```

- getTitle()은 Method, 행위에 해당한다.

# OOP 특성 - 상속(Inhertiance)

```
const BookSale = class extends Book {
    constructor(title, price){
        super(title, price)
    }
}
const instance = new BookSale("자바스크립트", 100);
console.log(instance.getTitle())
```

- 서브 클래스에서 슈퍼 클래스의 속성과 행위를 공유한다.
- Javascript에서 상속은 Super Class의 Prototype을 Instance 형태로 생성하여 Sub Class.[[ProtoType]]에 첨부하는 것이다.

- 엔진이 Prototype Slot으로 상속 구조를 만든다.

```
const BookSale = class extends Book{
    constructor(title, price){
        super(title, price)
    }
}
```

- BookSale(sub)클래스에서는 Book(super)클래스를 상속받는다.
- Constructor : new 연산자로 BookSale 생성자 함수를 호출하면서 ("자바스크립트", 100)을 파라미터로 넘겨준다.
- BookSale이 sub Class이고, Book이 super Class 이다.

```
const instance = new BookSale("js", 100);
console.log(instance.getTitle())
```

- 선언만 한 것은 개념적 형태의 상속이고, new 연산자로 생성자 함수를 호출했을 때 상속 구조를 만든다.

- super를 호출하면 Class의 생성자 함수가 호출된다. 이때 BookSale Class Instance를 생성한다. Book Class의 prototype에 연결된 Method로 Instance로 생성하여 BookSale Instance의 Prototype Slot에 첨부한다.
  > Prototype Slot으로 상속받는 구조가 만들어진다.

# 다형성(Polymorphism)

## Overriding

- 서브클래스와 슈퍼클래스에 같은 이름의 Method가 있다.
- 먼저 서브 클래스에서 식별자가 해결한다.
- 식별자가 해결되면 메소드를 실행한다.
- 식별자가 해결되지 않으면 슈퍼 클래스에서 식별자를 해결한다.

## Overloading

- Class에 같은 이름의 Method가 있지만, 파라미터 수 또는 타입이 다르다.
- 자바스크립트는 {key:value} 형태이므로, 오버로딩을 지원한다.

클래스 이름이 같은 메소드가 있다. 그런데, 메소드의 파라미터 수, 타입이 다르다.

```
const Book = class {
    constructor(title, price){
        this.title = title;
        this.price = price;
    }
    getAmount(qty){
        return this.price = qty;
    }
}

const BookSale = class extends Book {
    constructor(title, price){
        super(title, price)
    }
    getAmount(qty, point){
        return super.getAmount(qty)-point;
    }
}

const instance = new BookSale("js", 100)
console.log(instance.getAmount(50, 1000))
```

```
instance.getAmount(50,1000)
```

- Book Class와 BookSale Class에 getAmount가 있다.
- 인스턴스.[[prototype]].getAmount()
  인스턴스.[[prototype]].[[prototype]].getAmount()

- getAmount로 [[prototype]] chain에서 식별자를 해결한다.
- 인스턴스.[[Prototype]].getAmount()가 호출된다.
  ->BookSale Class의 getAmount()가 호출된다.

- 인스턴스.[[Prototype]].[[prototype]].getAmount()는 호출되지 않는다.
  -> Book Class의 getAmount()가 호출되지 않는다.

- super가 prototype chain에서 상속받은 바로 앞 super class의 prototype slot을 참조한다.

```
return super.getAmount(qty)-point
```

- overriding
- super class와 sub class 에 같은 이름의 method가 있다.
- subClass에서 식별자를 해결한다.-> 식별자가 해결되면 method를 실행한다.
  -> getAmount와 getAmount가 있지만, 먼저 BookSale Class의 getAmount를 호출하는 것이다.

---

# 캡슐화(Encapsulation)

- 인스턴스의 값(데이터)를 은닉하여 외부로부터 보호한다.
- 캡슐 속에 은닉할 데이터를 넣고, 캡슐 사용만 외부에 공개하는 개념이다.

## private

- #price처럼 #{hash}를 prefix로 사용하여 선언한다.
- 스펙에 #price 형태를 private property가 아니라 private field로 기술되어 있다.
- 한편, method는 private method로 기술하고 있다.

## field와 property에 대한 생각

- 일반적으로 field는 스펙에서 사용한다.(Promise, reolved와 같은 형태)

  - [[Promise]] field, [[Resolved]] field
  - Field names are always enclosed in double brackets, for example [[Value]]
  - Field 이름은 [[Value]]처럼 항상 대괄호 두 개를 사용한다.

  - Private field가 결국 {kay:value} 형태로 저장되므로, private프로퍼티라고 부르는 것이 일관성이 있다. 한편, property는 포괄적인 면도 있으며, private을 특별하게 나타내기 위한 뉘앙스도 있다.

  - 스펙이 기준이다.

  - 한편, 필드와 method를 포괄적으로 나타낼 때는 private property로 표기한다.

```
const Book = class {
    #price; // private으로 price field를 선언
    constructor(price){
        this.#price = price;
    }
    getPrice(){
        return this.#price;
    }
}

const BookSale = class extends Book {
    constructor(price){
        super(price)
    }
    getAmount(qty){
        // return this.#price *qty;
        return this.getPrice() * qty;
    }
}

const instance = new BookSale(100);
console.log(instance.getAmount(50))
```

```
// return this.#price * qty;
```

- sub class에서 super class의 private 필드를 this.price 형태로 access 할 수 없다.
- private 필드가 선언된 class의 method에서 private field에 access할 수 있다.

```
this.getPrice() *qty;
```

- #price 필드 값을 구하기 위해 #price 필드를 가진 getPrice()를 호출한다.

```
// instance.#price
```

- 클래스 밖에서 직접(인스턴스.private field 이름) 형태로 access 할 수 없다.
  이런 매커니즘으로 #price 필드 값을 보호할 수 있다.

### private 로 선언할 수 있는 것

- private Method
- private static ${field}, static Method
- private getter, setter,
- private static getter, setter

# Instnace 목적

```
const Sports = class {
    constructor(title){
        this.title = title;
    }
    getTitle(player){
        return this.title + player;
    }
}

const soccer = new Sports("축구");
console.log(soccer.getTitle("11명"))

const swim = new Sports("수영");
console.log(swim.getTitle("1명"))
```

- 인스턴스마다 프로퍼티 값을 각각 유지하기 위함

```
const soccer = new Sports("축구")
```

    - soccer 인스턴스의 title property 값은 "축구"이다.
    - 모든 인스턴스에서 Property 값을 각각 유지하려면 Instance Property로 정의한다.

- 각 인스턴스의 property를 같은 방법으로 CRUD 하려는 것
  - 추상화, 상속, 다형성, 캡슐화는 이를 위한 수단이다.
  - 파라미터 값은 다르지만, 메소드의 처리 방법은 같다.
