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



