---
title: "자바 구조적 프로그래밍"
date: 2021-02-08 월요일 23:55:00+0900
excerpt: "전역변수, 매개변수, 클래스, 가변길이배열, 오버로딩"
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true
last_modified_at: 2021-02-08T08:06:00-05:00
---

## 자바 구조적 프로그래밍

> reference : [뉴렉처 자바 구조적인 프로그래밍](https://youtube.com/playlist?list=PLq8wAnVUcTFWQ4TpRPZRa5nj1VwfyO7st) 을 정리한 내용입니다.

### 1. 프로그램을 쪼개서 만드는 것 (for 개발자)

- 코드를 나누다 보면 구조가 자연스럽게 생긴다.
  - Bottom up 방식 : 프로그램을 잘라 구조를 만드는 것
  - Top down 방식 : 구조부터 만들어 코드를 구현함

### 2. 함수의 역할

- 코드의 직접 사용을 차단할 수 있다.
- 코드를 작게 나누어서 만들 수 있다.
- 코드를 집중화 할 수 있다.
- 코드를 재사용 할 수 있다.

### 3. 전역변수 사용

- 똑같은 변수를 여러 곳에 반복해 정의하는 건 좋지 않음.
- 코드 바깥에 **static**을 붙이면 전역변수가 된다. `static int[] korList = new int[3];`
- 쪼갠 함수와 main 함수끼리 공유해야 하는 데이터만 전역변수로. scanner 처럼 값을 공유하지 않아도 되는 건 공유 x 따로 선언해주자
- 그런데 전역변수로 이름 충돌나게 하는 것 보단 지역화 시켜 다른 곳에 문제 발생 않도록 안쪽에 넣는게 바람직함. 그래서 등장하는게 매개변수임.

### 4. 함수의 매개변수

- 함수 안의 변수를 다른 곳의 전역변수를 통해 사용할시, 외부변화에 영향을 받게 된다. 함수의 독립적 변수의 자립도↓
- 그래서 나온 `매개변수`. 참조하는 식으로 매개값을 받아 씀.
- 매개변수 이름은 원래 변수명과 달라도 상관x 받아온 주소값으로 지역내에서 어떤 이름으로 쓸지 정하는 것이기 때문에
- 다른 외부 변화에 영향을 받지 않는 **함수의 고립화**  
  tip : 프로그래밍을 할 땐 배열연산을 최소화 할 수 있도록 지역변수 하나 준비하는게 바람직함.

  ```java
  static void inputKors(int[] kors) {
  int kor;
  for (int i = 0; i < 3; i++) {
    do {
      kor = scan.nextInt();
      if (kor < 0 || 100 < kor)
        system.out.println("국어성적은 0~100까지 범위")
    } while (kor < 0 || 100 < kor);
      kors[i] = kor;
    }
  }
  ```

### 5. 함수 이름 짓기

- 숫자로 시작할 수 없다.
- 문자 사이에 빈 공백은 사용할 수 없다.
- 특수문자(!@$%...)는 사용할 수 없다.  
  ex) 로또번호정렬 -> 정렬로또번호 -> sortLotto()

### 6. Top-down 방식

- 경험이 있을 경우, 비슷한걸 만들었을 경우에 사용하기 용이함.
- 예제) 로또번호 생성기

### 7. 매개 변수

- 함수 정의시 parameter를 어떻게 선언해야 하는지?

```java
// 예제1
int[] lotto = new int[3];
print(3, 5.3f, lotto);
void print(int a, float b, int[] c /*int[] lotto*/){
}

// 예제2
char[][] names = new char[3][10];
double cnt = print(true,4.0,names);
double print(boolean a, double b, char[][] names){
    return 2.2
}
```

### 8. 데이터 구조화

- grouping
- 고객관리 프로그램의 데이터 : 고객 (큰 단위로 생각)
- entity, 개념단위의 데이터
- 함수 만들 때 개념에 대한 의미를 함수명에 내포시킴, but entity를 데이터로 쓴다면?

```java
//함수 명에 개념 의미를 내포시킨 것
void drawEllipse(int x, int y, int w, int h, int color){}
// entity(개념단위의 데이터)를 함수에 제공한 것, 함수명이 훨씬 깔끔해진다
void draw(Ellipse ellipse){}
```

- **개념화된 데이터**를 쓰면 코드가 깔끔해진다!
- **Class** : 자료형을 엮어서 entity로 쓸 수 있게 정의해줌.  
  사용자가 원하는 형식의 데이터 구조를 정의하기 위한 목적으로 쓰인다.

### 9. Class

1. 데이터 구조 정의

```java
public class Exam{
    int kor;
    int eng;
    int math;
}
```

2. 선언

```java
public class Program{
    public static void main(String[] args){
        Exam exam; //선언
    }
}
```

- _참조_ vs 값
  - 값 형식(int, float, double)은 선언만 해도 데이터가 들어갈 수 있는 공간이 마련되지만, Exam 같이 참조 형식은 공간을 마련해 줘야 한다.
- 참조형식은 무조건 기본적으로 담고있는 공간이 null이다. 누군가가 `new`란 연산자로 뭔갈 만들어줘서 참조시켜 줘야 한다.
- exam.kor=30;은 따라서 nullPointerException. 실존하는 방이 아니기 때문에. 현재 exam은 null이다.

3. 객체 생성

- `exam = new Exam();`
- 값 형식이 아닌 것들은 모두 new로 공간 생성해줘야 한다! 이때서야 공간이 만들어짐.
- 정의한 걸 기반으로 kor, eng, math라는 방이 실존하게 되고(실체화 되고) 그때서야 exam 이름으로 참조하는 방이 생기니까 `exam.kor=30;` 가능하게 됨.

4. 정리

- 우리가 정의한 모든 건 참조형식이고 반드시 new를 통해 만든 객체를 대입해 줘야한다.  
  new로 공간을 만들어줘야만 그 공간에 값을 대입할 수 있다. 그렇지 않으면 NullPointerException이 발생한다.
  - tip!!! - pointer 변수 : new Exam()의 주소를 저장하면서 자기도 공간인 것. 그래서 자신의 주소가 있음.
- 참조 변수 : 똑같이 내부적으로 주소 저장하긴 하지만, 주소가 공간을 갖고있다고 하지 않는다. 객체가 존재하려면 이름이 필요함. `Exam exam = new Exam();`에서 exam은 객체의 식별자(이름)이라고 생각하기.

- 변수를 만들고 `Exam` 변수를 선언하고 `exam` 객체를 만들어 참조시켜 `new Exam()` 그 공간을 이용해서 값을 대입하고 `exam.kor=30` 값을 출력

### 10. 구조체 이용한 코드 작성하기

- `Exam exam = new Exam();`  
  함수에 전달해도 참조는 new Exam()을 하기 때문에 이 공간은 공유가 된다.
- 반복되는 건 임시변수 적극 활용하기! 코드가 더 깔끔해진다.

### 11. 구조체 배열

```java
Exam[] exams = new Exam[3];
exams[0].kor=30; // 안됨!
```

- 여기의 new는 Exam이란 객체가 만들어진 게 아님. Exam형식의 배열 3개가 만들어진 것. Exam 객체가 아니라 참조변수 3개가 만들어진 것이다!
- 국,영,수 없는 배열에 그냥 이름 달아준 것이다.

```java
Exam[] exams = new Exam[3];
exams[0] = new Exam(); // 반드시 필요함
//국,영,수 공간을 만들고 0번째 이름으로 참조하겠다
exams[0].kor=30;
```

- 해당 배열에 실제 객체를 참조시켜줘야 함.
- **클래스 배열은 객체 배열이 아니라 객체 참조 배열이다.**
- exams.length는 배열이 갖고 있는 방의갯수, 성적의 갯수가 아님. (printList 부분)
- for문 안 변수선언 비효율적? No!  
  선언은 함수가 호출될 때 마련되는 준비물 같은 것. 연산이 아니기 때문에 반복되며 연산되는 게 아님.  
  요리 준비물처럼 미리 준비되고, 동작 과정(요리)에선 포함되지 않음. for문이든 for문밖이든 반복해서 실행되지 않음.

### 12. 가변 길이 배열

- `int current`를 생성하여 시도

```java
//input 함수
private static void inputList(Exam[] exams, int current) {
  //생략
exams[current] = exam;
current++;
}
//print 함수
private static void printList(Exam[] exams, int size) {
		//생략
		for (int i = 0; i < size; i++) {
		//생략
		}
	}
  // 시도는 좋았으나...함수로 받아온 current만 그 지역내에서 값이 변하지 실제 current가 변하진 않는다. printList의 current(size)=0이라 출력X ㅠ
```

- 자바는 c, c++과 다르게 <u>주소 전달할 유일한 방법</u>은 기본 자료형이 아니라, <u>참조형 전달만</u> 가능함.
- 함수 단위에서 공유해야 할 데이터라면 큰 단위의 구조체로 묶어서 공유

```java
class ExamList{
  Exam[] exams;
  int current;
}

ExamList list = new Examlist();
list.exams = new Exam[3];
list.current = 0;
inputList(list);
printList(list);
```

- 동적인 공간을 관리하기 위해 필요한 capacity 변수
  - 용량(capacity)가 변하기 때문에 새로운 변수가 필요함.
  - 가변 List : [] + current + capacity
- 만약에 공간이 모자르면
  ```java
  if(capacity==current){
    Exam[] temp = new Exam[capacity+amount];
    //amount개 확장한 새로운 배열 temp를 생성
    for(int i=0; i<current; i++)
      temp[i] = list[i];
      //list(exams)에 있는 데이터를 temp 배열로 옮김
    list = temp;
    // temp가 참조하는 객체를 list가 참조하게 함
    // 원래 list 는 garvage가 된다
    capacity += amount;
    // 현재 capacity 값을 +10 증가시킴
  }
  ```

### 13. 함수 오버로딩

- 같은 기능 다른 인자를 가지는 함수 추가하기

```java
static void printList(ExamList list){
  int size = list.current;
  Exam[] exams = list.exams;
  //(생략)
}
static void printList(ExamList list, int size){
  //(생략)
}
// 두번째가 할 수 있는걸 첫번째가 다 할 수 X
// 첫번째가 할 수 있는건 두번째가 다 할 수 O
// 따라서!! 구현할 때 두번째만 있어도 됨
```

- 기본 함수(가장 심플한 함수)에서 인자 많아진 게 오버로드 된 함수.
  - 인자 없는 게 더 사용빈도가 높으니까 기본 함수
  - 오버로드 함수 구현하면 사용자가 골라쓸 수 있어서 편함.
- 인자 많은 함수가 인자 없는 함수의 일을 다 할 수 있다.
- **집중화**
  - 기능이 같으니 한 함수가 달라졌을 때 같이 고쳐져야 한다.
  - 따라서 공통의 기능은 집중화를 해야한다!
  - 구현은 인자 많은 것만! 인자 없는 보편적인 함수가 구현한 함수를 재호출 할 수 있도록 만들자.
  ```java
  static void printList(ExamList list){
    printList(list, list.current)
  }
  static void printList(ExamList list, int size){
    //(생략)
  }
  ```

### 14. 코드 실행과 함수 호출 스택

- 메모리 : 코드 실행 과정에 필요한 데이터 보관 영역 (Data)

  - Heap
    - 동적으로 생성되는 메모리 마련하는 공간
    - 입석
    - new Exam() 객체 공간이 할당되어야 함. 연산에 의해 할당되는 공간.
    ```java
      kor // 0(4번) -> 7(7번)
      eng // 0(4번)
      math // 0(4번)
    ```
    - 참조가 사라져도 남아있다. 메모리 누수.
    - 자바같은 경우엔 런타임 환경에 의해 참조되지 않은 객체가 heap에 있으면 주기적으로 찾아 지워준다.
  - Stack
    - 함수의 지역변수 마련하는 공간
    - 코드 연산(process) 전에 선언되어 있는 변수 공간이 미리 마련된다.
    ```java
      // 메인함수
      args
      total // 값, 0(1번)
      avg // 값, 0(1번)
      exam
      // input 함수 지역변수(6번), 7번 연산 후 (값반환) 사라짐
      // exam
      // test
    ```
    - 준비된 공간은 다른 공간이 할당될 수 없게 잠기고 딱 알맞은 메모리크기로 마련됨.
    - 예약석
    - 함수 호출되면 위에 쌓였다가 사라지면 밑으로 내려가는... 마치 메모리가 물건을 적재하듯 올라갔다가 다시 내려가는 것처럼 사용되기 때문에 stack
  - 코드가 올라오는 영역 (Text)
    - 컴파일 이진파일
    - process
    ```java
    ststic void main(String args[]){
      int total = 0; // 1번
      float avg = 0; // 2번
      Exam exam = new Exam();
      // 3번 new Exam 3번
      // 4번 () : 초기화(0,0,0)
      // 5번 = : new Exam()이 참조변수(heap의 kor,eng,math)를 가리키게 됨
      input(exam,7);
      // 6번, 함수 호출
    } // 8번 시스템으로 리턴 -> 스택에 메인함수 영역도 사라짐
    void input(Exam exam, int test){
      ...
      kor=test; // 7번
    }
    ```

---

### Tip

- F3 : 함수로 가는 단축키
- alt ← / alt → : 이전에 있던 곳으로 가는 단축키
