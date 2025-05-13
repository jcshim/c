# sizeof` 연산자
C 언어에서 **변수나 자료형의 크기(바이트 단위)** 

---

## ✅ 기본 문법

```c
sizeof(자료형)
sizeof(변수명)
```

* `sizeof(int)` → `int` 자료형의 바이트 수 반환
* `sizeof(x)` → 변수 `x`의 자료형에 따른 바이트 수 반환

> 🔸 주의: 괄호는 **자료형일 때만 필수**, 변수는 생략 가능 (`sizeof x`)

---

## ✅ 예제

```c
#include <stdio.h>

int main() {
    int a = 10;
    double b = 3.14;
    char c = 'A';

    printf("int: %lu bytes\n", sizeof(int));     // 자료형 크기
    printf("a: %lu bytes\n", sizeof(a));         // 변수 크기
    printf("double: %lu bytes\n", sizeof b);     // 괄호 생략 가능
    printf("char: %lu bytes\n", sizeof(char));
    
    return 0;
}
```

> 출력 결과 (컴파일러 및 시스템에 따라 다를 수 있음):

```
int: 4 bytes
a: 4 bytes
double: 8 bytes
char: 1 bytes
```

---

## ✅ 배열과 `sizeof`

```c
int arr[10];
printf("%lu\n", sizeof(arr));       // 10 * sizeof(int) = 40 (예시)
printf("%lu\n", sizeof(arr[0]));    // 4
```

> 🔸 배열의 전체 바이트 수를 구할 때 자주 사용합니다:

```c
int length = sizeof(arr) / sizeof(arr[0]);
```

---

## ✅ 포인터와 `sizeof`

```c
int *ptr;
printf("%lu\n", sizeof(ptr));       // 일반적으로 4 또는 8 (시스템에 따라 다름)
```

> 포인터 자체의 크기이지, **가리키는 값의 크기 아님**

---

## ✅ 특징 요약

| 특징       | 설명                       |
| -------- | ------------------------ |
| 연산 시점    | **컴파일 타임**에 결정됨          |
| 괄호       | **자료형에는 필요**, 변수에는 생략 가능 |
| 반환값      | `size_t` (부호 없는 정수형)     |
| 자주 쓰는 용도 | 배열 크기 계산, 메모리 할당 크기 지정 등 |
