# CH7

### 상속(Inheritance)

* 기존의 클래스로 새로운 클래스를 작성하는 것(코드의 재사용)
* 두 클래스를 부모와 자식으로 관계를 맺어주는 것

```java
class 자식클래스 extends 부모 클래스{
	// ...    
}
class Parent {}
class Chile extends Parent{
    // ...
}
```

* 자손은 조상의 모든 멤버를 상속받는다.(생성자, 초기화블럭 제외)
* 자손의 멤버 개수는 조상보다 적을 수 없다.(같거나 많다.)
* 자손의 변경은 조상에 영향을 미치지 않는다.

<img src="img/%EC%83%81%EC%86%8D.png" alt="상속" style="zoom:77%;" align="left" />



*  Parent의 멤버는 1개 Child는 자신의 멤버 1개 + 상속받은 멤버 1개 = 2개

```java
class Point{
    int x;
    int y;
}

// Point클래스와 관계 없음               // Point와 상속 관계
class Point3D{                        class Point3D extends Point{
    int x;                                int z;
    int y;                            }
    int z; 
}
```

### 포함관계

* 클래스의 멤버로 참조변수를 선언하는 것
* 작은 단위의 클래스를 만들고, 이 들을 조합해서 클래스를 만든다.

```java
class Point{
    int x;
    int y;
}                                                               0x100            0x200
class Circle{  // Cicle이 Point를 포함하는 관계       c[0x100] --> [0x200]c   -->  [  0  ]x
    Point c = new Point(); // 원점                               [  0  ]r        [  0  ]y
    int r; // 반지름(radius)
}
```

* 클래스 간의 관계 결정하기
  * 상속관계 : A는 B이다. (A is a B)
  * 포함관계 : A는 B를 가지고 있다.(A has a B) 

### 단일 상속

*  Java는 단일 상소만을 허용함
* 비중이 높은 클래스 하나만 상속관계로, 나머지는 포함관계로 한다.

```java
class Tv{
    boolean power;
    int channel;
    
    void power() { power = !power; }
    void channelUp() { ++channel; }
    void channelDown() { --channel; }
}
class DVD{
    boolean power;
    
    void power() { power = !power;}
    void play() { /* 내용 생략*/ }
    void stop() { /* 내용 생략*/ }
    void rew() { /* 내용 생략*/ }
    void ff() { /* 내용 생략*/ }
}


Class TvDVD extends Tv{ // 비중이 높은 클래스 하나만 상속관계
    Dvd dvd = new DVD(); // 포함관계로 처리
    
    void play(){
        dvd.play();
    }
    void stop(){
        dvd.stop();
    }
    void rew(){
        dvd.rew();
    }
    void ff(){
        dvd.ff();
    }
}
```

* Object 클래스  - 모든 클래스의 조상
  * 부모가 없는 클래스는 자동적으로 Object클래스를 상속받게 된다.
  * 모든 클래스는 Object 클래스에 정의된 11개의 메서드를 상속받음

### 오버라이딩(Overriding)

* 상속받은 조상의 메서드를 자신에 맞게 변경하는 것

```java
class Point{
    int x;
    int y;
    
    String getLocation(){
        return "x :" + x + ", y :" + y;
    }
}
class Point3D extends Point{
    int z;
    
    String getLocation(){ //오버라이딩(내용만 변경 가능)
        return "x :" + x + ", y :" + y +", z:" + z; 
    }
}
```

#### 오버라이딩 조건

* 선언부가 조상 클래스의 메서드와 일치해야 한다.
* 접근 제어자를 조산 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
* 예외는 조상 클래스의 메서드보다 많이 선언할 수 없다.

#### 오버로딩 vs 오버라이딩

* 오버로딩(overloading) : 기존에 없는 새로운 메서드를 정의하는 것(new)
* 오버라이딩(overriding) : 상속받은 메서드의 내용을 변경하는 것(change, modify)

```java
class Parent{
    void parentMethod() {}
}
class Child extends Parent{
    void parentMethod() {}  // 오버라이딩
    void parentMethod(int i) {}  // 오버로딩
    
    void childMethod() {}    // 메서드 정의
    void childMethod(int i) {}  // 오버로딩
    void childMethod() {} // 중복정의
}
```

### 참조변수 super

* 객체 자신을 가리키는 참조변수. 인스턴스 메서드(생성자)내에만 존재
* 조상의 멤버를 자신의 멤버와 구별할 때 사용

```java
class Ex7_2{
    public static void main(String args[]){                  0x100
        Child c = new Child();               c[0x100]  --->  [   10   ]this.x
        c.method();                                          [   20   ]super.x 
    }                                                        [method()]method
}
class Parent {
    int x = 10; // super.x
}
class Child extends Parent{
    int x = 20; // this.x
    
    void method(){
        System.out.println("x=" + x);
        System.out.println("this.x=" + this.x);
        System.out.println("super.x=" + super.x);
    }
}
```

```java
class Ex7_3{
    public static void main(String args[]){                  0x100
        Child c = new Child();               c[0x100]  --->  [   10   ]super.x
        c.method();                                          [method()]method 
    }                                                        
}
class Parent {
    int x = 10; // super.x와 this.x 둘 다 가능
}
class Child extends Parent{
    void method(){
        System.out.println("x=" + x);
        System.out.println("this.x=" + this.x);
        System.out.println("super.x=" + super.x);
    }
}
```

#### super() - 조상의 생성자

* 조상의 생성자를 호출할 때 사용
* 조상의 멤버는 조상의 생성자를 호출해서 초기화
* 생성자 첫 줄에 반드시 다른 생성자를 호출해야 함

```java
class Point{
    int x, y;
    
    Point(int x, int y){
        this.x = x;
        this.y = y;
    }
}

class Point3D extends Point{             --->         class Point3D extends Point{
    int z;                                                int z;
    Point3D(int x, int y, int z){                         Point3D(int x, int y, int z){
        this.x = x; /* 조상의 멤버를 초기화 */                   super(x, y); // 조상의 클래스 생성자를 호출
        this.y = y; /* 조상의 멤버를 초기화 */                   this.z = z;  // 자신의 멤버를 초기화
        this.z = z;                                       }
    }                                                  }
}
```

### 패키지(Package)

* 서로 관련된 클래스의 묶음
* 패키지는 소스파일의 첫 번째 문장으로 단 한번 선언
* 패키지 선언이 없으면 이름없는(unnamed) 패키지에 속하게 됨

### import 문

* 클래스를 사용할 때 패키지 이름을 생략할 수 있다.
* 컴파일러에게 클래스가 속한 패키지를 알려준다.
* java.lang 패키지의 클래스는 import하지 않고도 사용할 수 있다.
* static 멤버를 사용할 때 클래스 이름을 생략할 수 있게 해준다.

```java
import 패키지명.클래스명;
또는
import 패키지명.*;
```

### 제어자

* 클래스와 클래스의 멤버(멤버 변수, 메서드)에 부가적인 의미 부여
* 접근 제어자 : public, protected, default, privte
* 그 외 : static, final, abstract 등...
* 하나의 대상에 여러 제어자를 같이 사용 가능(접근 제어자는 하나만!)

#### static - 클래스의, 공통적인

* 멤버 변수 
  * 모든 인스턴스에 곹오적으로 사용되는 클래스 변수
  * 클래스 변수는 인스턴스를 생서하지 않고도 사용 가능
  * 클래스가 메모리에 로드될 때 생성됨
* 메서드
  * 인스턴스를 생서하지 않고도 호출이 가능한 static 메서드가 됨
  * static 메서드 내에서는 인스턴스 멤버들을 직접 사용할 수 없음

#### final - 마지막의, 변경될 수 없는

* 클래스
  * 변경될 수 없는 클래스, 확장될 수 없는 클래스가 됨
  * final로 지정된 클래스는 다른 클래스의 조상이 될 수 없음
* 메서드
  * 변경될 수 없는 메서드, final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없음
* 멤버변수, 지역변수
  * 변수 앞에 final이 붙이면, 값을 변경할 수 없는 상수가 됨

#### abstract - 추상의, 미완성의

* 인스턴스 생성 불가 (AbstractTest a = new AbstractTest 불가능)

* 클래스
  * 클래스 내에 추상 메서드가 선언되어 있음을 의미
* 메서드
  * 선언부만 작성하고 구현부는 작성하지 않은 추상 메서드임을 알림

### 접근 제어자

* private : 같은 클래스 내에서만 접근이 가능
* default : 같은 패키지 내에서만 접근이 가능
* protected : 같은 패키지 내에서, 그리고 다른 패키지의 자손클래스에서 접근이 가능
* public :  접근 제한이 전혀 없음

<img src="img/%EC%A0%91%EA%B7%BC%20%EC%A0%9C%EC%96%B4%EC%9E%90.JPG" alt="접근 제어자" style="zoom: 100%;" align="left" />

* 접근제어자를 사용하는 이유?
  * 외부로부터 데이터를 보호하기 위해서 사용함
  * 외부에는 불필요한, 내부적으로만 사용되는 부분을 감추기 위해서

### 캡슐화

* 관련있는 변수와 함수를 하나의 클래스로 묶고 외부에서 쉽게 접근하지 못하도록 은닉하는 것

```java
public class Time{
    private int hour;  //private로 외부 접근을 차단
    private int minute;
    private int second;
    
    
    public int getHour(){  // 메소드는 public로 설정하여 간접 접근 허용
        return hour;
    }
    public void setHour(int hour){
        if (hour < 0 || hour > 23) return; // 엉뚱한 값이 들어오지 않도록 값을 보호함
        this.hour = hour;
    }
}

public class Ex7_22 {
	public static void main(String args[]){
        Time t = new Time();
        t.hour = 25; // 멤버변수에 직접 접근 -> private로 막혀서 접근 불가
        t.setHour(25); // 메소드를 통한 간접 접근이지만, if문에 의해 hour는 변하지 않음
        t.setHour(21); // 메소드를 통한 간접 접근으로 hour는 21로 저장됨
    }
}
```

### 다형성(polymorphism)

* 조상 타입 참조 변수로 자손 타입 객체를 다루는 것
* 자손 타입의 참조 변수로 조상 타입의 객체를 가리킬 수 없음
* 장점
  * 1. <span style='background-color:#fff5b1' >다형적 매개변수</span>
    2. <span style='background-color:#fff5b1' >하나의 배열로 여러종류 객체 다루기</span>

```java
class Tv{
    boolean power;
    int channel;
    
    void power() { power = !power; }
    void channelUp() { ++channel; }
    void channelDown() { --channel; }
}
class SmartTv extends Tv{
    String text;
    void caption() {/*내용 생략*/}
}

public class Polymorphism{
    public static void main(String args[]){
        // 참조 변수와 객체의 타입이 일치해야함
        Tv t = new Tv();
        SmartTv s = new SmartTv();
        
        // 타입이 일치하지 않아도 괜찮음(다형성 )
        // 조상 타입 참조 변수(t)로 자손 타입 객체(SmartTv)를 다룸
        Tv t = new SmartTv();
    }
}
```

* 객체와 참조변수의 타입이 일치할 때와 일치하지 않을 때의 차이?
* SmartTv s = new SmartTv();  ---> 7 개의 기능을 사용(power, channel, power(), channelUp(), channelDown(), text, caption())
* Tv t = new SmartTv();  ---> 5개의 기능을 사용(power, channel, power(), channelUp(), channelDown())

### 참조변수의 형변환

* 사용할 수 있는 멤버의 갯수를 조절하는 것
* 조상 자손 관계의 참조변수는 서로 형변환 가능

```java
class Car{
    String color;
    int door;
    
    void drive(){
        System.out.println("drive, Brrr~~~");
    }
    void stop(){
        System.out.println("stop~~~");
    }
}

class FireEngine extends Car{
    void water(){
        System.out.println("water!!!");
    }
}

public class Ex7_24{
    public static void main(String args[]){
        FireEngine f = new FireEngine();    // 5개의 멤버를 사용 가능
        
        Car c = (Car)f;                   //OK, 조상인 Car 타입으로 형변환, 4개의 멤버만 사용 가능
        FireEngine f2 = (FireEngine)c;    //OK, 자손인 FireEngine타입으로 형변환,  5개의 멤버를 사용 가능
        Ambulance a = (Ambulance)f;       //에러, 상속관계가 아닌 클래스 간의 형변환 불가
    }
}
                     0x100
f[0x100]    --->     [  null ]color
    ↓        ↗      [   0   ]door
c[0x100]     ↗      [drive()]
    ↓                [ stop()]
f2[0x100]            [water()]
```

### instanceof 연산자

* 참조변수의 형변환 가능여부 확인에 사용, 가능하면 true 반환
* 형변환 전에 반드시 instanceof로 확인해야 함.

```java
void doWork(Car c){ 
    if (c instanceof FireEngine) {      // 1. 형변환이 가능한지 확인 
        FireEngine fe = (FireEngine)c;  // 2. 형변환
        fe.water();
    }
}
```

### 매개변수의 다형성

* 참조형 매개변수는 메서드 호출시, <span style="color:red">자신과 같은 타입 또는 자손 타입</span>의 인스턴스를 넘겨줄 수 있다.

```java
class Product{
    int price;
    int bonusPoint;
}
class Tv extends Product{}
class Computer extends Product{}
class Audio extends Product{}
class Buyer{
    int money = 1000;
    int bonusPoint = 0;
    
    //buy 기능 추가 하려고함
    //but, 각 인스턴스마다 추가해줘야함 => 코드의 중복 발생         다형성으로 코드 중복 줄임
    void buy(Tv t){                       -->                 void buy(Product p){
        money =- t.price;                                         money =- p.price;
        bonusPoint += t.bonusPoint;                               bonusPoint += p.bonusPoint;
    }                                                         }
    void buy(Computerv c){
        money =- t.price;
        bonusPoint += t.bonusPoint;
    }
    void buy(Audio a){
        money =- t.price;
        bonusPoint += t.bonusPoint;
    }
}

class Ex7_27{
    public static void main(String args[]){
        Buyer b = new Buyer();
        
        // 아래와 같은 의미(다형성:조상의 참조 객체로 자손의 객체를 다룸)
        // Product p = new Tv();
        // b.buy(p);
        b.buy(new Tv());
        b.buy(new Computer());
        
        System.out.println("현재 남은 돈은 " + b.money + "만원입니다.");
        System.out.println("현재 보너스점수는 " + b.bonusPoint + "만원입니다.");
    }
}
```

### 여러 종류의 객체를 배열로 다루기

* 조상타입의 배열에 자손들의 객체를 담을 수 있다.

```java
Product p1 = new Tv();                                Product p[] = new Product[3];
Product p2 = new Computer();              ----->      p[0] = new Tv();
Product p3 = new Audio();                             p[1] = new Computer();
                                                      p[2] = new Audio();

               0x100 (Tv)                                           0x001             0x100 (Tv)
p1[0x100] ---> [ 0 ]price                 ----->       p[0x001] --->[0x100]p[0]  ---> [ 0 ]price
               [ 0 ]bonusPoint                                                        [ 0 ]bonusPoint
               0x200 (Computer)                                                       0x200 (Computer)
p2[0x200] ---> [ 0 ]price                                           [0x200]p[1]  ---> [ 0 ]price
               [ 0 ]bonusPoint                                                        [ 0 ]bonusPoint
               0x300 (Audio)                                                          0x300 (Audio)
p3[0x300] ---> [ 0 ]price                                           [0x300]p[2]  ---> [ 0 ]price
               [ 0 ]bonusPoint                                                        [ 0 ]bonusPoint
```

* Vector 클래스 : 가변 배열 기능

```java
public class Vector extends AbstractList
    implements List, Cloneable, java.io.Serializable {
    protected Object elementData[]; // 모든 종류의 객체 저장 가능
}
```

