
# 🌟 구조체(struct)
---

### 1. 구조체란?

📌 **설명**:
여러 데이터를 하나로 묶고 싶을 때 사용하는 사용자 정의 자료형.
`int`나 `float` 하나만으로는 학생 한 명을 표현하기 어려워요!

```c
#include <stdio.h>

struct Student {
    int id;
    float grade;
};

int main() {
    struct Student me = { 1001, 87.5};
    printf("ID: %d, 점수: %.1f\n", me.id, me.grade);
    return 0;
}
```

---

### 2. 구조체 선언과 사용

📌 **설명**:
`struct`로 구조를 만들고, 그걸로 변수를 선언해서 사용합니다.

```c
#include <stdio.h>

struct Student {
    int id;
    float grade;
};

int main() {
    struct Student me;
    me.id =  1002;
    me.grade = 91.0;
    printf("ID: %d, 점수: %.1f\n", me.id, me.grade);
    return 0;
}
```

---

### 3. 멤버 접근과 초기화

📌 **설명**:
`.`으로 멤버에 접근, 선언과 동시에 초기화도 가능!

```c
#include <stdio.h>

struct Student {
    int id;
    float grade;
};

int main() {
    struct Student me = { 1003, 75.5};
    printf("%d번 학생의 점수는 %.1f입니다.\n", me.id, me.grade);
    return 0;
}
```

---

### 4. 구조체 배열

📌 **설명**:
같은 구조의 여러 데이터를 배열로 관리!

```c
#include <stdio.h>

struct Student {
    int id;
    float grade;
};

int main() {
    struct Student list[2] = {{ 1004, 88.0}, { 1005, 91.0}};
    for (int i = 0; i < 2; i++) {
        printf("%d번 학생: %.1f점\n", list[i].id, list[i].grade);
    }
    return 0;
}
```

---

### 5. 구조체와 함수

📌 **설명**:
구조체를 함수에 넘겨서 처리 가능!

```c
#include <stdio.h>

struct Student {
    int id;
    float grade;
};

void show(struct Student me) {
    printf("ID: %d, 점수: %.1f\n", me.id, me.grade);
}

int main() {
    struct Student me = { 1006, 84.0};
    show(me);
    return 0;
}
```

---

### 6. 중첩 구조체

📌 **설명**:
구조체 안에 또 다른 구조체도 멤버로 포함 가능!

```c
#include <stdio.h>

struct Name {
    char first[20];
    char last[20];
};

struct Student {
    int id;
    struct Name name;
    float grade;
};

int main() {
    struct Student st = { 1007, {"Min", "Kim"}, 95.0};
    printf("%s %s의 점수: %.1f\n", st.name.first, st.name.last, st.grade);
    return 0;
}
```

---

### 7. 구조체와 포인터

📌 **설명**:
포인터로 구조체를 동적으로 관리할 수 있어요. `->`로 접근!

```c
#include <stdio.h>
#include <stdlib.h>

struct Student {
    int id;
    float grade;
};

int main() {
    struct Student* me = malloc(sizeof(struct Student));
    me->id =  1008;
    me->grade = 78.5;
    printf("ID: %d, 점수: %.1f\n", me->id, me->grade);
    free(me);
    return 0;
}
```

---

### 8. typedef로 구조체 간단히

📌 **설명**:
`struct` 생략하고 간단하게 쓰는 방법!

```c
#include <stdio.h>

typedef struct {
    int id;
    float grade;
} Student;

int main() {
    Student me = { 1009, 89.0};
    printf("학번: %d, 점수: %.1f\n", me.id, me.grade);
    return 0;
}
```

---

### 9. 응용 예제 – 학생 리스트 출력

📌 **설명**:
함수와 배열을 조합해서 학생 정보 전체 관리 가능!

```c
#include <stdio.h>

typedef struct {
    int id;
    float grade;
} Student;

void show(Student s) {
    printf("학번: %d, 점수: %.1f\n", s.id, s.grade);
}

int main() {
    Student list[3] = {{010, 85.5}, {011, 92.0}, {012, 77.5}};
    for (int i = 0; i < 3; i++) {
        show(list[i]);
    }
    return 0;
}
```

---

## 📌 요약

| 항목      | 개념 요약                       |
| ------- | --------------------------- |
| 구조체     | 여러 데이터 묶는 사용자 정의 자료형        |
| 멤버 접근   | `.` 연산자 (`->`는 포인터용)        |
| 배열      | `struct A arr[10];`처럼 사용 가능 |
| 함수 전달   | 값 복사 혹은 포인터 전달              |
| 중첩 구조체  | 구조체 안에 구조체 가능               |
| typedef | `struct` 없이도 선언 가능          |

---
