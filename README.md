# Compiler for new language
if나 for, while같은 제어 구문까지 override 가능한 언어를 만들고 싶은데.
복잡하게 생각하지 말고, 다음과 같은 형식을 생각하자.

```
control(p1)
{
    p2
}
```

이때 고민은, p1을 어떻게 처리해야 아름답게 처리되냐는 것이다. p1에는 분명히 코드 블럭이 들어온다. 문제를 환원하면, 코드 블럭들을 어떻게 파라매터로 받을 수 있는가다.
- 그냥 세미콜론으로 구분할까?
    - 만약 여러 줄의 코드 블럭을 받고 싶다면?
- 코드 블럭을 나타내는 기호 {}를 정의하면 어떨까?
    - 멋진 생각이긴 한데, 그러면 스코프가 분리된다.
- 바로 위의 방법을 그대로 차용하되, 코드 블럭을 제어구문 내에서 사용하면 스코프를 공유하도록 하면 어떨까?
    - 좋은 방법인 것 같다.

#### 2020 / 04 / 26
- 말도 안 되는 문법의 언어를 만듬.
- 일단 자바스크립트처럼 사용 가능

#### 2020 / 04 / 27
- 다음과 같은 정말 추상적이기 그지없는 코드 구조를 만들어보자.
    - 코드의 실행 단위는 코드블럭이다.
    - 코드블럭은 하나의 코드라인이거나, 코드라인들의 묶음이다.
    - 코드블럭은 반환값을 가질 수 있다.
    - 코드블럭은 기본적으로 실행되지 않는다.

#### 2020 / 04 / 28
- 어제 대충 문법을 완성하였다. 물론 if문도 없는 단순한 언어이기는 하다.
- 그렇다면 이제 진짜 프로그래밍 언어를 작성해보자.
- 프로그래밍 언어라면, 아래와 같은 속성을 가져야 한다.
    1. 변수를 선언할 수 있다.
    2. 변수를 참조할 수 있다.
    3. 변수에 값을 할당할 수 있다.
    4. 변수의 값을 검사하여 특정 위치로 점프할 수 있다.(0이면/아니면 점프)
- 지금 문제되는 것은 과연 프로그래밍 언어에서 '예기치 못한'변수가 생길 수 있냐는 점이다. 예를 들어 if문 안에 변수를 만든다면?
    - 이게 문제가 되는 이유는 변수의 스코프와, 변수 공간 할당 때문이다.
    - 만약 모든 변수가 <strong>컴파일 타임에 예상가능하다면</strong>, 우리는 변수 공간을 고민할 필요도 없고, 스코프도 고민할 필요가 없다.
    - 그러나 c언어에서 malloc이 보여주듯, 그렇지 않음이 자명하다.
    - 그런데 고민되는 점은, 만약 이럴 경우 scope는 어떻게 되냐는 점이다.
    - 결론 : 눈에 보이는 변수의 스코프만 고려하자.

#### 2020 / 04 / 29
- AST만드는 법을 공부해야겠다.

#### 2020 / 05 / 01
- AST만드는 법을 알아냈다. Antlr4에서 nonterminal에 label붙이는 것을 지원한다. 그러므로 쉽게 AST를 분석할 수 있다.
- 추가적으로 visitor pattern과 listener pattern에 대해 알게 되었다.
    - visitor pattern이 더욱 flexible하다고 볼 수 있지만, listener는 explicit stack을 사용하는 반면, visitor는 call stack을 사용한다. visitor는 그러므로 recursion이 너무 깊어질 경우 터질 위험이 있다.
    - 근데, call stack이 아무리 깊어 봐야 100~1000단계를 넘을 수 있을까? 어지간한 상용 언어는 아무리 못해도 64단계는 버틴다.

#### 2020 / 05 / 02
- Antlr4에서 Visitor pattern을 제대로 사용해본다!

#### 2020 / 05 / 04
- 인터넷에 Python 예제가 너무 없어서, Java 예제를 사용하기로 한다.

