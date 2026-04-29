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

        // 처음 수 보다 끝 수가 작다면 두 수 교환
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
# Java 9주차 상속 정리

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

### 접근 지정자 정리

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

```java
class Child extends Parent {
    Child() {
        // super(); 가 자동으로 들어간다
        System.out.println("자식 생성자");
    }
}
```

하지만 부모 클래스에 기본 생성자가 없으면 오류가 발생한다.
이 경우에는 반드시 `super(값)` 형태로 부모 생성자를 직접 호출해야 한다.

---

## 6. Java 파일 실행 방법

Java 파일을 실행하려면 먼저 컴파일을 해야 한다.

```powershell
javac Midterm.java
```

컴파일에 성공하면 `.class` 파일이 생성된다.

```text
Midterm.java   : 내가 작성한 소스 코드 파일
Midterm.class  : 컴파일된 실행 파일
```

실행은 다음 명령어로 한다.

```powershell
java Midterm
```

주의할 점은 클래스 이름과 파일 이름이 같아야 한다는 것이다.

```java
public class Midterm
```

위처럼 클래스 이름이 `Midterm`이면 파일 이름은 반드시 다음과 같아야 한다.

```text
Midterm.java
```

---

## 7. 오류 정리

### ClassNotFoundException 오류

```text
Error: Could not find or load main class Midterm
```

이 오류는 보통 실행 위치가 잘못되었거나 클래스 이름을 잘못 입력했을 때 발생한다.

해결 방법:

```powershell
cd C:\Java2\Midterm
javac Midterm.java
java Midterm
```

---

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

## 8. 핵심 요약

상속은 부모 클래스의 멤버를 자식 클래스가 물려받는 것이다.

`private` 멤버는 자식 클래스에서도 직접 접근할 수 없다.

`public` 멤버는 어디서든 접근할 수 있다.

`protected` 멤버는 같은 패키지이거나 상속 관계일 때 접근할 수 있다.

자식 객체가 생성될 때는 부모 생성자가 먼저 실행되고, 그 다음 자식 생성자가 실행된다.

`super()`는 부모 생성자를 직접 호출할 때 사용한다.

Java 파일은 `javac`로 컴파일한 뒤 `java`로 실행한다.
