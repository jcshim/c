// ds_minis.c : C 핵심 자료구조 5종 "최소 예제 + 간단 테스트"
// build: gcc -std=c99 -O2 ds_minis.c -o ds_minis
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>

/* =========================================
 * 1) 동적 배열 (Dynamic Array, int 전용 간이판)
 * ========================================= */
typedef struct {
    int *a;
    int n, cap;
} DynArr;

void da_init(DynArr *d) { d->a=NULL; d->n=0; d->cap=0; }
void da_reserve(DynArr *d, int need){
    if(d->cap >= need) return;
    int nc = d->cap ? d->cap*2 : 4;
    if(nc < need) nc = need;
    int *na = (int*)realloc(d->a, sizeof(int)*nc);
    assert(na);
    d->a = na; d->cap = nc;
}
void da_push_back(DynArr *d, int v){
    if(d->n == d->cap) da_reserve(d, d->n+1);
    d->a[d->n++] = v;
}
int  da_get(const DynArr *d, int i){ assert(0<=i && i<d->n); return d->a[i]; }
void da_set(DynArr *d, int i, int v){ assert(0<=i && i<d->n); d->a[i]=v; }
void da_free(DynArr *d){ free(d->a); d->a=NULL; d->n=d->cap=0; }

static void test_dynamic_array(void){
    DynArr d; da_init(&d);
    for(int i=1;i<=5;i++) da_push_back(&d, i); // [1,2,3,4,5]
    assert(d.n==5 && da_get(&d,0)==1 && da_get(&d,4)==5);

    da_set(&d,2,99); // [1,2,99,4,5]
    int sum=0; for(int i=0;i<d.n;i++) sum+=da_get(&d,i);
    assert(sum==(1+2+99+4+5));
    da_free(&d);
    puts("[1] Dynamic Array OK");
}

/* =========================================
 * 2) 단일 연결 리스트 (Singly Linked List)
 * ========================================= */
typedef struct Node {
    int x;
    struct Node* next;
} Node;

void list_push_front(Node **head, int v){
    Node *n = (Node*)malloc(sizeof *n);
    assert(n);
    n->x=v; n->next=*head; *head=n;
}
Node* list_find(Node *p, int key){
    for(; p; p=p->next) if(p->x==key) return p;
    return NULL;
}
void list_pop_front(Node **head){
    assert(head && *head);
    Node *t=*head; *head=(*head)->next; free(t);
}
void list_free(Node *p){ while(p){ Node* n=p->next; free(p); p=n; } }

static void test_singly_linked_list(void){
    Node *head=NULL;
    list_push_front(&head,3);
    list_push_front(&head,2);
    list_push_front(&head,1); // 1->2->3
    assert(head && head->x==1 && head->next->x==2);

    Node *f = list_find(head,2);
    assert(f && f->x==2);

    list_pop_front(&head); // drop 1 -> 2->3
    assert(head && head->x==2);

    list_free(head);
    puts("[2] Singly Linked List OK");
}

/* =========================================
 * 3) 스택 (배열 기반, 고정 용량 소형 예제)
 * ========================================= */
#define STACK_N 16
typedef struct {
    int a[STACK_N];
    int top; // a[top-1]가 맨 위, top==0이면 비어있음
} Stack;

void stk_init(Stack *s){ s->top=0; }
int  stk_empty(const Stack *s){ return s->top==0; }
void stk_push(Stack *s, int v){ assert(s->top<STACK_N); s->a[s->top++]=v; }
int  stk_pop(Stack *s){ assert(s->top>0); return s->a[--s->top]; }
int  stk_peek(const Stack *s){ assert(s->top>0); return s->a[s->top-1]; }

static void test_stack(void){
    Stack s; stk_init(&s);
    assert(stk_empty(&s));
    stk_push(&s,10); stk_push(&s,20); stk_push(&s,30);
    assert(stk_peek(&s)==30);
    assert(stk_pop(&s)==30);
    assert(stk_pop(&s)==20);
    assert(stk_pop(&s)==10);
    assert(stk_empty(&s));
    puts("[3] Stack (LIFO) OK");
}

/* =========================================
 * 4) 큐 (원형 큐, FIFO, 고정 용량 소형 예제)
 * ========================================= */
#define QUEUE_N 8
typedef struct {
    int a[QUEUE_N];
    int f, r, c; // front index, rear index, count
} Queue;

void que_init(Queue *q){ q->f=q->r=q->c=0; }
int  que_empty(const Queue *q){ return q->c==0; }
int  que_full (const Queue *q){ return q->c==QUEUE_N; }
void enq(Queue *q, int v){ assert(!que_full(q)); q->a[q->r]=v; q->r=(q->r+1)%QUEUE_N; q->c++; }
int  deq(Queue *q){ assert(!que_empty(q)); int v=q->a[q->f]; q->f=(q->f+1)%QUEUE_N; q->c--; return v; }
int  que_front(const Queue *q){ assert(!que_empty(q)); return q->a[q->f]; }

static void test_queue(void){
    Queue q; que_init(&q);
    enq(&q,1); enq(&q,2); enq(&q,3);
    assert(que_front(&q)==1);
    assert(deq(&q)==1); // [2,3]
    enq(&q,4); enq(&q,5);
    // wrap 테스트
    while(!que_empty(&q)) { static int expect=2; assert(deq(&q)==expect++); if(expect==6) expect=6; }
    puts("[4] Queue (FIFO) OK");
}

/* =========================================
 * 5) 해시 테이블 (체이닝, 문자열 키 -> int 값)
 *   - 교육용 최소판: 삭제/리사이즈 생략
 * ========================================= */
#define HM    101
typedef struct HNode {
    char *k; int v;
    struct HNode *next;
} HNode;

static HNode* gtab[HM];

unsigned hfunc(const char *s){
    unsigned h=0u;
    for(; *s; ++s) h = h*131u + (unsigned char)(*s);
    return h % HM;
}
char* str_dup(const char* s){
    size_t n=strlen(s)+1;
    char *p=(char*)malloc(n);
    assert(p);
    memcpy(p,s,n);
    return p;
}
void h_put(const char* k, int v){
    unsigned i = hfunc(k);
    for(HNode *p=gtab[i]; p; p=p->next){
        if(strcmp(p->k,k)==0){ p->v=v; return; }
    }
    HNode *n=(HNode*)malloc(sizeof *n); assert(n);
    n->k=str_dup(k); n->v=v; n->next=gtab[i]; gtab[i]=n;
}
HNode* h_get(const char* k){
    for(HNode *p=gtab[hfunc(k)]; p; p=p->next)
        if(strcmp(p->k,k)==0) return p;
    return NULL;
}
void h_free_all(void){
    for(int i=0;i<HM;i++){
        HNode *p=gtab[i];
        while(p){ HNode *n=p->next; free(p->k); free(p); p=n; }
        gtab[i]=NULL;
    }
}

static void test_hash_table(void){
    memset(gtab, 0, sizeof gtab);
    h_put("apple", 3);
    h_put("banana", 5);
    h_put("orange", 7);
    // 업데이트 테스트
    h_put("banana", 42);
    HNode *b = h_get("banana");
    HNode *a = h_get("apple");
    HNode *z = h_get("zzz");
    assert(b && b->v==42);
    assert(a && a->v==3);
    assert(z==NULL);
    h_free_all();
    puts("[5] Hash Table (Chaining) OK");
}

/* =========================================
 * MAIN: 전 섹션 테스트 실행
 * ========================================= */
int main(void){
    test_dynamic_array();
    test_singly_linked_list();
    test_stack();
    test_queue();
    test_hash_table();
    puts("All tests passed.");
    return 0;
}
