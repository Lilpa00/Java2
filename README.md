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
