### 1. 구조체란 무엇인가? – 개념과 필요성

> 구조체의 정의, 사용하는 이유(복합 데이터 표현), 배열/변수와의 차이점

**간단 설명:**
C언어의 기본 자료형(int, float 등)으로는 하나의 학생 정보를 통째로 표현하기 어렵다. 구조체는 서로 다른 타입의 데이터를 하나로 묶어 복합적인 정보를 표현할 수 있게 해준다.

**예제 코드:**

```c
#include <stdio.h>

struct Student {
    int id;
    float score;
};

int main() {
    struct Student s = {2023001, 87.5};
    printf("학번: %d, 성적: %.1f\n", s.id, s.score);
    return 0;
}
```

---

### 2. 구조체 선언과 정의 방법

> `struct` 키워드 사용법, 멤버 변수 선언, 구조체 변수 생성 방법

**간단 설명:**
구조체는 `struct` 키워드를 사용하여 정의하며, 이후 구조체 변수를 선언해 사용할 수 있다.

**예제 코드:**

```c
#include <stdio.h>

struct Student {
    int id;
    float score;
};

int main() {
    struct Student s;  // 선언
    s.id = 2023002;
    s.score = 92.0;
    printf("학번: %d, 성적: %.1f\n", s.id, s.score);
    return 0;
}
```

---

### 3. 구조체 멤버 접근과 초기화

> 점(.) 연산자를 이용한 접근, 선언 시 초기화 방법, 멤버 변수 사용 예제

**간단 설명:**
구조체 멤버는 점(.) 연산자를 통해 접근하며, 선언과 동시에 초기화할 수 있다.

**예제 코드:**

```c
#include <stdio.h>

struct Student {
    int id;
    float score;
};

int main() {
    struct Student s = {2023003, 75.5};
    printf("ID: %d, Score: %.1f\n", s.id, s.score);
    return 0;
}
```

---

### 4. 구조체 배열 – 동일한 구조체의 집합

> 구조체를 배열로 선언하는 방법, 반복문을 이용한 입력/출력 예제

**간단 설명:**
같은 형식의 구조체 데이터를 여러 개 저장하려면 배열을 사용할 수 있다.

**예제 코드:**

```c
#include <stdio.h>

struct Student {
    int id;
    float score;
};

int main() {
    struct Student list[2] = {{2023004, 88.0}, {2023005, 91.0}};
    for (int i = 0; i < 2; i++) {
        printf("학생 %d - ID: %d, 점수: %.1f\n", i+1, list[i].id, list[i].score);
    }
    return 0;
}
```

---

### 5. 구조체와 함수 – 구조체 전달 방식

> 함수 인자로 구조체 넘기기 (값에 의한 전달 vs 포인터에 의한 전달), 예제 실습

**간단 설명:**
구조체를 함수에 인자로 넘길 수 있으며, 복사본을 넘길지 주소를 넘길지에 따라 결과가 다르다.

**예제 코드:**

```c
#include <stdio.h>

struct Student {
    int id;
    float score;
};

void printStudent(struct Student s) {
    printf("ID: %d, 점수: %.1f\n", s.id, s.score);
}

int main() {
    struct Student s = {2023006, 84.0};
    printStudent(s);
    return 0;
}
```

---

### 6. 중첩 구조체 – 구조체 안에 구조체

> 구조체 멤버로 다른 구조체를 포함하는 방식, 계층적 데이터 모델링 실습

**간단 설명:**
구조체 안에 다른 구조체를 멤버로 선언할 수 있어 복잡한 정보를 표현할 수 있다.

**예제 코드:**

```c
#include <stdio.h>

struct Name {
    char first[20];
    char last[20];
};

struct Student {
    int id;
    struct Name name;
    float score;
};

int main() {
    struct Student s = {2023007, {"Min", "Kim"}, 95.0};
    printf("이름: %s %s, 점수: %.1f\n", s.name.first, s.name.last, s.score);
    return 0;
}
```

---

### 7. 구조체와 포인터 – 동적 메모리와 연계

> 구조체 포인터 선언, `->` 연산자 사용법, malloc/free와의 연계 실습

**간단 설명:**
구조체 포인터를 사용하면 동적으로 구조체를 생성하고 관리할 수 있으며, `->` 연산자로 멤버에 접근한다.

**예제 코드:**

```c
#include <stdio.h>
#include <stdlib.h>

struct Student {
    int id;
    float score;
};

int main() {
    struct Student* s = (struct Student*)malloc(sizeof(struct Student));
    s->id = 2023008;
    s->score = 78.5;
    printf("ID: %d, 점수: %.1f\n", s->id, s->score);
    free(s);
    return 0;
}
```

---

### 8. typedef와 구조체 – 코드 간결화

> `typedef`를 이용한 구조체 이름 단순화, 가독성 향상 사례

**간단 설명:**
`typedef`를 사용하면 구조체 이름을 간단히 정의할 수 있어 코드가 깔끔해진다.

**예제 코드:**

```c
#include <stdio.h>

typedef struct {
    int id;
    float score;
} Student;

int main() {
    Student s = {2023009, 89.0};
    printf("학번: %d, 점수: %.1f\n", s.id, s.score);
    return 0;
}
```

---

### 9. 구조체 응용 예제 – 학생 정보 관리 시스템

> 구조체를 활용한 간단한 실습 프로젝트 (ex: 학번, 이름, 평균 점수 관리)

**간단 설명:**
여러 명의 학생 데이터를 관리하는 간단한 정보 시스템을 만들 수 있다.

**예제 코드:**

```c
#include <stdio.h>

typedef struct {
    int id;
    float score;
} Student;

void printStudent(Student s) {
    printf("학번: %d, 점수: %.1f\n", s.id, s.score);
}

int main() {
    Student list[3] = {{2023010, 85.5}, {2023011, 92.0}, {2023012, 77.5}};
    for (int i = 0; i < 3; i++) {
        printStudent(list[i]);
    }
    return 0;
}
```

---
