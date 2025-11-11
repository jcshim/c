좋아요! 완전 초보도 이해하기 쉬운 **체이닝(연결 리스트) 방식 해시 테이블**의 최소 예제입니다.
키는 문자열, 값은 정수 하나로 하고, 버킷 수는 고정(리사이징 없음)으로 갑니다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

#define BUCKETS 8   // 해시 테이블 칸(버킷) 개수 - 작게 고정

/* 연결 리스트 노드: (key, value) 한 쌍 */
typedef struct Node {
    char *key;
    int   value;
    struct Node *next;
} Node;

/* 해시 테이블: 각 버킷은 Node* (연결 리스트의 머리) */
typedef struct {
    Node *b[BUCKETS];
} HashTable;

/* strdup 의존 피하려고 직접 구현 (윈도우/리눅스 어디서나 동작) */
static char* my_strdup(const char *s) {
    size_t n = strlen(s) + 1;
    char *p = (char*)malloc(n);
    if (!p) { perror("malloc"); exit(1); }
    memcpy(p, s, n);
    return p;
}

/* 아주 단순한 해시 함수: 모든 문자 코드의 합을 BUCKETS로 나눈 나머지 */
static unsigned hash_str(const char *s) {
    unsigned h = 0;
    for (const unsigned char *p = (const unsigned char*)s; *p; ++p)
        h += *p;
    return h % BUCKETS;
}

/* 해시 테이블 초기화 (모든 버킷을 NULL로) */
static void ht_init(HashTable *ht) {
    for (int i = 0; i < BUCKETS; ++i) ht->b[i] = NULL;
}

/* put: 키가 이미 있으면 값 업데이트, 없으면 앞쪽에 새 노드 삽입(체이닝) */
static void ht_put(HashTable *ht, const char *key, int value) {
    unsigned i = hash_str(key);
    for (Node *n = ht->b[i]; n; n = n->next) {
        if (strcmp(n->key, key) == 0) {   // 이미 있으면 값만 바꿈
            n->value = value;
            return;
        }
    }
    // 없으면 새 노드 만들어 버킷 앞쪽에 연결
    Node *n = (Node*)malloc(sizeof(Node));
    if (!n) { perror("malloc"); exit(1); }
    n->key   = my_strdup(key);
    n->value = value;
    n->next  = ht->b[i];
    ht->b[i] = n;
}

/* get: 키가 있으면 out에 값 넣고 true, 없으면 false */
static bool ht_get(const HashTable *ht, const char *key, int *out) {
    unsigned i = hash_str(key);
    for (Node *n = ht->b[i]; n; n = n->next) {
        if (strcmp(n->key, key) == 0) {
            if (out) *out = n->value;
            return true;
        }
    }
    return false;
}

/* (옵션) del: 키가 있으면 삭제하고 true */
static bool ht_del(HashTable *ht, const char *key) {
    unsigned i = hash_str(key);
    Node **pp = &ht->b[i];        // 이전 포인터 기법
    while (*pp) {
        Node *n = *pp;
        if (strcmp(n->key, key) == 0) {
            *pp = n->next;        // 리스트에서 빼고
            free(n->key);
            free(n);              // 메모리 해제
            return true;
        }
        pp = &n->next;
    }
    return false;
}

/* (옵션) 테이블 상태 출력: 체이닝 리스트가 보이게 */
static void ht_dump(const HashTable *ht) {
    for (int i = 0; i < BUCKETS; ++i) {
        printf("[%d] ", i);
        for (Node *n = ht->b[i]; n; n = n->next) {
            printf("(%s:%d)%s", n->key, n->value, n->next ? " -> " : "");
        }
        puts("");
    }
}

/* 모든 노드 메모리 해제 */
static void ht_free(HashTable *ht) {
    for (int i = 0; i < BUCKETS; ++i) {
        Node *n = ht->b[i];
        while (n) {
            Node *next = n->next;
            free(n->key);
            free(n);
            n = next;
        }
        ht->b[i] = NULL;
    }
}

/* === 데모 === */
int main(void) {
    HashTable ht;
    ht_init(&ht);

    // 일부러 충돌이 나도록 몇 개 넣어보기
    ht_put(&ht, "Tom",   90);
    ht_put(&ht, "Amy",   80);
    ht_put(&ht, "Bob",   70);
    ht_put(&ht, "Jane",  95);
    ht_put(&ht, "Mona",  88);
    ht_put(&ht, "Tom",   99);   // 같은 키 업데이트

    int v;
    if (ht_get(&ht, "Tom", &v))   printf("Tom  = %d\n", v);
    if (ht_get(&ht, "Jane", &v))  printf("Jane = %d\n", v);
    if (!ht_get(&ht, "Kate", &v)) printf("Kate not found\n");

    ht_del(&ht, "Bob");

    puts("\n=== DUMP ===");
    ht_dump(&ht);

    ht_free(&ht);
    return 0;
}
```

### 설명 요약

* **해시 함수**로 버킷 인덱스(`0 ~ BUCKETS-1`)를 구합니다.
* 같은 버킷으로 모인 것들은 **연결 리스트(체이닝)** 로 이어붙입니다.
* `ht_put`은 **이미 있는 키면 값만 변경**, 없으면 **앞쪽에 노드 삽입**합니다.
* `ht_get`은 해당 버킷의 **연결 리스트를 순회하며** 키를 찾습니다.
* `ht_del`은 **이전 포인터 기법**으로 리스트에서 노드를 빼고 해제합니다.
* 실전에서는 **버킷 수를 더 크게** 하고, **로드 팩터**가 커지면 **리사이징**을 합니다(여긴 교육용 최소 버전).

필요하면 이 코드에 **키를 정수/문자 하나로 바꾼 초간단 버전**이나,
**개방 주소법(선형 조사)** 버전도 같은 난이도로 만들어 드릴게요!
