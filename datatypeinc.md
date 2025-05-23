# c언어의 **데이터 형(Data Type)**: 

변수에 저장할 수 있는 **값의 종류와 크기**를 정의

---

### 🟦 1. 기본 데이터 형 (Primitive Types)

| 형식       | 설명                                         | 예시                        |
| -------- | ------------------------------------------ | ------------------------- |
| `int`    | 정수 (4바이트, -2,147,483,648 \~ 2,147,483,647) | `int age = 20;`           |
| `float`  | 실수 (4바이트, 소수점 약 6자리 정밀도)                   | `float pi = 3.14f;`       |
| `double` | 정밀 실수 (8바이트, 소수점 약 15자리)                   | `double pi = 3.14159265;` |
| `char`   | 문자 1개 (1바이트, 아스키 문자 저장)                    | `char grade = 'A';`       |

---

### 🟩 2. 수정자 (Modifiers)와 확장

* **`short`** / **`long`**: 크기를 줄이거나 늘림
  예: `short int`, `long int`, `long long int`

* **`unsigned`**: 음수를 제외하고 양수만 표현
  예: `unsigned int`, `unsigned char`

---

### 🟨 3. 기타 데이터 형

| 형식        | 설명                        |
| --------- | ------------------------- |
| `void`    | 반환값 없음 (함수에서 주로 사용)       |
| `enum`    | 열거형 (정수 상수 집합 정의)         |
| `struct`  | 구조체 (여러 자료형을 하나로 묶음)      |
| `union`   | 공용체 (하나의 메모리 공간에 여러 형 저장) |
| `typedef` | 사용자 정의 데이터형 별칭            |

---

### 📝 참고 사항

* 각 데이터형의 **크기(size)** 는 시스템(32bit/64bit) 및 컴파일러에 따라 다를 수 있습니다.
  → `sizeof()` 함수로 확인 가능
  예: `printf("%lu", sizeof(int));`

---

필요하시면 각 데이터형에 대한 예제 코드도 제공해드릴 수 있습니다!


---

## ✅ C언어 데이터 형의 분류

### 1. **기본형 (Primary / Fundamental Types)**

* `int`, `float`, `double`, `char`, `void` 등
* 우리가 흔히 아는 **단순한 값**을 표현

### 2. **파생형 (Derived Types)** ← 여기 포함됩니다!

* **배열 (array)**: 같은 형의 여러 데이터를 순차적으로 저장
  예: `int arr[5];` → `int`형 5개의 공간

* **포인터 (pointer)**: 특정 데이터의 **주소를 저장**하는 변수
  예: `int *p;` → `int`형 값을 가리키는 포인터

* **함수형 (function)**: 반환형과 매개변수로 구성된 형
  예: `int add(int, int);`

---

### 3. **사용자 정의형 (User-defined Types)**

* `struct`, `union`, `enum`, `typedef` 등
* 기본형과 파생형을 조합해서 **새로운 형식**을 만들 수 있음

---

## 📌 요약

| 분류      | 예시                          | 설명             |
| ------- | --------------------------- | -------------- |
| 기본형     | `int`, `char`, `float`      | 단일 값 저장        |
| 파생형     | `int*`, `char[]`, `float()` | 기본형에서 파생된 형    |
| 사용자 정의형 | `struct`, `union`, `enum`   | 복합 자료 구조 구성 가능 |

---
