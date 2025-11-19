## 1. 이론부터: 트리 → 이진 트리 개념

수업 초반에 이렇게 정리하시면 좋습니다.

1. **트리(Tree)**

   * 계층 구조를 표현하는 자료구조
   * 용어 정리

     * **노드(node)**: 데이터가 들어 있는 한 점
     * **루트(root)**: 맨 위 노드
     * **자식(child)** / **부모(parent)**
     * **리프(leaf)**: 자식이 없는 노드
     * **서브트리(subtree)**: 어떤 노드를 루트로 하는 작은 트리

2. **이진 트리(Binary Tree)**

   * 각 노드가 **최대 두 개**의 자식을 가지는 트리
   * 왼쪽 자식(left child), 오른쪽 자식(right child)
   * 응용: 이진 탐색 트리(BST), 힙, 표현식 트리 등

3. **메모리 안에서는 어떻게 생겼나?**

   * C에서는 “연결된 구조” = **구조체 + 포인터**
   * 노드 하나가 자기 왼쪽/오른쪽 자식을 가리키는 포인터를 가진다고 설명

칠판/판서용 문장:

> “각 노드는 자기 데이터 + 왼쪽 자식 포인터 + 오른쪽 자식 포인터 3개로 이뤄집니다.”

---

## 2. C에서 이진 트리 노드 구조체 설계

### 구조체 선언

```c
typedef struct Node {
    int data;               // 노드에 저장할 값
    struct Node* left;      // 왼쪽 자식
    struct Node* right;     // 오른쪽 자식
} Node;
```

설명 포인트:

* `typedef struct Node { ... } Node;`
  → 앞으로 `struct Node` 대신 `Node`라고 쓰기 위한 문법.
* `struct Node* left;` 에서 `struct Node`를 아직 다 쓰기 전에 자기 자신을 가리킬 수 있게 “앞으로 이런 구조체가 올 거다”라고 선언하는 형태.

수업 멘트 예:

> “노드 하나가 또 다른 노드를 가리키기 때문에, 자기 자신을 가리키는 포인터를 구조체 안에 넣습니다. 이게 바로 ‘연결 자료구조’의 핵심입니다.”

---

## 3. 노드 생성 함수 (동적 메모리)

```c
#include <stdio.h>
#include <stdlib.h>

Node* createNode(int value) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("메모리 할당 실패\n");
        exit(1);
    }

    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;

    return newNode;
}
```

설명 포인트:

* **왜 malloc?**
  → 트리는 노드 개수가 미리 정해져 있지 않고, 실행 중에 계속 삽입/삭제가 일어나므로 **동적 메모리 할당** 필요.
* `left`, `right`를 `NULL`로 초기화하는 이유: “아직 자식이 없다”는 의미.

---

## 4. 이진 탐색 트리(BST)로 삽입 원리 설명

개념을 먼저 잡습니다.

* **이진 탐색 트리(BST) 규칙**

  * 왼쪽 서브트리의 값 < 루트의 값
  * 오른쪽 서브트리의 값 > 루트의 값
* 삽입 알고리즘(재귀 설명용으로 좋음)

  1. 현재 위치가 `NULL`이면 → 새 노드를 만들어서 놓는다.
  2. 현재 노드 값보다 작으면 → 왼쪽으로 내려간다.
  3. 현재 노드 값보다 크면 → 오른쪽으로 내려간다.

### 코드 (재귀 삽입)

```c
Node* insert(Node* root, int value) {
    if (root == NULL) {
        // 현재 위치에 새 노드 생성
        return createNode(value);
    }

    if (value < root->data) {
        // 왼쪽 서브트리에 삽입
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        // 오른쪽 서브트리에 삽입
        root->right = insert(root->right, value);
    } else {
        // 이미 같은 값이 있는 경우: 여기서는 아무 것도 안 함
        // (필요하면 count 증가 등으로 처리 가능)
    }

    return root; // 루트는 변하지 않으므로 그대로 반환
}
```

설명 포인트:

* 왜 `Node*`를 반환하나요?

  * 루트가 `NULL`이던 경우, 새 노드가 **새 루트**가 되기 때문.
  * 따라서 항상 “현재 서브트리의 루트”를 리턴한다고 설명.
* 재귀 호출 그림 그리기

  * `insert(root->left, value)`를 부르면 “왼쪽 서브트리에 똑같은 작업을 한다”고 설명.
  * 나중에 함수가 끝날 때 원래 `root`를 반환하므로, 위쪽에서 포인터 연결이 유지됨.

---

## 5. 트리 순회(Traversal)의 원리와 C 코드

수업에서 **재귀 함수 구조를 보여주기 좋은 예**입니다.

### 1) 중위 순회(Inorder Traversal)

BST에서는 **오름차순 출력**이 되는 중요한 순회법.

**순서**: 왼쪽 서브트리 → 현재 노드 → 오른쪽 서브트리

```c
void inorder(Node* root) {
    if (root == NULL)
        return;

    inorder(root->left);          // 1. 왼쪽
    printf("%d ", root->data);    // 2. 자기 자신
    inorder(root->right);         // 3. 오른쪽
}
```

설명 포인트:

* “왼쪽 것들 전부 → 현재 → 오른쪽 것들 전부”
* 그림 그려서 실제 값이 정렬되어 나오는 걸 보여주면 학생들이 확 이해합니다.

### 2) 전위, 후위 순회

코드 구조는 거의 같고 **순서만** 바뀝니다.

```c
void preorder(Node* root) {
    if (root == NULL)
        return;

    printf("%d ", root->data);  // 자기 자신
    preorder(root->left);       // 왼쪽
    preorder(root->right);      // 오른쪽
}

void postorder(Node* root) {
    if (root == NULL)
        return;

    postorder(root->left);      // 왼쪽
    postorder(root->right);     // 오른쪽
    printf("%d ", root->data);  // 자기 자신
}
```

학생들에게 강조:

> “세 함수의 구조는 똑같고, **print가 어디에 있느냐**만 다릅니다.
> 전위: 자기-왼-오 / 중위: 왼-자기-오 / 후위: 왼-오-자기”

---

## 6. 전체 예제: Visual Studio에서 바로 돌릴 수 있는 코드

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* left;
    struct Node* right;
} Node;

Node* createNode(int value) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    if (newNode == NULL) {
        printf("메모리 할당 실패\n");
        exit(1);
    }
    newNode->data = value;
    newNode->left = NULL;
    newNode->right = NULL;
    return newNode;
}

Node* insert(Node* root, int value) {
    if (root == NULL) {
        return createNode(value);
    }

    if (value < root->data) {
        root->left = insert(root->left, value);
    } else if (value > root->data) {
        root->right = insert(root->right, value);
    }
    // 같을 때는 아무 것도 안 함
    return root;
}

void inorder(Node* root) {
    if (root == NULL) return;
    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}

void preorder(Node* root) {
    if (root == NULL) return;
    printf("%d ", root->data);
    preorder(root->left);
    preorder(root->right);
}

void postorder(Node* root) {
    if (root == NULL) return;
    postorder(root->left);
    postorder(root->right);
    printf("%d ", root->data);
}

void freeTree(Node* root) {
    if (root == NULL) return;
    freeTree(root->left);
    freeTree(root->right);
    free(root);
}

int main(void) {
    Node* root = NULL;   // 빈 트리

    // 몇 개 값 삽입
    int values[] = { 5, 3, 7, 2, 4, 6, 8 };
    int n = sizeof(values) / sizeof(values[0]);

    for (int i = 0; i < n; i++) {
        root = insert(root, values[i]);
    }

    printf("중위 순회 (Inorder): ");
    inorder(root);
    printf("\n");

    printf("전위 순회 (Preorder): ");
    preorder(root);
    printf("\n");

    printf("후위 순회 (Postorder): ");
    postorder(root);
    printf("\n");

    freeTree(root);  // 동적 메모리 해제
    return 0;
}
```

### Visual Studio 수업 팁

* **프로젝트**: “Empty Project” 또는 “콘솔 앱” → `main.c` 추가
* **C로 컴파일**:

  * 파일 확장자를 `.c`로 만들고,
  * (필요하면) 프로젝트 속성 → C/C++ → Advanced → Compile As → “Compile as C Code(/TC)”

---

## 7. 수업 흐름 예시 (요약)

1. 트리 / 이진 트리 개념, 용어 설명
2. “포인터로 노드를 서로 연결하는 구조” 그림으로 설명
3. `Node` 구조체 설계
4. `createNode`, `insert` 재귀 알고리즘 설명
5. 중위/전위/후위 순회 재귀 구조 비교
6. Visual Studio에서 전체 코드 실행 → 출력 결과 확인
7. 응용 과제 제안

   * 특정 값 찾기 함수 `Node* search(Node* root, int key);`
   * 노드 개수 세기, 트리 높이 계산 등
