# inline
- ODR(One Definition Rule) 문제 없이 헤더에 정의를 넣을 있게 해주는 링킹 키워드
- 성능 관점에서 컴파일러가 최적화 결정
- 여러번 정의 가능
- 설정 값/상수 객체를 헤더 하나로 배포할 때 유용(헤더 전역변수 정의, C++17)
- C++에서는 static inline을 잘 쓰지 않는다.

```cpp
// config.hpp
#pragma once
inline int default_port = 5432;
inline std::string app_name = "IF";
```