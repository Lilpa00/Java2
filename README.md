# 이름: 이재인, 학번: 202330121

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
