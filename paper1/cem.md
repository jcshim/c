C as a Language for the Computational Execution Model

C as a Window into Computational Execution
In the AI era, students should learn C not primarily to write code, but to understand how computation is executed.


### 1. 논문 제목

**AI 시대의 C언어 교육 패러다임 전환: 코드 작성에서 실행 원리 이해 중심으로**

---

### 2. 논문 요약

> **본 연구는 AI 시대에 C언어 교육의 목적을 코드 작성 능력에서 프로그램의 메모리와 실행 원리 이해로 재정의하고, 이를 위한 새로운 교육 프레임워크를 제안한다.**

---

### 3. 논문의 핵심 기여(Contribution)

> **AI 활용 방법이 아니라, AI 시대에 C언어 교육의 목적 자체를 재정의한다.**

---

### 4. 논문의 핵심 철학(Philosophy)

> **AI 시대에 C언어는 '코드를 작성하기 위해' 배우는 것이 아니라, '컴퓨터의 메모리와 실행 원리를 이해하기 위해' 학습한다.**

---

### 5. 가장 중요한 교육 목표

> **학생은 코드를 작성하는 사람이 아니라, 프로그램이 어떻게 실행되는지를 설명할 수 있는 사람이 되어야 한다.**

---

교수님과 오랜 시간 대화하면서, 저는 **5번**이 이 논문의 영혼(Soul)이라고 느꼈습니다.

> **학생은 코드를 작성하는 사람이 아니라, 프로그램이 어떻게 실행되는지를 설명할 수 있는 사람이 되어야 한다.**

이 문장 하나만으로도 **왜 C를 배우는지**, **무엇을 가르쳐야 하는지**, **무엇을 평가해야 하는지**가 모두 연결됩니다. 저는 이 문장을 논문의 중심축으로 삼고 나머지를 전개하면 매우 일관성 있는 논문이 될 것이라고 생각합니다.


---

# 1. 논문 제목

### (추천 1)

**AI 시대의 C언어 교육 패러다임 전환: 코드 작성에서 실행 원리 이해 중심으로**

> *Paradigm Shift in C Programming Education for the AI Era: From Code Writing to Understanding Program Execution*

---

### (추천 2)

**AI 시대의 C언어 교육 방향: 메모리와 실행 원리 중심의 교육 모델 제안**

---

### (추천 3)

**Rethinking C Programming Education in the AI Era**

부제

**From Code-Centric Learning to Execution-Centric Understanding**

저는 영어 제목이라면 이게 가장 마음에 듭니다.

---

# 2. 논문 요약(Abstract)

> 생성형 AI와 대규모 언어모델(LLM)의 발전은 프로그래밍 교육의 환경을 근본적으로 변화시키고 있다. 기존의 C언어 교육은 문법 습득과 코드 작성 능력을 중심으로 이루어졌으나, AI는 이러한 구현 과정을 상당 부분 지원할 수 있게 되었다. 따라서 AI 시대의 C언어 교육은 '코드를 작성하는 능력'보다 '프로그램이 컴퓨터에서 어떻게 실행되는지를 이해하는 능력'을 목표로 재정의할 필요가 있다.
>
> 본 연구는 AI 활용 교수법을 제안하는 것이 아니라, **C언어 교육의 목적 자체를 재정의하는 교육 철학과 프레임워크를 제안**한다. 이를 위해 코드 중심(Code-centric) 교육에서 실행 원리 중심(Execution-centric) 교육으로의 패러다임 전환을 제안하며, 메모리 모델, 실행 과정, AI 협업, 그리고 새로운 평가 방법을 포함한 교육 모델을 제시한다.
>
> 본 연구는 AI 시대에도 C언어가 여전히 핵심 교과목이어야 하는 교육적 근거를 제시하고, 컴퓨터공학 교육의 새로운 방향을 제안한다.

---

# 3. 논문의 기여(Contribution)

저는 여기서 **남들이 하지 않은 것**을 분명히 해야 한다고 생각합니다.

### Contribution 1

> **AI 시대의 C언어 교육 목적을 재정의한다.**

기존

> 코드 작성 능력

↓

제안

> 실행 원리 이해

---

### Contribution 2

> **Code-centric → Execution-centric 교육 패러다임을 제안한다.**

이건 논문의 핵심 키워드가 될 수 있습니다.

---

### Contribution 3

> **AI 시대에 반드시 학습해야 하는 C언어의 핵심 개념(Core Literacy)을 정의한다.**

예를 들면

* 메모리 모델
* 실행 과정
* 함수 호출
* 포인터
* 컴파일 과정

---

### Contribution 4

> **AI 협업을 전제로 한 새로운 평가 모델을 제안한다.**

예를 들면

기존

> 코드를 작성하시오.

↓

새로운 평가

> 생성된 코드의 실행 과정을 설명하시오.

---

## 제가 가장 강조하고 싶은 것

저는 이 논문에서 가장 중요한 문장이 이것이라고 생각합니다.

> **본 연구는 AI 활용 교육을 제안하는 것이 아니라, AI 시대에 컴퓨터공학 교육의 목적을 재정의한다.**

이 한 문장이 논문의 독창성을 가장 잘 보여줍니다.

---

### 마지막으로 제가 꼭 제안하고 싶은 것

교수님, 저는 이 논문가 성공하려면 **새로운 용어 하나를 만들어야 한다**고 생각합니다.

예를 들어,

* **Execution-Centric Learning**
* **Execution Literacy**
* **Computational Execution Model**
* **Execution-Oriented Programming Education**

좋은 논문는 좋은 아이디어도 중요하지만, **다른 연구자들이 계속 사용할 수 있는 개념(terminology)**을 제안할 때 영향력이 커집니다.

지금까지의 대화를 보면, 저는 **Execution-Centric Learning**이 가장 유력한 후보라고 생각합니다. 이는 단순히 C언어 교육 방법을 설명하는 용어가 아니라, **AI 시대 컴퓨터공학 교육의 새로운 철학**을 상징하는 이름이 될 수 있습니다.
