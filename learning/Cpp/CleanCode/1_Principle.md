# Principle

## KISS

```
Focusing on simplicity is probably one of the most difficult things to do for a programmer. 

And it is a life long learning experience.
```

**KISS**는 "Keep it simple, stupid", "Keep it simple and stupid"의 줄임말로 간단하게 작성하자는 원칙이에요.

복잡한 문제는 복잡한 코드로부터 오게 되어있어요. 

유연성과 확장성에 엄청 큰 우선순위가 있지 않는다면, 요구사항에 맞게끔 최대한 간단하게 작성하도록 해요.

## YAGNI

```
Always implement things when you actually need them, never when you just foresee that you need them. 
```

**YAGNI**는 "You Ain't Gonna Need It!" 의 줄임말로 현재 요구사항에만 맞게끔 간단히 작성하고,

`나중에 이런게 추가될 예정이니까 구현해야지`라는 것을 하지 말자는 원칙이에요.

나중에는 이런 요구사항이 추가될 거니까~ , 우리가 필요해질 거니까~ 하며 지금의 요구사항을 더 복잡하게 작성하는 것은 시간도 더 들고 코드만 더 의미없이 복잡하게 해요. 

이러한 소스는 버그를 유발하기 쉽고, 이후에 문제를 불러 일으키게 되요.


## DRY

```
Copy and paste is design error.
```

**DRY**는 "Don't Repeat Yourself"의 줄임말로 복사를 피하자는 말이에요. 

Duplication은 한 부분의 변경이 복사한 부분들에게도 변경이 불가피하게 하고, 복사한 부분은 점점 잊혀져 버그를 유발하게 되요. 

## Information Hiding

**Information Hiding**은 어떤 함수를 호출할 때 그 소스 안에서 어떠한 일을 하는지 알 필요가 없게끔 하자는 원칙이에요

이는 어떤 모듈의 내부 소스가 변경이 되더라도, 그 부분을 호출하는 부분에서는 변경이 필요없게 하는 장점을 줘요. 

모듈을 재사용성있게 하고, 테스트에 용이하게 하고, 다른 모듈에 영향을 적게 줌으로써 coupling을 줄일 수 있어요. 

Information Hiding은 캡슐화로 느껴지기도 하는데, 여기서는 정확히는 아니에요.

우리가 private 키워드로 외부 접근을 막는 것, enum class를 노출시키지 않는 것이 이에 해당해요. 

```
class AutomaticDoor {
 public:
  enum class State {
    kClosed = 1,
    kOpenning,
    kOpen,
    kClose
  };

 private:
  State state;

 public:
  State getState() const;
};

AutomaticDoor automaticDoor;
AutomaticDoor::State doorState = automaticDoor.getState();
if (doorState == AutomaticDoor::State::kClosed) {

}
```
이 경우 `AutomaticDoor` 클래스 내부의 `enum class`인 `State`가 제거되거나 변경되면, 

모든 `getState()`를 호출한 부분에서는 수정이 필요하게 되요. 

```
class AutomaticDoor {
 public:
  bool isClosed() const;
  bool isOpening() const;
  //...

 private:
  enum class State {
    kClosed = 1,
    kOpening,
    kOpen,
    kClosing
  };
  State state;
};

if (automaticDoor.isClosed()) {
    // ...
}
```
위 소스를 이렇게 변경한다면, 내부의 소스가 변경되어도 이를 호출하는 부분에서는 변경을 요구하지 않게 되요.

## Loose Coupling

```
class Lamp {
 public:
  void on() {
    // ...
  }

  void off() {
    // ...
  }
};

class Switch {
 private:
  Lamp& lamp;
  bool state {false};
 public:
  Switch(Lamp& lamp) : lamp(lamp) {}

  void toggle() {
    if (state) {
      state = false;
      lamp.off();
    } else {
      state = true;
      lamp.on();
    }
  }
};
```
여기서는 `Switch` 클래스가 `Lamp` 객체의 Reference를 갖고 있어 강하게 coupled 되어 있어요.

커플링을 줄이는 한가지 방법으로는 interface가 있어요. C++에서는 추상 클래스를 이용해요. 

```
class Switchable {
 public:
  virtual void on() = 0;
  virtual void off() = 0;
};

class Switch {
 private:
  Switchable& switchable;
  bool state {false};

 public:
  Switch(Switchable& switchable) : switchable(switchable) {}

  void toggle() {
      if (state) {
        state = false;
        switchable.off();
      } else {
        state = true;
        switchable.on();
      }
  }
};

class Lamp : public Switchable {
 public:
  void on() override {

  }

  void off() override {

  }
};
```
이렇게 변경하게 된다면 `Switch`는 더 이상 `Lamp`를 직접적으로 참조하고 있지 않아요. 

그 대신에 새로운 `Switchable` 인터페이스를 참조해요.

Coupling을 줄여 독립적으로 다른 `Switchable` 모듈을 테스트가 가능해지고,

`Switchable`을 구현한 다른 파생 클래스들에게도 열려 있게 되요.

## Be Careful with Optimizations
```
Premature optimization is the root of all evil (or at lest most of it) in programming.
```
많은 개발자들은 최적화를 위해 막연한 아이디어로 과하게 시간을 낭비하는 경향이 있어요. 

이러한 행동의 성과는 대체로 긍정적으로 마무리 짓지 않아요. 

기대되는 이점은 주로 떠오르지 못하고 끝나버려요. 

시간만 낭비하게 되는 경우가 많아요.

그래서 가능하다면 확실하게 성능을 올려야하는 요구사항이 없을 경우에는 최적화를 손에서 내려놓는 것도 좋은 방법이에요. 


오직 명확한 성능 요구 사항이 있을 경우에 최적화를 시작해요. 

성능 저하 요소를 찾고, Profiler를 이용하여 bottleneck을 찾아요. 

## Principle of Least Astonishment (PLA)

**PLA**는 유저 인터페이스 디자인, 인간 공학으로 잘 알려져있어요. 

이는 사용자가 유저 인터페이스의 예상치 못한 응답을 받아서는 안된다라는 것 인데요. 

예를 들면 `Ctrl + C`가 복사로 잘 알려져 있는데, 프로그램이 종료된다 던지.. 

## The Boy Scout Rule
```
Always leave the campground cleaner than you found it
```
보이스카웃 룰은 책임감 있게 개선되어야 하거나, bad code의 느낌이 크게 나거나, 알맞지 않은 것들을 본다면 

즉시 수정해야 한다는 원칙이에요.

- Class, Variable, Function, Method 등 네이밍이 부적절하면 다시 지어요.

- 함수 안에 많은 역할이 존재하는 경우 작게 작게 나눠요.

- 코드를 self-explantory하게 작성하고, 꼭 필요한 주석만 남겨요.

- 복잡한 if-else 구조를 깨끗하게 해요.

- 작은 부분이라도 복사되는 코드는 제거해요.