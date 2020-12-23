# Unit Test

## Rules for good unit tests

이 문서는 [Clean C++](https://book.naver.com/bookdb/book_detail.nhn?bid=13740244)을 읽고, 정리를 했던 글 입니다. 

### 1. Test Code Quality

더 질 좋은 유닛 테스트 코드를 만들기 위해서는 테스트 코드와 실제 코드의 요구사항을 동일시해야 해요. 

### 2. Unit Test Naming

유닛 테스트가 실패하였을 때 개발자는 바로 어떤 테스트가 실패하였는지, 어떤 환경에서 실패하였는지, 실제 결과와 실패한 결과를 즉시 알아야 해요.

그렇기 때문에 유닛 테스트의 이름은 굉장히 중요해요. 

좋은 이름은

 - 테스트 시나리오의 전제 조건
 - 테스트 기능 부분 (프로시저, 함수, API의 이름)
 - 테스트 예상 결과

를 포함하면 좋아요.

예를 들면 C++에서는 

```
PreconditionAndStateOfUnitTest_TestedPartOfAPI_ExpectedBehavior

CustomerCacheTest::cacheIsEmpty_addElement_sizeIsOne()
```

### 3. Unit Test Independence

유닛 테스트가 서로 영향을 끼치면 안되요. 독립적이어야 해요.

주로 발생할 수 있는 상황은 글로벌한 변수를 사용하였을 경우가 있어요.

Singleton 객체나 static한 멤버 변수를 사용하였을 경우에는 조심해야 해요.

이 경우에는 의존성 주입 (DI)를 고려하면 더 좋아요.

### 4. One Assertion per Test

여러 테스트를 한 번에 테스트 한다면, 실패하였을 때 어떤 이유 때문인지 빠르게 캐치하기 어려워요. 

그리고 한 에러가 이어서 다른 테스트에 영향을 끼칠 수도 있기 때문이에요.

### 5. Independent Initialization of Unit Test Environments

테스트를 매번 새롭게 초기화하여 사용해야 해요.

그래서 테스트가 완료되었을 때, 관련된 모든 state들은 제거되어야 해요.

### 6. Don't Mix Test Code with Production Code

테스트 코드와 실제 코드를 같은 공간에 합치지 마세요. 

개별적으로 분리합시다.

## TDD (Test Driven Development)

TDD는 90년대 후반 Kent Beck에 의해 고안된 방법으로 코드가 지속적으로 수정되더라도, 

수정된 코드가 side effect 없이 정상적으로 동작할 수 있도록 지원해주는 개발 방법론이에요.

대표적으로 구글에서는 TDD를 기본으로 하여 테스트로 검증이 되지 않은 소스는 반영하지 못 해요.

실제 많은 기업에서도 TDD로 개발을 하기 때문에, 테스트 코드를 작성하는 것에도 관심을 갖으면 좋아요.

TDD를 활용하면 2가지 장점이 있어요.

- 테스트를 통해 리팩토링으로 인한 side effect를 줄이고, 코드를 깔끔하게 유지할 수 있어요

- 구현 시 코드 구현부에 대한 인터페이스에 대해 집중할 수 있어, 사용성 관점에서 깊게 고려해볼 수 있어요

TDD 방법론을 고려하면 기능을 구현할 때 다음과 같은 단계로 진행할 수 있어요.

1. 구현하고자 하는 기능에 대한 테스트 코드 작성

2. 테스트가 통과하기 위한 기능적인 코드 작성

3. 테스트가 통과되는 범위 내에 지속적인 리팩토링 가능

TDD 관련하여 아래 서적을 참고하면 지속적으로 공부하는데 도움이 됩니다.

- 테스트 주도 개발 / 켄트 벡 : https://book.naver.com/bookdb/book_detail.nhn?bid=7443642

- 리팩토링 / 마틴 파울러 : https://book.naver.com/bookdb/book_detail.nhn?bid=16311029


사례

- 배달의 민족 정산시스템팀 : https://woowahan.oopy.io/60a06399-3f95-4fec-a436-000ad6baff40