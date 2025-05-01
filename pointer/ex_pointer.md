### ✅ 예제 1: 포인터의 선언과 주소 저장

```c
#include <stdio.h>

int main() {
    int a = 10;
    int* p = &a;  // p는 a의 주소를 저장하는 포인터

    printf("a의 값: %d\n", a);       // 10
    printf("a의 주소: %p\n", &a);    // 주소
    printf("p가 가리키는 주소: %p\n", p);   // a의 주소
    printf("p가 가리키는 값: %d\n", *p);    // 10

    return 0;
}
```

📌 **핵심**: `int* p`는 `int`형 변수의 주소를 저장하며, `*p`는 주소에 있는 값을 의미함.

---

### ✅ 예제 2: 포인터를 통한 값 변경

```c
#include <stdio.h>

int main() {
    int a = 5;
    int* p = &a;

    *p = 100;  // a의 값을 포인터를 통해 바꿈

    printf("a의 값: %d\n", a);  // 100
    return 0;
}
```

📌 **핵심**: `*p = 값;`은 해당 주소의 변수 값을 직접 바꾸는 것과 같음.

---

### ✅ 예제 3: 배열과 포인터

```c
#include <stdio.h>

int main() {
    int arr[3] = {1, 2, 3};
    int* p = arr;

    printf("%d\n", *p);       // 1
    printf("%d\n", *(p + 1)); // 2
    printf("%d\n", *(p + 2)); // 3

    return 0;
}
```

📌 **핵심**: 배열 이름은 포인터처럼 동작하며, `arr[i] == *(arr + i)`.

---

### ✅ 예제 4: 함수에서 포인터로 값 전달

```c
#include <stdio.h>

void changeValue(int* p) {
    *p = 999;
}

int main() {
    int x = 10;
    changeValue(&x);
    printf("x의 값: %d\n", x);  // 999
    return 0;
}
```

📌 **핵심**: 함수에 주소를 전달하면, 원래 변수의 값을 바꿀 수 있음 (**Call by reference** 효과).

---

### ✅ 예제 5: 포인터 간단 복습 퀴즈용

```c
#include <stdio.h>

int main() {
    int a = 3, b = 4;
    int* p1 = &a;
    int* p2 = &b;

    *p1 = *p2;
    printf("a: %d, b: %d\n", a, b);  // a: 4, b: 4

    return 0;
}
```

📌 **핵심**: `*p1 = *p2;` → `a = b;`와 같음. 즉, **값 복사**이지 **주소 복사 아님**.

---

각 예제를 머릿속에 그림처럼 기억하면, 포인터가 무서운 개념이 아니라 **메모리와 값의 연결을 직접 다루는 강력한 도구**라는 걸 알 수 있어요.

---

### ✅ 예제 6: `malloc`을 이용한 동적 메모리 할당

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int* p = (int*)malloc(sizeof(int));  // int 크기만큼 메모리 할당

    if (p == NULL) {
        printf("메모리 할당 실패\n");
        return 1;
    }

    *p = 42;  // 할당된 공간에 값 저장

    printf("동적 할당된 메모리의 값: %d\n", *p);  // 42

    free(p);  // 메모리 해제
    return 0;
}
```

---

### 🧠 핵심 요점 정리:

- `malloc(sizeof(int))`: 힙 영역에 int 하나 크기만큼 메모리를 요청.
- `p == NULL`: 메모리 할당 실패 여부 확인.
- `*p = 42`: 할당된 공간에 값 저장.
- `free(p)`: 사용 후 메모리 반납 안 하면 **메모리 누수** 발생.

---

이 예제는 특히 **배열을 동적으로 만들 때**, 구조체 동적 생성, 또는 유연한 메모리 관리에 꼭 필요합니다.

