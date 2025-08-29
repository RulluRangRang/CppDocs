# 2025.08.28 p1 ~ p72
    - 2 영 초기화부터 시작

## import <iostream>
- C++20 이상 부터 import 사용 가능
- MSVC에서는 문제없이 import 사용 가능
- Apple Clang 12.0.0(유사한 GCC 버전)은 -std=c++2a -fmodules-ts 옵션 사용해야 사용 가능
- C 표준 라이브러리(예: stdio.h)는 import문으로 호출이 안될 수 있으므로, #include로 처리해서 호출

## C++ 빌드 작업 단계
1) 전처리(ProProcess)
    - 소스코드에 담긴 메타 정보를 처리

2) 컴파일(Compile)
    - 소스코드를 머신이 읽을 수 있는 객체 파일로 변환

3) 링크(Link)
    - 앞에서 변환한 여러 객체 파일을 애플리케이션으로 엮음

## endl
    - endl은 스트림에 줄바꿈 문자를 추가한 뒤 현재 버퍼에 있는 내용을 출력 장치로 내보낸다. endl은 성능에 영향을 미치기 때문에 특히 루프와 같은 문장에서 남용하면 좋지 않다. 반면, \n도 스트림에 줄바꿈 문자를 추가하지만 버퍼를 자동으로 비우지 않는다.

## C++에서 printf, scanf를 사용하지 않는 이유
    - printf, scanf는 타입 안정성(type safety)를 보장하지 않는다

## 스코프 지정 연산자(scope resolution operator, ::)

## using문은 절대 헤더파일에 사용 금지
    - 해당 헤더파일을 참조하는 모든 파일에서 동일한 using 지정방식대로 호출해야 하기 때문

## 리터럴(literal)
    - 10진수: 123
    - 8진수: 0173
    - 16진수: 0x7B
    - 2진수: 0b1111011
    - 숫자 리터럴: 23'456'789, 0.234'456f
        - '는 자릿수 구분자(digits separator)

## 균일 초기화(uniform initialization)
    - 균일 초기화는 2011년 C++11 버전에서 도입
    - 균일 초기화 사용 이유: 1.1.25절

```cpp
// 기존 초기화 int initializedInt = 7;

int initializedInt { 7 };
```


## 타입별 바이트 크기
    - int: 대부분 4바이트 표현
    - short: 대부분 2바이트 표현
    - long: 대부분 4바이트 표현
    - long long: 주로 8바이트 표현

## 타입 suffix

```cpp
float f { 7.2f };
double d { 7.2 };
long double ld { 16.98L };
char ch { 'm' };
char8_t c8 { u8'm' };
char16_t c16 { u'm' };
char32_t c32 { U'm' };
wchar_t w { L'm' };
bool b { true };
```

## #include <limits>를 쓰지 않아도 numeric_limits<int>가 동작하는 이유
    - iostream에 우연히 limits가 포함되었기 때문에 동작
    - 단, 컴파일러 버전/빌드 옵션/모듈로 옮겨지면 바로 깨질 수 있으므로, 명시적으로 #include <limits>를 써주는 것이 좋다. 
    - 동일하게 std::format도 #include <format>을 써주는 것이 좋다. 

# 2025.08.29 p73 ~ p

## 0 초기자
    - 유니폼 초기자에서 { 0 } 0을 생략해도 자동으로 0으로 초기화됨.
    - 기본 정수 타임(char, int 등)은 0, 기본 부동소수점 타입(0.0), 포인터 타입은 nullptr로 초기화됨

## 타입 캐스트
    - 세 번째 방법을 많이 사용함.
    - 첫 번째 방법은 C Style
    - 두 번째 방법은 사용안함. 단, 가능은 함.
```cpp
float myFlot { 3.14f }
int i1 { (int)myFloat };    // 첫 번째 방법
int i2 { int(myFloat) };    // 두 번째 방법
int i3 { static_cast<int>(myFloat) }; // 세 번째 방법
```

## (책외) enum과 enum class의 차이
    - 전통적인 enum
        - 암시적 정수 변환
        - 스코프 오염(전역 스코프 노출로 다른 enum과 같은 이름이 있으면 충돌위험)
        - 기본 타입 지정 불가(C++11 이전)
    - enum class(C++11 부터 지원)
        - 스코프 한정 가능
        - 암시적 변환 금지(반드시 명시적 캐스팅 필요)
        - 타입 안정성(다른 enum class끼리 비교할 수 없음)
            
```cpp
// 암시적 변환 금지 예시
int x = Color::RED;     // ❌ 컴파일 오류
int y = (int)Color::RED; // ⭕ 명시적 캐스팅 필요
```

## enum 주의 사항
    - 열거(enum) 타입은 강타입이 아니고(=type-unsafe), 강타입 버전인 enum class를 쓰는 것이 좋음.


## 구조체
    - .ccpm은 모듈 인터페이스 파일의 확장자

```cpp
// employee.ccpm
export module employee;

export struct Employee{
    char firstInitial;
    char lastInitial;
    int employeeNumber;
    int salary;
};
```

## class가 있음에도 struct가 존재해야 하는 이유
    - C와의 호환성 때문에 struct는 유지
    - 의미적 구분으로 struct는 단순 데이터, class는 캡슐화된 객체라는 관례가 널리 쓰임

## switch case 구문 fallthrough
    - case문에 break가 없을 경우, break가 없다고 경고 메시지가 뜸
    - 의도된 기능으로 컴파일러에게 알려주기 위해 [[fallthrough]]를 사용

```cpp
switch(mode){
    using enum Mode;

    case Custom:
        value = 84;
        [[fallthrogh]]
    case Standard:
        ...
}
```

## <=> 3방향 비교 연산자(C++20 추가)
    - 우주선 연산자
    - #include <compare>에 정의된 열거 타입으로 리턴
        <피연산자가 정수 타입의 경우>
        - strong_ordering::less: 첫 번째 피연산자가 두 번째 피연산자보다 작다.
        - strong_ordering::greater: 첫 번째 피연산자가 두 번째 피연산자보다 크다.
        - strong_ordering::equal: 두 피연산자가 같다.
        <피연산자가 부동소수점 타입의 경우>
        - partial_ordering::less: 첫 번째 피연산자가 두 번째 피연산자보다 작다.
        - partial_ordering::greater: 첫 번째 피연산자가 두 번째 피연산자보다 크다.
        - partial_ordering::equivalent: 두 피연산자가 같다.
        - partial_ordering::unordered: 두 피연산자 중 하나는 숫자가 아니다.
        - 단, 암시적 변환 가능하므로, partial_ordering에 정수 타입 비교 가능
        <피연산자가 직접 정의한 타입의 경우>
        - weak_ordering::less: 첫 번째 피연산자가 두 번째 피연산자보다 작다.
        - weak_ordering::greater: 첫 번째 피연산자가 두 번째 피연산자보다 크다.
        - weak_ordering::equivalent: 두 피연산자가 같다.

## #include <compare>
    - std::is_eq(), is_neq(), is_lt(), is_lteq(), is_gt(), is_gteq() 제공
    - 반환결과 true/false

## 단락 논리(short-circuit logic, 축약 논리)
    - 표현식 도중 최종 결과가 나오면 나머지 조건은 평가하지 않는다.

```cpp
bool result { bool1 || bool2 || (i>7) || (27 / 13 % i + 1) < 2>};
// bool2가 true면 나머지 뒷 단계는 검증조차 하지 않음
```

## 함수 반환 타입 추론

```cpp
auto addNumbers(int number1, int number2){
    return number1 + number2;   // int + int = int 반환(컴파일러가 추론)
}
```

## 현재 호출함수 이름 출력

```cpp
int addNumber(int number1, int number2) {
    cout<< "Entering function " << __func__ <<endl;
    return number1 + number2;
}
```

## 함수 오버로딩(overloading)
    - 함수 이름은 같지만 매개변수 구성은 다른 함수를 여러 개 제공

```cpp
int addNumbers(int a, int b){ return a + b; }
double addNumbers(double a, double b) { return a + b; }

cout << addNumbers(1, 2) << endl;           // int 버전 호출
cout << addNumbers(1.11, 2.22) << endl;     // double 버전 호출
```

## 어트리뷰트(Attribute)
    - 소스코드에 벤더에서 제공하는 정보나 옵션을 추가하는 메커니즘
    - C++11부터 [[어트리뷰트]]와 같이 대괄호 형식으로 사용하도록 표준화 됨.


```cpp
// [[nodiscard]] (C++20)
// 반환 값에 아무런 작업하지 않으면 컴파일러 경고 출력
[[nodiscard]] int func(){
    return 42;
}

int main(){
    func();
}

// [[maybe_unused]]
// 파라미터를 사용하지 않으면 컴파일러 경고 출력
int func(int param1, [[maybe_unused]] int param2){
    return 42;
}
```