# 이름: 이재인, 학번: 202330121

작동 정상

```java
import java.util.*;

public class Midterm {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("시작 수를 입력하세요: ");
        int start = sc.nextInt();

        System.out.print("끝 수를 입력하세요: ");
        int end = sc.nextInt();

        System.out.print("찾고자 하는 배수를 입력하세요: ");
        int multiple = sc.nextInt();

        // 처음 수보다 끝 수가 작다면 두 수 교환
        if (start > end) {
            int temp = start;
            start = end;
            end = temp;
        }

        int count = 0;
        for (int i = start; i <= end; i++) {
            if (i % multiple == 0) {
                count++;
            }
        }

        // 개수만큼 배열 생성
        int[] multiples = new int[count];
        int index = 0;

        // 배열에 배수 저장
        for (int i = start; i <= end; i++) {
            if (i % multiple == 0) {
                multiples[index++] = i;
            }
        }

        System.out.println(start + "부터 " + end + "까지 사이의 " + multiple + "의 배수는 " + count + "개입니다.");
        System.out.println("배수 배열: " + Arrays.toString(multiples));

        sc.close();
    }
}
```

# Java 9주차 

## 상속 정리

## 1. 클래스 상속이란?

Java에서 상속은 기존 클래스의 기능을 물려받아서 새로운 클래스를 만드는 것이다.

```java
class ColorPoint extends Point {
}
```

여기서 `Point`는 슈퍼 클래스이고, `ColorPoint`는 서브 클래스이다.

- 슈퍼 클래스: 부모 클래스
- 서브 클래스: 자식 클래스

서브 클래스 객체는 슈퍼 클래스의 멤버도 함께 가진다.

예를 들어 `Point` 클래스에 `x`, `y`가 있고, `ColorPoint` 클래스에 `color`가 있다면 `ColorPoint` 객체는 `x`, `y`, `color`를 모두 사용할 수 있다.

---

## 2. 상속 예제

```java
class Point {
    private int x, y;

    public void set(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void showPoint() {
        System.out.println("(" + x + "," + y + ")");
    }
}

class ColorPoint extends Point {
    private String color;

    public void setColor(String color) {
        this.color = color;
    }

    public void showColorPoint() {
        System.out.print(color);
        showPoint();
    }
}
```

실행 예시는 다음과 같다.

```java
ColorPoint cp = new ColorPoint();
cp.set(3, 4);
cp.setColor("red");
cp.showColorPoint();
```

출력 결과:

```text
red(3,4)
```

`ColorPoint` 클래스 안에는 `set()` 메소드가 직접 없지만, `Point` 클래스를 상속받았기 때문에 사용할 수 있다.

---

## 3. 슈퍼 클래스 멤버 접근

서브 클래스가 슈퍼 클래스의 멤버에 접근할 수 있는지는 접근 지정자에 따라 달라진다.

| 접근 지정자 | 서브 클래스에서 접근 가능 여부 |
|---|---|
| `private` | 접근 불가능 |
| default | 같은 패키지일 때 접근 가능 |
| `public` | 항상 접근 가능 |
| `protected` | 같은 패키지이거나 상속 관계일 때 접근 가능 |

```text
private   : 서브 클래스에서도 직접 접근할 수 없다.
default   : 같은 패키지 안에서만 접근할 수 있다.
public    : 어디서든 접근할 수 있다.
protected : 같은 패키지 또는 상속받은 클래스에서 접근할 수 있다.
```

예시:

```java
class Parent {
    private int a;
    int b;
    public int c;
    protected int d;
}

class Child extends Parent {
    void test() {
        // a = 1; // private라서 접근 불가
        b = 2;    // 같은 패키지면 접근 가능
        c = 3;    // public이라 접근 가능
        d = 4;    // protected라 접근 가능
    }
}
```

---

## 4. 서브 클래스와 슈퍼 클래스의 생성자

서브 클래스 객체가 생성될 때는 서브 클래스 생성자만 실행되는 것이 아니라 슈퍼 클래스 생성자도 함께 실행된다.

실행 순서는 다음과 같다.

```text
슈퍼 클래스 생성자 실행
서브 클래스 생성자 실행
```

예시:

```java
class Parent {
    Parent() {
        System.out.println("부모 생성자");
    }
}

class Child extends Parent {
    Child() {
        System.out.println("자식 생성자");
    }
}
```

실행 코드:

```java
Child c = new Child();
```

출력 결과:

```text
부모 생성자
자식 생성자
```

즉, 자식 객체를 만들면 부모 생성자가 먼저 실행되고 그 다음 자식 생성자가 실행된다.

---

## 5. super() 키워드

`super()`는 서브 클래스 생성자에서 슈퍼 클래스 생성자를 직접 호출할 때 사용한다.

```java
class Parent {
    Parent(int x) {
        System.out.println("부모 생성자: " + x);
    }
}

class Child extends Parent {
    Child() {
        super(10);
        System.out.println("자식 생성자");
    }
}
```

출력 결과:

```text
부모 생성자: 10
자식 생성자
```

`super()`를 직접 쓰지 않으면 컴파일러가 자동으로 `super();`를 넣는다.

하지만 부모 클래스에 기본 생성자가 없으면 오류가 발생한다.  
이 경우에는 반드시 `super(값)` 형태로 부모 생성자를 직접 호출해야 한다.

---

## 6. 오류 정리

### ClassNotFoundException 오류

```text
Error: Could not find or load main class Midterm
```

이 오류는 보통 실행 위치가 잘못되었거나 클래스 이름을 잘못 입력했을 때 발생한다.

```powershell
cd C:\Java2\Midterm
javac Midterm.java
java Midterm
```

### reached end of file while parsing 오류

```text
error: reached end of file while parsing
```

이 오류는 중괄호 `{ }`를 제대로 닫지 않았을 때 발생한다.

잘못된 예:

```java
public class Midterm {
```

올바른 예:

```java
public class Midterm {
    public static void main(String[] args) {

    }
}
```

---

## 7. 업캐스팅(upcasting) 개념

업캐스팅은 서브 클래스 객체를 슈퍼 클래스 타입의 참조 변수에 대입하는 것이다.

쉽게 말하면 자식 객체를 부모 타입으로 바라보는 것이다.

```java
class Person { }

class Student extends Person { }

Student s = new Student();
Person p = s; // 업캐스팅
```

`Student`는 `Person`을 상속받았기 때문에 `Student` 객체를 `Person` 타입 변수에 넣을 수 있다.

슬라이드에서는 생물이 들어가는 박스에 사람이나 코끼리를 넣어도 문제가 없다고 설명한다.  
사람이나 코끼리 모두 생물을 상속받은 것으로 볼 수 있기 때문이다.

업캐스팅의 특징은 다음과 같다.

```text
서브 클래스의 레퍼런스를 슈퍼 클래스 레퍼런스에 대입한다.
슈퍼 클래스 레퍼런스로 서브 클래스 객체를 가리키게 된다.
```

---

## 8. 업캐스팅 시 접근 범위

업캐스팅을 하면 실제 객체는 서브 클래스 객체이지만, 참조 변수의 타입은 슈퍼 클래스가 된다.

```java
class Person {
    String name;
}

class Student extends Person {
    String grade;
}

Person p = new Student(); // 업캐스팅
```

이 경우 실제 객체는 `Student` 객체이지만, 참조 변수 `p`의 타입은 `Person`이다.

따라서 `p`를 통해서는 `Person` 클래스에 있는 멤버만 접근할 수 있다.

```java
p.name = "이재문"; // 가능
p.grade = "A";   // 오류
```

`grade`는 `Student` 클래스의 멤버이기 때문에 `Person` 타입인 `p`로는 바로 접근할 수 없다.

---

## 9. 다운캐스팅(downcasting)

다운캐스팅은 업캐스팅된 객체를 다시 원래의 서브 클래스 타입으로 되돌리는 것이다.

```text
슈퍼 클래스 레퍼런스를 서브 클래스 레퍼런스에 대입하는 것
업캐스팅 된 것을 다시 원래 타입으로 되돌리는 것
```

다운캐스팅을 할 때는 반드시 명시적으로 타입 변환을 적어야 한다.

```java
class Person { }

class Student extends Person { }

Person p = new Student(); // 업캐스팅

Student s = (Student)p; // 다운캐스팅
```

여기서 `p`는 `Person` 타입이지만 실제로는 `Student` 객체를 가리키고 있다.  
그래서 `(Student)p`처럼 형변환을 해주면 다시 `Student` 타입으로 사용할 수 있다.

---

## 10. 다운캐스팅 사례

```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }
}

class Student extends Person {
    String grade;
    String department;

    Student(String name) {
        super(name);
    }
}

public class DowncastingEx {
    public static void main(String[] args) {
        Person p = new Student("이재문"); // 업캐스팅

        Student s;
        s = (Student)p; // 다운캐스팅

        System.out.println(s.name); // 오류 없음
        s.grade = "A";              // 오류 없음
    }
}
```

처음에는 `Student` 객체를 만들었지만 `Person` 타입 변수 `p`에 저장했다.  
그래서 처음에는 `Person`의 멤버만 사용할 수 있다.

이후에

```java
s = (Student)p;
```

처럼 다운캐스팅을 하면 다시 `Student` 타입으로 사용할 수 있다.

그래서 `s.grade`처럼 `Student` 클래스에 있는 멤버도 접근할 수 있다.

---

## 11. 업캐스팅과 다운캐스팅 비교

| 구분 | 설명 |
|---|---|
| 업캐스팅 | 서브 클래스 객체를 슈퍼 클래스 타입으로 변환 |
| 다운캐스팅 | 슈퍼 클래스 타입을 다시 서브 클래스 타입으로 변환 |
| 업캐스팅 형변환 | 자동으로 가능 |
| 다운캐스팅 형변환 | 직접 타입을 적어야 함 |

예시:

```java
Person p = new Student(); // 업캐스팅
Student s = (Student)p;   // 다운캐스팅
```

업캐스팅은 자동으로 처리되지만, 다운캐스팅은 반드시 `(Student)`처럼 변환할 타입을 직접 적어야 한다.

---

## 12. 메소드 오버라이딩(Method Overriding)의 개념

메소드 오버라이딩은 서브 클래스에서 슈퍼 클래스의 메소드를 중복 작성하는 것이다.

즉, 부모 클래스에 이미 있는 메소드를 자식 클래스에서 같은 형태로 다시 만드는 것이다.

```java
class Parent {
    void show() {
        System.out.println("부모 클래스의 show()");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("자식 클래스의 show()");
    }
}
```

위 코드처럼 `Parent` 클래스에 있는 `show()` 메소드를 `Child` 클래스에서 다시 작성하면 오버라이딩이 된다.

오버라이딩이 되면 슈퍼 클래스의 메소드는 무력화되고, 실제 실행될 때는 서브 클래스에서 오버라이딩한 메소드가 실행된다.

```java
Child c = new Child();
c.show();
```

출력 결과:

```text
자식 클래스의 show()
```

슬라이드에서는 오버라이딩을 “메소드 무시하기”라고 번역하기도 한다고 설명한다.

---

## 13. 오버라이딩 조건

오버라이딩을 하려면 슈퍼 클래스 메소드의 원형과 서브 클래스 메소드의 원형이 같아야 한다.

같아야 하는 부분은 다음과 같다.

```text
메소드 이름
인자 타입
인자 개수
리턴 타입
```

예시:

```java
class Parent {
    int sum(int a, int b) {
        return a + b;
    }
}

class Child extends Parent {
    int sum(int a, int b) {
        return a + b + 10;
    }
}
```

위 예시는 메소드 이름, 인자 타입과 개수, 리턴 타입이 같기 때문에 오버라이딩이다.

반대로 인자 개수가 달라지면 오버라이딩이 아니라 새로운 메소드를 하나 더 만든 것이다.

---

## 14. 오버라이딩된 메소드 실행

오버라이딩된 메소드는 참조 변수의 타입이 아니라 실제 객체의 타입을 기준으로 실행된다.

```java
class Parent {
    void print() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void print() {
        System.out.println("Child");
    }
}

Parent p = new Child();
p.print();
```

출력 결과:

```text
Child
```

참조 변수의 타입은 `Parent`이지만 실제 객체는 `Child` 객체이다.  
그래서 오버라이딩된 메소드는 `Child` 클래스의 `print()`가 실행된다.

```text
멤버 변수 접근은 참조 변수 타입 기준
오버라이딩 메소드 실행은 실제 객체 타입 기준
```

---

## 15. 오버라이딩을 사용하는 이유

오버라이딩은 상속받은 기능을 서브 클래스에 맞게 고쳐 쓰기 위해 사용한다.

부모 클래스에서 기본 동작을 정해두고, 자식 클래스에서 필요한 부분만 다르게 구현할 수 있다.

```java
class Animal {
    void sound() {
        System.out.println("동물이 소리를 낸다");
    }
}

class Dog extends Animal {
    void sound() {
        System.out.println("강아지가 멍멍 짖는다");
    }
}

class Cat extends Animal {
    void sound() {
        System.out.println("고양이가 야옹 운다");
    }
}
```

실행:

```java
Animal a1 = new Dog();
Animal a2 = new Cat();

a1.sound();
a2.sound();
```

출력:

```text
강아지가 멍멍 짖는다
고양이가 야옹 운다
```

이처럼 같은 `sound()` 메소드라도 실제 객체가 무엇인지에 따라 실행 결과가 달라진다.

---

## 16. 동적 바인딩 - 오버라이딩된 메소드 호출

동적 바인딩은 실행할 메소드가 컴파일할 때가 아니라 실행 중에 결정되는 것이다.

오버라이딩된 메소드가 있을 때는 참조 변수의 타입보다 실제 객체의 타입이 더 중요하다.  
즉, 슈퍼 클래스 타입의 변수로 메소드를 호출해도 실제 객체가 서브 클래스 객체라면 서브 클래스에서 오버라이딩한 메소드가 실행된다.

슬라이드에서는 `SuperObject` 하나만 있는 경우와 `SubObject`가 상속받은 경우를 비교해서 설명한다.

### SuperObject 하나만 있는 경우

```java
public class SuperObject {
    protected String name;

    public void paint() {
        draw();
    }

    public void draw() {
        System.out.println("Super Object");
    }

    public static void main(String[] args) {
        SuperObject a = new SuperObject();
        a.paint();
    }
}
```

실행 결과:

```text
Super Object
```

`a.paint()`를 호출하면 `paint()` 안에서 `draw()`가 실행된다.  
이때 객체가 `SuperObject` 하나뿐이므로 `SuperObject`의 `draw()`가 실행된다.

---

### SubObject가 상속받은 경우

```java
class SuperObject {
    protected String name;

    public void paint() {
        draw();
    }

    public void draw() {
        System.out.println("Super Object");
    }
}

public class SubObject extends SuperObject {
    public void draw() {
        System.out.println("Sub Object");
    }

    public static void main(String[] args) {
        SuperObject b = new SubObject();
        b.paint();
    }
}
```

실행 결과:

```text
Sub Object
```

여기서 참조 변수 `b`의 타입은 `SuperObject`이다.

```java
SuperObject b = new SubObject();
```

하지만 실제로 만들어진 객체는 `SubObject`이다.  
따라서 `b.paint()`를 호출하면 `SuperObject`의 `paint()`가 실행되지만, 그 안에서 호출되는 `draw()`는 `SubObject`에서 오버라이딩한 `draw()`가 실행된다.

이것이 동적 바인딩이다.

---

## 17. 동적 바인딩의 핵심

동적 바인딩에서는 오버라이딩된 메소드가 항상 우선적으로 호출된다.

```text
참조 변수 타입: SuperObject
실제 객체 타입: SubObject
호출 메소드: b.paint()
paint() 내부 호출: draw()
실제로 실행되는 draw(): SubObject의 draw()
```

즉, `paint()`는 슈퍼 클래스에 있는 메소드이지만, `paint()` 안에서 호출한 `draw()`는 실제 객체 기준으로 결정된다.

그래서 다음 코드의 결과는 `Super Object`가 아니라 `Sub Object`가 된다.

```java
SuperObject b = new SubObject();
b.paint();
```

출력:

```text
Sub Object
```

슬라이드에 적힌 것처럼 `SuperObject`는 키워드가 아니라 예제에서 만든 클래스 이름이다.

또한 상속받은 경우에는 오버라이딩된 메소드가 있으면 그 메소드가 실행된다.  
이 부분이 오버라이딩과 다형성에서 중요한 부분이다.


---

## 18. 오버로딩과 오버라이딩

오버로딩과 오버라이딩은 이름이 비슷하지만 기준이 다르다.

오버로딩은 같은 이름의 메소드를 여러 개 만드는 것이고, 오버라이딩은 상속 관계에서 부모 클래스의 메소드를 자식 클래스가 다시 작성하는 것이다.

| 비교 요소 | 메소드 오버로딩 | 메소드 오버라이딩 |
|---|---|---|
| 선언 | 같은 클래스나 상속 관계에서 동일한 이름의 메소드 중복 작성 | 서브 클래스에서 슈퍼 클래스에 있는 메소드와 동일한 이름의 메소드 재작성 |
| 관계 | 동일한 클래스 내 혹은 상속 관계 | 상속 관계 |
| 목적 | 이름이 같은 여러 개의 메소드를 중복 선언하여 사용의 편리성 향상 | 슈퍼 클래스에 구현된 메소드를 무시하고 서브 클래스에서 새로운 기능의 메소드를 재정의 |
| 조건 | 메소드 이름은 반드시 동일하고, 인자의 개수나 타입이 달라야 성립 | 메소드 이름, 인자의 타입, 인자의 개수, 리턴 타입이 모두 동일해야 성립 |
| 바인딩 | 정적 바인딩 | 동적 바인딩 |

오버로딩은 컴파일할 때 어떤 메소드를 실행할지 결정된다.  
반면 오버라이딩은 실행 시간에 실제 객체를 보고 오버라이딩된 메소드를 찾아 호출한다.

간단히 구분하면 다음과 같다.

```text
오버로딩: 이름은 같고 매개변수는 다름
오버라이딩: 이름, 매개변수, 리턴 타입이 같음
```

예시:

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

위 코드는 `add`라는 이름은 같지만 매개변수 개수가 다르기 때문에 오버로딩이다.

```java
class Parent {
    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("Child");
    }
}
```

위 코드는 상속 관계에서 같은 형태의 메소드를 다시 작성했기 때문에 오버라이딩이다.

---

## 19. 추상 클래스

추상 클래스는 미완성 메소드를 포함할 수 있는 클래스이다.  
여기서 미완성 메소드는 추상 메소드라고 한다.

### 추상 메소드(abstract method)

추상 메소드는 `abstract`로 선언된 메소드이다.

추상 메소드는 메소드의 코드가 없고, 원형만 선언한다.

```java
abstract public String getName();
```

위 코드처럼 리턴 타입, 메소드 이름, 매개변수만 있고 실행 내용은 없다.

아래처럼 `abstract` 메소드에 코드 내용을 작성하면 오류가 발생한다.

```java
abstract public String fail() {
    return "Good Bye";
}
```

`abstract`로 선언된 메소드는 본문을 가질 수 없기 때문이다.

---

### 추상 클래스(abstract class)

추상 클래스는 `abstract`로 선언된 클래스이다.

추상 클래스는 다음 두 가지 경우가 있다.

```text
추상 메소드를 가지고 있고 abstract로 선언된 클래스
추상 메소드가 없어도 abstract로 선언한 클래스
```

추상 메소드를 가진 클래스 예시는 다음과 같다.

```java
abstract class Shape {
    public Shape() {

    }

    public void edit() {

    }

    abstract public void draw();
}
```

위 코드에서 `draw()`는 코드 내용이 없는 추상 메소드이다.  
따라서 `Shape` 클래스는 `abstract class`로 선언해야 한다.

추상 메소드가 없는 추상 클래스도 만들 수 있다.

```java
abstract class JComponent {
    String name;

    public void load(String name) {
        this.name = name;
    }
}
```

이 클래스는 추상 메소드는 없지만 `abstract`로 선언되어 있으므로 추상 클래스이다.

---

### 추상 메소드 작성 시 주의할 점

추상 메소드가 하나라도 있으면 그 클래스는 반드시 추상 클래스로 선언해야 한다.

잘못된 예:

```java
class Fault {
    abstract public void f();
}
```

이 코드는 오류가 발생한다.  
`f()`가 추상 메소드인데 `Fault` 클래스가 `abstract class`로 선언되어 있지 않기 때문이다.

올바른 예:

```java
abstract class Fault {
    abstract public void f();
}
```

정리하면 추상 메소드는 자식 클래스에서 구현하도록 남겨둔 메소드이고, 그 메소드를 가진 클래스는 반드시 추상 클래스로 선언해야 한다.

---

## 20. 추상 클래스의 상속과 구현

추상 클래스를 상속받으면 서브 클래스도 추상 클래스가 될 수 있다.

특히 슈퍼 클래스에 추상 메소드가 있는데, 서브 클래스에서 그 메소드를 구현하지 않으면 서브 클래스도 `abstract`로 선언해야 한다.

### 추상 클래스 상속

```java
abstract class A {
    abstract public int add(int x, int y);
}

abstract class B extends A {
    public void show() {
        System.out.println("B");
    }
}
```

위 코드에서 `A`는 추상 클래스이고 `add()`라는 추상 메소드를 가지고 있다.

`B`는 `A`를 상속받았지만 `add()`를 구현하지 않았다.  
그래서 `B`도 반드시 추상 클래스로 선언해야 한다.

```java
abstract class B extends A
```

추상 클래스는 객체를 직접 만들 수 없다.

```java
A a = new A(); // 컴파일 오류
B b = new B(); // 컴파일 오류
```

`A`와 `B`는 모두 추상 클래스이므로 인스턴스를 생성할 수 없다.

---

### 추상 클래스 구현

추상 클래스를 상속받은 서브 클래스가 슈퍼 클래스의 추상 메소드를 구현하면 일반 클래스로 만들 수 있다.

```java
class C extends A {
    public int add(int x, int y) {
        return x + y;
    }

    public void show() {
        System.out.println("C");
    }
}
```

위 코드에서 `C`는 `A`의 추상 메소드인 `add()`를 구현했다.

```java
public int add(int x, int y) {
    return x + y;
}
```

이렇게 추상 메소드를 구현한 클래스는 더 이상 추상 클래스가 아니다.  
그래서 객체를 만들 수 있다.

```java
C c = new C(); // 정상
```

정리하면 추상 클래스를 상속받았을 때 선택지는 두 가지이다.

```text
추상 메소드를 구현하지 않으면 서브 클래스도 abstract로 선언
추상 메소드를 구현하면 일반 클래스로 사용 가능
```
# Java 패키지와 모듈 정리

## 1. 자바의 패키지와 모듈이란?

이번 주제는 Java의 패키지와 모듈이다.

패키지는 서로 관련된 클래스와 인터페이스를 모아 놓는 단위이고, 모듈은 여러 패키지와 이미지 등의 자원을 모아 놓는 더 큰 단위이다.

---

## 2. 패키지(package)

패키지는 서로 관련된 클래스와 인터페이스를 컴파일한 클래스 파일들을 묶어 놓은 디렉터리이다.

하나의 응용프로그램은 한 개 이상의 패키지로 작성할 수 있다.

또한 패키지는 `.jar` 파일로 압축할 수 있다.

```text
패키지 = 관련 있는 클래스와 인터페이스를 묶어 놓은 디렉터리
```

예를 들어 `com.example`이라는 패키지를 사용하면 보통 폴더 구조는 다음과 같이 만들어진다.

```text
com
└─ example
   ├─ Student.class
   ├─ Person.class
   └─ Main.class
```

Java 코드에서는 파일 맨 위에 `package`문을 사용해서 이 클래스가 어떤 패키지에 속하는지 표시한다.

```java
package com.example;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

패키지를 사용하면 클래스들을 주제별로 정리할 수 있고, 같은 이름의 클래스가 충돌하는 것도 줄일 수 있다.

---

## 3. 모듈(module)

모듈은 여러 패키지와 이미지 등의 자원을 모아 놓은 컨테이너이다.

패키지가 클래스들을 묶는 단위라면, 모듈은 패키지들을 다시 한 번 묶는 더 큰 단위라고 볼 수 있다.

```text
모듈 = 여러 패키지와 자원을 모아 놓은 컨테이너
```

하나의 모듈은 하나의 `.jmod` 파일에 저장할 수 있다.

모듈 안에는 다음과 같은 것들이 들어갈 수 있다.

```text
여러 패키지
이미지 같은 자원 파일
모듈 정보
```

---

## 4. Java 9부터 모듈화 도입

Java 9부터 모듈화 개념이 도입되었다.

기존에는 Java API의 많은 클래스들이 패키지 중심으로 구성되어 있었지만, Java 9부터는 Java 실행 환경의 클래스들이 모듈 기반으로 재구성되었다.

슬라이드에서는 이것을 플랫폼의 모듈화라고 설명한다.

```text
Java 9부터 자바 API의 모든 클래스들이 모듈 기반으로 재구성됨
```

응용프로그램도 모듈화할 수 있다.

기본 흐름은 다음과 같다.

```text
클래스들을 패키지로 만듦
패키지를 다시 모듈로 만듦
```

다만 모듈 프로그래밍은 기존 방식보다 더 어렵고 복잡할 수 있다.  
그래서 일반적인 Java 기초 단계에서는 패키지 개념을 먼저 이해하는 것이 중요하다.

---

## 5. 자바 모듈화의 목적

Java에서 모듈화를 도입한 목적은 필요한 모듈만 사용해서 실행 환경을 구성하기 위해서이다.

Java 8까지는 `rt.jar`라는 하나의 파일에 많은 Java API가 저장되어 있었다.  
하지만 Java 9부터는 Java API를 여러 모듈로 분리하였다.

슬라이드에서는 Java 9부터 Java API를 여러 모듈로 분할했다고 설명한다.

```text
Java 8까지: rt.jar 한 파일에 모든 API 저장
Java 9부터: Java API를 여러 모듈로 분리
```

현재는 여러 모듈로 정리되어 있으며, 필요한 모듈만 골라 사용할 수 있다.

---

## 6. 필요한 모듈만 사용

모듈화를 하면 응용프로그램이 실행될 때 꼭 필요한 모듈만으로 실행 환경을 구성할 수 있다.

이렇게 하면 메모리 자원이 약한 작은 기기에서도 필요한 기능만 포함한 작은 크기의 실행 이미지를 만들 수 있다.

```text
필요한 모듈만 포함
실행 이미지 크기 감소
메모리 사용 감소
작은 기기에서도 실행 가능
```

즉, 모듈화는 Java 실행 환경을 더 가볍게 구성하기 위한 목적도 가지고 있다.

---

## 7. 모듈의 현실

모듈은 Java 9부터 본격적으로 도입되었지만, 처음 배우는 입장에서는 꽤 복잡한 개념이다.

슬라이드에서는 모듈의 현실을 다음처럼 설명한다.

```text
Java 9부터 전면적으로 도입
복잡한 개념
큰 Java 응용프로그램에는 개발, 유지보수 등에 적합
현실적으로 모듈로 나누어 Java 프로그램을 작성할 필요는 많지 않음
```

즉, 모듈은 대규모 프로젝트에서는 유용하지만 작은 예제나 기초 Java 프로그램에서는 꼭 사용할 필요가 많지는 않다.

그래서 지금 단계에서는 모듈을 직접 작성하는 것보다, 패키지와 모듈이 어떤 역할을 하는지 이해하는 것이 더 중요하다.

---

## 8. 모듈화 정리

모듈화 작업은 중요한 개념이고, 특히 큰 프로젝트에서 효과가 있다.

처음부터 모듈 구조를 잘 잡으면 나중에 프로젝트가 커졌을 때 관리하기 쉽다.

```text
작은 프로젝트: 패키지 중심으로 작성해도 충분함
큰 프로젝트: 모듈화를 적용하면 관리와 유지보수에 도움됨
```

따라서 모듈은 당장 자주 사용하지 않더라도 Java 구조를 이해하기 위해 알아두는 것이 좋다.


---

## 9. 자바 API의 모듈 파일들

Java 9부터 Java API는 모듈 단위로 구성되었다.

보통 Java 모듈 파일들은 `jmods` 디렉터리에서 확인할 수 있다.  
하지만 사용하는 OpenJDK 종류나 버전에 따라 `jmods` 디렉터리가 없을 수도 있다.

슬라이드에서는 Temurin OpenJDK 24 기준으로 `jmods` 디렉터리가 사라진 이유를 설명한다.

---

## 10. Temurin OpenJDK 24와 jmods 디렉터리

Temurin OpenJDK 24부터는 JEP 493 표준을 따르게 되어 `jmods` 디렉터리가 포함되지 않는다고 한다.

그래서 Temurin OpenJDK 24를 설치했을 때 기존처럼 `jmods` 폴더가 보이지 않을 수 있다.

```text
Temurin OpenJDK 24부터 jmods 디렉터리가 포함되지 않을 수 있음
```

이것은 오류라기보다는 JDK 배포 방식이 바뀐 것으로 보면 된다.

---

## 11. jlink와 실행 이미지

Temurin의 `jlink` 도구를 사용하면 JMOD 파일을 직접 사용하지 않아도 사용자가 지정한 런타임 이미지를 만들 수 있다.

이렇게 필요한 모듈만 포함한 실행 이미지를 만들면 JDK 크기를 줄일 수 있다.

슬라이드에서는 JDK 크기를 약 25% 줄일 수 있다고 설명한다.

```text
jlink 사용
필요한 런타임 이미지 생성
JDK 크기 감소
```

Temurin OpenJDK 24에서는 빌드할 때 이 기능이 기본적으로 활성화된다고 한다.

---

## 12. jmods 파일 확인

만약 `jmods` 파일을 직접 확인하고 싶다면 다른 OpenJDK 배포판을 확인해 볼 수 있다.

OpenJDK 배포판마다 포함되는 파일 구조가 조금씩 다를 수 있기 때문이다.

```text
JDK 버전과 배포판에 따라 jmods 디렉터리 유무가 달라질 수 있음
```

따라서 `jmods` 폴더가 없다고 해서 Java 설치가 잘못된 것은 아니다.  
현재 사용하는 JDK의 배포 방식에 따라 달라질 수 있다.
