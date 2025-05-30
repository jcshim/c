## 🔷 스택이란1?

* **"나중에 넣은 것이 먼저 나온다(LIFO)"** 구조
* 예: 접시 쌓기 → 마지막에 올린 접시가 제일 먼저 빠짐

---

## 🔷 스택 구조 (배열로 구현)

```c
int stack[SIZE];  // 데이터를 저장할 배열
int top = -1;     // 스택의 맨 위 위치
```

* `top`은 스택의 현재 위치를 나타냄
* 처음에는 `-1` → 아무것도 없음

---

## 🔷 push: 데이터를 넣기

```c
void push(int value) {
    stack[++top] = value;
}
```

* `top`을 1 증가시키고 값을 넣음

---

## 🔷 pop: 데이터를 꺼내기

```c
int pop() {
    return stack[top--];
}
```

* 현재 `top` 값을 꺼내고 `top`을 1 줄임

---

## 🔷 실행 예

```c
push(10);      // 스택: [10]
push(20);      // 스택: [10, 20]
pop();         // 20 꺼냄 → 스택: [10]
pop();         // 10 꺼냄 → 스택: []
```

---

## 🔚 요약

* 스택 = 나중에 넣은 걸 먼저 꺼냄
* `push`: 넣기
* `pop`: 꺼내기
* 배열과 `top` 변수만으로 쉽게 구현 가능!

---

## 🔷 스택(Stack)이란 2?

* **후입선출(LIFO)** 구조: 나중에 들어온 데이터가 먼저 나간다.
* **예시**: 책 더미, 접시 쌓기
  → 마지막에 올린 접시가 가장 먼저 꺼내진다.

---

## 🔷 스택에서 주로 쓰는 동작

| 동작          | 설명                       |
| ----------- | ------------------------ |
| `push()`    | 데이터를 스택에 넣는다             |
| `pop()`     | 데이터를 스택에서 꺼낸다            |
| `peek()`    | 스택의 맨 위 데이터를 확인 (꺼내진 않음) |
| `isEmpty()` | 스택이 비었는지 확인              |

---

## ✅ 배열로 구현한 간단한 C언어 스택 예제

```c
#include <stdio.h>
#define SIZE 5  // 스택 최대 크기

int stack[SIZE];  // 스택 배열
int top = -1;     // 스택의 꼭대기 위치 (초기에는 비어있음)

// push 함수
void push(int value) {
    if (top == SIZE - 1) {
        printf("스택이 꽉 찼습니다!\n");
    } else {
        stack[++top] = value;
        printf("%d를 스택에 넣었습니다.\n", value);
    }
}

// pop 함수
int pop() {
    if (top == -1) {
        printf("스택이 비었습니다!\n");
        return -1;
    } else {
        return stack[top--];
    }
}

// peek 함수
int peek() {
    if (top == -1) {
        printf("스택이 비었습니다!\n");
        return -1;
    } else {
        return stack[top];
    }
}

// main 함수
int main() {
    push(10);
    push(20);
    printf("스택 맨 위 값: %d\n", peek());
    printf("꺼낸 값: %d\n", pop());
    printf("꺼낸 값: %d\n", pop());
    printf("꺼낸 값: %d\n", pop());  // 스택 비었을 때

    return 0;
}
```

---

## 📌 핵심 요약

* `top`은 스택의 꼭대기 위치를 기억하는 변수
* `push`는 `top++` 후에 배열에 값을 넣음
* `pop`은 현재 `top` 값을 꺼내고 `top--`함
* 스택이 비었거나 가득 찼을 때의 예외처리 중요
