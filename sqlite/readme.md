# ✅ SQLite를 C에서 사용하는 핵심 개념

SQLite는 **파일 기반(file-based) DB**입니다.
즉, 서버가 없고 `.db` 파일 하나가 **곧 데이터베이스 전체**입니다.

C에서 SQLite를 쓰려면 딱 이 3가지만 기억하면 됩니다:

1. **sqlite3_open()** — DB 파일 열기/생성
2. **sqlite3_exec()** — SQL 실행하기 (CREATE/INSERT/UPDATE)
3. **sqlite3_prepare, step, finalize** — SELECT 결과 읽기
4. **sqlite3_close()** — DB 닫기

---

# ✅ 1. SQLite 준비 (Visual Studio / Windows 기준)

### 🔹 방법 1: SQLite 공식 DLL/헤더 사용 (가장 일반적)

1. 아래 다운로드
   [https://www.sqlite.org/download.html](https://www.sqlite.org/download.html)

   * *sqlite-dll-win64-x64-xxxx.zip* → DLL
   * *sqlite-amalgamation-xxxx.zip* → sqlite3.h + sqlite3.c

2. 프로젝트에 포함

   * `sqlite3.h` → **소스 폴더**에 추가
   * `sqlite3.c` → **프로젝트에 직접 추가**
   * `sqlite3.dll` → 실행 파일과 같은 폴더에 복사

3. 컴파일하면 자동으로 SQLite가 링크됨
   → 별도의 라이브러리(.lib) 필요 없음.

---

# ✅ 2. 최소 예제: C에서 SQLite 접속 + 테이블 생성 + INSERT

이 코드를 그대로 복사해 컴파일하면 됩니다.

```c
#include <stdio.h>
#include "sqlite3.h"

int main(void) {
    sqlite3 *db;
    char *err = 0;

    // 1) DB 열기 (파일 없으면 자동 생성)
    if (sqlite3_open("test.db", &db)) {
        printf("DB Open Error: %s\n", sqlite3_errmsg(db));
        return 1;
    }

    // 2) SQL 한 번에 실행
    const char *sql = 
        "CREATE TABLE IF NOT EXISTS students ("
        " id INTEGER PRIMARY KEY AUTOINCREMENT,"
        " name TEXT,"
        " score INTEGER);"
        "INSERT INTO students (name, score) VALUES ('Kim', 90);";

    int rc = sqlite3_exec(db, sql, 0, 0, &err);

    if (rc != SQLITE_OK) {
        printf("SQL Error: %s\n", err);
        sqlite3_free(err);
    } else {
        printf("Query OK\n");
    }

    // 3) DB 닫기
    sqlite3_close(db);

    return 0;
}
```

---

# ✅ 3. SELECT 결과를 읽는 방법 (prepare → step → column)

예제: 학생 목록을 화면에 출력

```c
#include <stdio.h>
#include "sqlite3.h"

int main(void) {
    sqlite3 *db;
    sqlite3_stmt *stmt;

    sqlite3_open("test.db", &db);

    const char *sql = "SELECT id, name, score FROM students;";

    // 1) SQL 준비
    sqlite3_prepare_v2(db, sql, -1, &stmt, NULL);

    // 2) 결과 반복
    while (sqlite3_step(stmt) == SQLITE_ROW) {
        int id      = sqlite3_column_int(stmt, 0);
        const unsigned char *name = sqlite3_column_text(stmt, 1);
        int score   = sqlite3_column_int(stmt, 2);

        printf("%d | %s | %d\n", id, name, score);
    }

    // 3) 리소스 해제
    sqlite3_finalize(stmt);
    sqlite3_close(db);

    return 0;
}
```

---

# ✅ 4. INSERT / UPDATE / DELETE는 exec( )가 가장 쉽다

```c
char *err = 0;
sqlite3_exec(db,
    "UPDATE students SET score=95 WHERE id=1;",
    0, 0, &err);
```

에러 발생 시:

```c
printf("SQL Error: %s\n", err);
sqlite3_free(err);
```

---

# ✅ 5. 아주 중요한 팁

### ✔ SQLite는 스레드 안전하지만 파일 기반이라서 “동시 쓰기”는 약함

→ 교육/개인 프로젝트에는 최강
→ 대규모 서버에는 보통 PostgreSQL, MySQL 사용

### ✔ 파라미터 바인딩도 가능

SQL Injection 막기 위해서:

```c
sqlite3_prepare_v2(db, 
    "INSERT INTO students (name, score) VALUES (?, ?);",
    -1, &stmt, NULL);

sqlite3_bind_text(stmt, 1, "Lee", -1, SQLITE_STATIC);
sqlite3_bind_int(stmt, 2, 88);

sqlite3_step(stmt);
sqlite3_finalize(stmt);
```

---

# ✅ 6. 전체 흐름 요약(암기용)

```
sqlite3_open() → 연결
sqlite3_exec() → 단순 SQL 실행
sqlite3_prepare_v2() → SELECT 문 준비
sqlite3_step() → 결과 하나씩 읽기
sqlite3_column_int/text() → 값 꺼내기
sqlite3_finalize() → 정리
sqlite3_close() → 종료
```

---

# 📥 필요하면 다음도 만들어 드립니다

* VS2026 프로젝트 생성 전체 과정 step-by-step
* SQLite CRUD(생성/조회/수정/삭제) 샘플 전체 세트
* 헤더/소스 파일 구성 템플릿
* 학생 과제용 ‘SQLite 미니 프로젝트’ 기획안
* SQLite + Raylib + C GUI 예제
* SQLite 성적 관리 프로그램 예제

원하는 버전을 말해 주시면 바로 제작해 드립니다!
