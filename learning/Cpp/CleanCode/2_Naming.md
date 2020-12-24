# Basics of Clean C++

## Good Names
```
Programs must be written for people to read, and only incidentally for machines to execute.
```

소스 코드 파일, 네임스페이스, 클래스, 템플릿, 함수, 매개변수 등등 모든 이름들은 이해하기 쉽도록 의미있고 표현이 풍부해야 해요. 

이름을 정하는 데 고민하는 시간은 아깝지 않은 것 같아요.

## Names should be Self-Explanatory
```
Use simple but descriptive and self-explaning names.
```
좋은 이름이란 따로 설명이 필요 없이 이름 자체로 잘 설명하는 이름이에요.

num, flag, list, data 같은 이름은 의미가 포괄적이어서 좋지 않은 이름이에요.

## Use Names from the Domain

**DOD(Domain-Driven Design)** 라는 말로 실제 시스템의 도메인 이름을 따서 소프트웨어 개발을 하는 방법을 말해요.

예를 들어 Rental에 관한 비즈니스를 개발한다 하면, ( rental car, car pool, rentee, rental period, rental confirmation, accounting .. ) 이러한 프로세스들이 존재할 것이에요. 

이렇게 실제 도메인의 자주 사용되는 용어들을 개발에 사용하면 커뮤니케이션에 유용하다는 장점이 있어요.

도메인에서 사용되는 말을 이용하여 컴포넌트, 클래스, 함수를 만들어보면, 더 이해하기 쉽게 되요.

```
class ReserveCarUseCaseController {
 public:
  Customer identifyCustomer(const UniqueIdentifier& customerId);
  CarList getListOfAvailableCars(const Station& atStation, const RentalPeriod& desiredRentalPeriod) const;
  ConfirmationOfReservation reserveCar(const UniqueIdentifier& carId, const RentalPeriod& rentalPeriod) const;

 private:
  Customer& inquiringCustomer;
}
```
위 클래스는 전형적인 차를 대여하는 차 렌트 도메인이에요.

public 함수들을 차례로 읽어보면 순서대로 차를 대여하는 독립적인 단계임을 알 수 있어요. 

이러한 점이 코드를 더 읽기 쉽게 하고, 이해하기 쉽도록 도와줘요.

## Choose names at an Appropriate Level of Abstraction

최근의 복잡한 소프트웨어를 개발하고 유지하기 위해서는 구조적으로 작게 작게 나누어 개발을 해요.

앞서 얘기했던 **Domain-Driven Design(DDD)** 혹은 **Object-Oriented Analysis and Design(OOAD)** 는 코드를 구조적으로 작게 나누는 하나의 방법이었어요.

이와 같은 **Decomposition**처럼, 소프트웨어 개발에서 각각의 모듈들은 서로 다른 **Abstraction** 레벨에서 만들어져요.

큰 컴포넌트부터 작은 서브 시스템의 클래스까지 레벨이 존재해요.

높은 추상 레벨의 컴포넌트는 그보다 조금 낮은 추상 레벨과 인터랙션을 해요.

점점 더 구조적으로 아래로 깊어질수록, 우리가 사용하는 **Elements** 들의 이름은 점점 더 **Concrete** 해져요. 

더 이름이 길어지고, 자세해진다는 말이에요.

WebShop을 예로 들어봅시다. 

가장 높은 추상화 레벨에서의 한 컴포넌트는 Billing이 될 수 있고, 보통 이런 컴포넌트는 좀 더 작은 컴포넌트 혹은 클래스로 구성되어져요.

그 중 하나의 보다 작은 모듈은 할인율을 계산하는 역할(Responsible)을 할 수 있어요.

다른 하나의 모듈은 영수증을 생성하는 역할(responsible)을 하는 모듈이 될 수 있어요. 

따라서 이러한 모듈의 좋은 이름은 **DiscountCalculator**, **LineItemFactory** 가 될 수 있어요.

이렇게 점점 더 낮은 레벨로 분리를 하면 할 수록 클래스, 함수 이름들은 점점 더 **concrete** 해지고 더 길어져요.

`calculateReducedValueAddedTax()` 이 처럼요.

작성하는 추상 레벨에 맞춰 엘리먼트들의 이름을 짓는 연습을 해보아요.

## Avoid Redundancy When Choosing a Name

클래스의 이름 혹은 다른 이름이 멤버 변수의 이름에 들어가는 것은 불필요한 요소에요.

예를 들어
```
class Movie {
 private:
  std::string movieTitle;
};
```
이처럼 이미 Movie class안에 있는 제목에 movie를 붙이는 것은 불필요해요.

```
class Movie {
 private:
  std::string stringTitle;
};
```
이것도 또 다른 경우로 자료형의 타입을 이름에 붙이는 것은 불필요해요.

## Avoid Cryptic Abbreviations

이름을 정할 때, 잘 알려지지 않은 줄임말은 피하세요.

이는 가독성을 크게 줄입니다. 

```
std::size_t idx; // Bad!
std::size_t index; // Good; might be sufficient in some cases
std::size_t customerIndex; // To be preferred, especially in situations where
                           // several objects are indexed

Car ctw; // Bad
Car carToWash // Good

Polygon ply1; // Bad
Polygon firstPolygon // Good

unsigned int nBottles; // Bad!
unsigned int bottleAmount; // Good
unsigned int bottlesPerHour; // Good
```

## Avoid Hungarian Notation and Prefixes

**Hungarian notation** 은 이름을 짓는 하나의 방법이에요.

변수 이름에 타입이나 스코프의 접두사를 포함시켜 이름을 짓는 방법이에요.

예를 들면
```
bool fEnabled; // f = a boolean flag
int nCounter; // n = number type (int, short, ...)
char* pszName; // psz = a pointer to a zero-terinated string
std::string strName; // str = a C++ stdlib string
int m_nCounter; // m_ is member variable, n is number

char* g_pszNotice; // g_ is global variable
int dRange; // d = double-precision floating point. 
```

**Hungarian notation** 은 과거에 심플한 에디터, C언어에서는 유용했을지라도 현대의 개발자 도구에서는 가독성을 방해하는 요인이 될 수 있어요.

다형성을 지원하는 객체 지향 언어에서 접두사는 적용하기 어려워요.

사용하는 것을 지양하는 것이 좋아요.

## Avoid Using the Same Name for Different Purposes

의미 있는 이름과 표현이 좋은 이름을 어떤 Entity에 사용했으면, 다른 목적으로 그 이름을 또 다시 사용하지 마세요. 

이는 더 헷갈리게 해요.

## End

좋은 이름을 짓는 규칙에 대해 알아보았는데, 좋은 이름의 특징은 **self-explanatory** 한 것이 좋다고 생각해요.

이름 자체로 그 엔터티가 어떤 친구인지 알 수 있도록, 이름 스스로 잘 표현이 되는 것이 가장 좋아요. 

이름을 짓는 다양한 방법이 있듯이, 각각의 언어마다 스타일 가이드가 존재해요.

대표적으로 저는 구글 스타일 가이드를 따르려고 하는 편이에요.

**Google C++ Style Guide**, **Google Python Style Guide** 로 검색을 해보시면 구글이 어떤 방식으로 코드를 일관성있게 작성하는지를 알 수 있으니 참고하시는 것도 좋아요.

[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)