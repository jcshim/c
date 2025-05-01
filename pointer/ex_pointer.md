## ✅ C 언어 포인터 초보자 필수 9가지 예제

---

### 🔹 1. 포인터 선언과 주소 저장

```c
#include <stdio.h>

int main() {
    int a = 10;
    int* p = &a;

    printf("a의 값: %d\n", a);
    printf("a의 주소: %p\n", &a);
    printf("p가 가리키는 주소: %p\n", p);
    printf("p가 가리키는 값: %d\n", *p);

    return 0;
}
```

📌 **핵심**: `*p`는 포인터가 가리키는 주소의 값을 읽는다.

---

### 🔹 2. 포인터로 값 변경하기

```c
#include <stdio.h>

int main() {
    int a = 5;
    int* p = &a;

    *p = 100;

    printf("a의 값: %d\n", a);
    return 0;
}
```

📌 **핵심**: 포인터를 통해 원래 변수의 값을 바꿀 수 있다.

---

### 🔹 3. 배열과 포인터

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

📌 **핵심**: 배열 이름은 주소이고, 포인터처럼 동작한다.

---

### 🔹 4. 함수에 포인터 전달

```c
#include <stdio.h>

void changeValue(int* p) {
    *p = 999;
}

int main() {
    int x = 10;
    changeValue(&x);
    printf("x의 값: %d\n", x);
    return 0;
}
```

📌 **핵심**: 포인터로 함수에 값을 전달하면 원본이 바뀐다.

---

### 🔹 5. 포인터 간 값 복사

```c
#include <stdio.h>

int main() {
    int a = 3, b = 4;
    int* p1 = &a;
    int* p2 = &b;

    *p1 = *p2;
    printf("a: %d, b: %d\n", a, b);

    return 0;
}
```

📌 **핵심**: `*p1 = *p2;`는 값 복사, 주소 복사가 아님.

---

### 🔹 6. 동적 메모리 할당 (`malloc`)

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int* p = (int*)malloc(sizeof(int));
    if (p == NULL) {
        printf("메모리 할당 실패\n");
        return 1;
    }

    *p = 42;
    printf("동적 메모리 값: %d\n", *p);

    free(p);
    return 0;
}
```

📌 **핵심**: `malloc`은 메모리를 직접 확보하고 `free`로 반납해야 한다.

---

### 🔹 7. 이중 포인터 (`int**`)

```c
#include <stdio.h>

int main() {
    int a = 10;
    int* p = &a;
    int** pp = &p;

    printf("a: %d\n", a);
    printf("*p: %d\n", *p);
    printf("**pp: %d\n", **pp);

    return 0;
}
```

📌 **핵심**: 포인터를 가리키는 포인터도 가능하다 (`**pp`).

---

### 🔹 8. 포인터로 문자열 다루기

```c
#include <stdio.h>

int main() {
    char* str = "Hello, world!";
    printf("%s\n", str);
    printf("%c\n", str[1]);
    printf("%c\n", *(str + 7));

    return 0;
}
```

📌 **핵심**: 문자열은 `char*` 타입이며, 배열처럼 인덱싱도 가능하다.

---

### 🔹 9. 포인터 배열 (문자열 배열)

```c
#include <stdio.h>

int main() {
    char* fruits[3] = {"Apple", "Banana", "Cherry"};

    for (int i = 0; i < 3; i++) {
        printf("%s\n", fruits[i]);
        printf("%c\n", fruits[i][0]);
    }

    return 0;
}
```

📌 **핵심**: `char*` 배열은 여러 문자열을 가리키는 포인터 집합이다.

---

## ✅ 총정리 – C 포인터의 9가지 핵심 사용법

| 번호 | 주제                    | 요약 설명                                 |
|------|-------------------------|--------------------------------------------|
| 1    | 포인터 선언 및 주소     | 변수의 주소 저장, `*`로 값 접근            |
| 2    | 포인터로 값 변경        | 포인터를 통해 변수 값 수정                |
| 3    | 배열과 포인터           | 배열 이름은 포인터처럼 작동                |
| 4    | 함수 인자로 포인터 전달 | Call by Reference 효과                     |
| 5    | 포인터 간 값 복사       | 주소 복사가 아닌 값 복사                  |
| 6    | 동적 메모리 할당        | `malloc`, `free`를 이용한 힙 메모리 관리   |
| 7    | 이중 포인터             | 포인터를 가리키는 포인터 (`int**`)        |
| 8    | 문자열과 포인터         | `char*`로 문자열 접근 및 조작             |
| 9    | 포인터 배열             | 문자열 배열, 다차원 포인터 응용           |

---
