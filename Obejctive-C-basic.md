# Obejctive-C-basic

Obejctive -C의 기초 문법과  특징들을 정리

- 본 페이지는 Objective -C 를 처음 배우는 입장에서 기본 문법등을 정리하기 위해서 만들어진 페이지입니다.
- 기존 앱들은 Objective-c로 이루어진 앱들이 많이 있기 때문에 유지 보수의 측면에서 문법을 이해하고 읽을 수 있어야 스위프트로 넘어갈 준비가 된거라 생각합니다.

참고 사이트

- <a herf=https://ko.wikipedia.org/wiki/%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8B%B0%EB%B8%8C-C#.EC.84.A0.EC.96.B8.28Interface.29>위키백과</a>
- <a herf=http://blog.yagom.net/>야곰 블로그</a>

## 개요

- **오브젝티브-C**(영어는 C 프로그래밍 언어에 스몰토크 스타일의 메시지 구문을 추가한 객체 지향 언어이다. 현재, 이 언어는 애플의 매킨토시의 운영 체제인 OS X과 아이폰의 운영 체제인 iOS에서 사용되고 있다.
- 오브젝티브-C는 애플의 코코아를 사용하기 위한 기본 언어이다.
- 코코아는 애플이 개발하여 개발자에게 제공하고 있는 API로 IOS 환경에서는 I phone, I pad에서는 코코아 터치라는 API를 사용한다.
- 코코아 터치는 코코아 터치 계층이라고도 불리는데, 코코아 터치는 가장 상위 계층으로 하위 계층의 구현을 몰라도 하위의 핵심 기능들을 접근 할 수 있도록 지원한다.

-<a herf=https://ko.wikipedia.org/wiki/%EC%98%A4%EB%B8%8C%EC%A0%9D%ED%8B%B0%EB%B8%8C-C>위키백과</a>-



## 특징

### 1. C언어와 스몰토크의 계승

- 오브젝티브-C는 C 위에 덮인 얇은 층이라 볼 수 있다.
- **전처리(preprocessing), 표기(expressions), 함수 선언(function declarations),**  그리고 **함수 호출(function calls)** 등과 같은 대부분의 문법은 C에서 상속받았다.
- 객체 지향적인 기능들은 스몰토크 방식의 **메시지 전달**을 통해서 구현되었다.

### 2. 메세지

- 메시지는 객체 지향 프로그래밍을 지원하기 위해 내부적으로 추가된 문법이다.
- 오브젝티브-C의 기본적인 차이점은 *메소드를 호출*하는 것이 아니라 ***메시지를 전달***한다는 것이다.
  - JAVA에서의 메소드 호출 `obj.doSomething();`
  - Objective-c 에서의 메세지 전달 ` [obj doSomething];`

### 3. 클래스의 선언과 구현

- C++과 비슷하게 선언과 구현을 따로 파일로 나눈다.  헤더파일의 확장자는 .h 소스 파일의 확장자는 .m 또는 .mm이다.

- ##### 선언(Interface)

  ```
  <MyFirstClass.h>

  @interface MyfirstClass : NSObject
  {
    int myFirstInt;
    NSString *myName;
    NSString *mySecret;
  }
  // Make getter, setter
  @property {nonatomic, assign} int myFristInt;
  @property {nonatomic, retain} NSString *myName;

  -(void) myFirstMethod;
  -(void) setMySecret : (NSString *)secret;
  -(NSString *) getMySecret;

  +(void) itIsClassMethod;

  @end
  ```

  - 클래스의 선언은 @interface와 @end 사이에서 선언되어야 합니다.
  - : NSObject는 JAVA에서의 Object 클래스와 마찬가지로 최 상위 클래스로 모든 클래스는 이 NSObject에서 상속되어 생성되는 클래스
  - { } 안에 있는 변수는 인스턴스 변수
  - @property로 선언되는 인스턴스 변수들은 구현에서 @synthesize로 선언해주면 getter와 setter가 자동으로 생성된다.
  - -로 시작하는 메소드들은 인스턴스 메소드로 객체가 생성되어야 사용 가능하다.
  - +로 시작되는 메소드들은 정적 메소드로 맴버 변수에 접근 할 수 없지만 객체가 없어도 사용 가능하다.

- ##### 구현(Implementation)

  ```
  <MyFirstClass.m>
  @implementation MyFirstClass

  @synthesize myFristInt, myName;

  -(void) myFirstMethod{
    NSLog(@"This is my first Method's Log")
  }
  -(void) setMySecret : (NSString *)secret{
    mySecret = [NSString stringWithString:secret];
  }
  -(NSString *) getMySecret{
    return mySecret;
  }

  +(void) itIsClassMethod{
    NSLog(@"It is class method. it works without allc or init")
  }

  @end
  ```

  - 구현은 @implementation과 @end 사이에 코딩하는데, 메소드만 구현하면 된다.
  - @synthesize는 앞서 @property로 선언된 인스턴스 변수들을 자동으로 getter와 setter를 만들어 달라고 요청하는 부분이다.

### 프로토콜

- 공식 프로토콜
  - 지정된 메소드와프로퍼티의 목록으로 JAVA의 interface와 같은 역할을 한다.
- 비공식 프로토콜
  - 명세된 모든 메소드들을 구현해야 하는 프로토콜과 달리 일부만 개발자가 선택해서 개발 할 수 있는 프로토콜.
  - 개발자가 이 객체를 구현하는데 필요하다고 예상되는 메소드들을 명세해 놓은 것으로 개발