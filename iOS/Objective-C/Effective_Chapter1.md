## Effective Objective-C

앞으로 7번에 걸쳐 Effective Objective-C라는 책을 보며 공부한 내용을 정리하고자 합니다. 총 7장으로 이루어져 있는 책으로 각 장마다 5개의 아이템이 있습니다. 이 글은 순수하게 공부 목적으로 적는 것을 알려드리면서  시작하겠습니다.

### Intro

이 책의 저자는 커뮤니티에 있는 자료들을 모아 자신이 직접 정리해서 어떻게 쓰는 것이 좋다는 측면에서 새롭게 묶어서 작성한 책이라고 합니다. 사실 이 책이 나온지 벌써 4년이 지났는데, Objective-C와 관련된 중급서적이 마땅하지 않아서 이 책을 공부하기로 결정했습니다. 

### item#1. Objective-C의 기원과 친숙해져라

objective-c에는 런타임이라는 것이 숨겨져 있다. 사실 objective-c는 스몰토크라는 언어가 가지고 있는 컨셉과 구조와 문법을 반영해서 만든 언어이다. 그래서 스몰토크에서 넘어온 문법 중에 objective-c에서 가장 많이 사용하는 것이 `대괄호`이다. 특정한 객체에게 메세징을 보낼 때 사용한다. 다음 짧은 코드를 보자.

![messaging](../../img/Chapter1/1.png)

첫 줄은 obj라는 객체를 만들면서 클래스한테 메세지를 보내게 된다. 

*//여기서 new는 old-fasion한 문법인데 컴파일러가 이 코드를 실행시키면 +alloc 과 -init을 불러준다. 초기에는 스몰토크에서 넘어와서 사용했지만 지금은 사용되지 않는다. 지금도 사용할 수는 있다고 하는데 권장되지는 않는다고 한다.*

두번째 줄은 obj라는 객체에 performWith 함수를 통해 파라미터를 메세징하는 방식이다. 

다음 그림을 보자.

![messaging inner](../../img/Chapter1/2.png)

여기서 `isa`라는 게 있다. 처음 보고 아이사, 이사라고 읽었는데 이즈어라고 읽는 거라는걸 알게되고 무지함에 감탄했다. objective-c의 모든 객체는 isa라는 변수를 가지고 있다. isa는 클래스인데 클래스를 가르키는 포인터를 가지고 있다. 

**I'm am a boy**라는 문장을 살펴보자.  I와 boy의 관계를 생각하면 I는 boy의 범주 안에 포함되어 있다. 이를 is a 관계라고 한다. 

[myClassB printVar]를 보면 myClassB와 printVar도 isa 관계입니다. 메세지는 myClassB라는 인스턴스로 전달됩니다. 그러면 런타임이 obj_sendMsg라는 함수 호출을 하게 되고 런타임 함수가 myClassB의 isa 포인터를 보고 myClassB라는 인스턴스가 어떤 클래스인지를 알게 됩니다. 그러면 런타임은 찾은 클래스가 가지고 있는 메소드를 가지고 printVar라는 selector가 어디에 있는 메소드인지 찾아줍니다. 

*//이게 맞는건지 잘 모르겠습니다*ㅜㅜ



Objective-c에서 객체는 `항상` 힙에 할당이 된다.  다음 코드를 통해 좀 더 알아보자.

![declare instance](../../img/Chapter1/3.png)

첫 번째 코드부터 살펴보자. 여기서 인스턴스는 the string이다. 그리고 someString이라는 포인터가 생기는데 이 포인터는 the string을 가르키게 된다. 

두 번째 코드 역시 마찬가지다. anotherString이라는 포인터는 someString의 포인터를 복사하고 the string을 가르키게 된다.

![stack and heap](../../img/Chapter1/4.png)

위에서 말했듯이 객체는 힙에 할당이 되어 있고 스택에는 객체를 가르키는 포인터 주소를 담고 있다. 

다음 코드를 보자.

![error](../../img/Chapter1/5.png)

만약 이런 식으로 선언을 하게 되면 어떻게 될까? 에러가 난다. 이유는 간단하다. 무조건 모든 객체는 힙에 만들어져야 하기 때문에 포인터로 선언을 해줘야 한다.

하지만 예외인 경우가 있다. `int`, `double`,`bool`, `CGRect`, `struct` 등등… 이런 것들은 클래스가 아니라 C 타입이고 이러한 타입은 스택에 값을 바로 저장할 수 있기 때문입니다. `NSUInteager` 또한 내부는 int랑 같아서 스택에 바로 값을 저장한다. 이런 것들은 스택 공간 변수라고 불린다. 



###  item#2.
