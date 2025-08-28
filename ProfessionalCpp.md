# 2025.08.25 p1 ~ p72
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
