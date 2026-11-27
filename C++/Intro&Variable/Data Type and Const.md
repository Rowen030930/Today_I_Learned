# TIL: C++ 자료형과 `const`

## 오늘 배운 내용

<br>

### 자료형이란?

자료형은 변수가 어떤 종류의 값을 저장할지 정한다. 정수와 실수처럼 값의 종류가 다르면 사용하는 자료형도 달라진다.

```cpp
int age = 24;               // 정수
double height = 178.5;      // 소수점이 있는 수
char grade = 'A';           // 문자 하나
bool isLearning = true;     // 참 또는 거짓
```

자료형은 저장할 수 있는 값의 종류와 표현 범위, 메모리 사용 방식에 영향을 준다. 어떤 자료형을 선택할지는 그 변수에 담을 값의 성격을 기준으로 정한다.

| 자료형 | 주로 저장하는 값 | 예시 |
|---|---|---|
| `int` | 정수 | `24`, `-10` |
| `float` | 소수점이 있는 수 | `3.14f` |
| `double` | 더 정밀한 소수점 값 | `178.5` |
| `char` | 문자 하나 | `'A'` |
| `bool` | 참 또는 거짓 | `true`, `false` |

`float` 값에는 보통 숫자 뒤에 `f`를 붙인다. `double`은 소수점이 있는 수를 나타내는 기본 자료형으로 자주 사용한다.

```cpp
float speed = 3.5f;
double distance = 120.75;
```

### `sizeof`로 자료형의 크기 알아보기

`sizeof`는 자료형이나 변수의 크기를 바이트 단위로 구한다.

```cpp
#include <iostream>

int main()
{
    std::cout << "int 크기: " << sizeof(int) << "바이트\n";
    std::cout << "double 크기: " << sizeof(double) << "바이트\n";

    return 0;
}
```

`sizeof(int)`처럼 자료형 이름을 넣을 때는 괄호를 사용한다. 실제 크기는 자료형과 컴파일 환경에 따라 달라질 수 있으므로, 모든 컴퓨터에서 같은 숫자가 나온다고 가정하면 안 된다. `sizeof(char)`는 항상 1바이트다. :chatgpt-content-reference{index="0"}

### `const` — 값을 바꾸지 못하게 하기

`const`는 변수를 선언할 때 붙여, 해당 변수의 값을 나중에 바꾸지 못하게 한다. `const` 변수는 선언하면서 값을 정해 주어야 한다.

```cpp
const int maxHealth = 100;
const double gravity = 9.8;
```

다음 코드는 정상적으로 사용할 수 있다.

```cpp
#include <iostream>

int main()
{
    const int maxHealth = 100;

    std::cout << "최대 체력: " << maxHealth << '\n';

    return 0;
}
```

초기화한 뒤 값을 다시 대입하려고 하면 컴파일 오류가 발생한다.

```cpp
const int maxHealth = 100;

// maxHealth = 200;  // 컴파일 오류: const 변수는 값을 바꿀 수 없다.
```

`const`는 실수로 값이 바뀌는 것을 막아 준다. 게임에서 변하지 않아야 하는 설정값처럼, 처음 정한 뒤 계속 같은 값이어야 하는 변수에 사용할 수 있다.

```cpp
const int maxAmmo = 30;
const float criticalDamageMultiplier = 2.0f;
```

<br>

## 실습 코드

아래 코드는 여러 기본 자료형의 변수와 `const` 변수를 선언하고 출력한다.

```cpp
#include <iostream>

int main()
{
    int playerLevel = 10;
    double playerHealth = 95.5;
    char rank = 'A';
    bool isAlive = true;

    const int maxHealth = 100;

    std::cout << "레벨: " << playerLevel << '\n';
    std::cout << "현재 체력: " << playerHealth << '\n';
    std::cout << "등급: " << rank << '\n';
    std::cout << "생존 여부: " << std::boolalpha << isAlive << '\n';
    std::cout << "최대 체력: " << maxHealth << '\n';

    std::cout << "int 크기: " << sizeof(int) << "바이트\n";
    std::cout << "double 크기: " << sizeof(double) << "바이트\n";

    return 0;
}
```

예상되는 출력 형태는 다음과 같다. `sizeof` 결과는 컴파일 환경에 따라 달라질 수 있다.

```text
레벨: 10
현재 체력: 95.5
등급: A
생존 여부: true
최대 체력: 100
int 크기: 컴파일 환경에 따라 달라짐
double 크기: 컴파일 환경에 따라 달라짐
```

<br>

## 헷갈린 점과 해결 과정

### `const`는 자료형인가?

`const` 자체는 `int`, `double` 같은 자료형이 아니다. 기존 자료형에 조건을 붙여 그 변수의 값을 바꾸지 못하게 하는 키워드다.

```cpp
const int maxHealth = 100;
```

여기서 자료형은 `int`이고, `const`가 이 변수의 값을 변경하지 못하도록 한다. C++에서 `const`는 값이 의도치 않게 바뀌지 않도록 컴파일 시점에 제한을 걸어 준다. :chatgpt-content-reference{index="1"}

### `const` 변수는 선언만 해도 되는가?

값을 나중에 대입할 수 없으므로, 선언과 동시에 초기화해야 한다.

```cpp
const int maxHealth = 100;  // 정상
```

### `sizeof(int)`의 결과는 항상 같은가?

아니다. 자료형 크기는 컴파일러와 플랫폼에 따라 달라질 수 있다. 직접 확인하려면 `sizeof`를 사용한다.

<br>

## 오늘 내용 정리

- 자료형은 변수가 저장할 값의 종류를 정한다.
- `int`, `double`, `char`, `bool`은 서로 다른 종류의 값을 표현한다.
- `sizeof`는 자료형이나 변수의 크기를 바이트 단위로 확인할 때 사용한다.
- `sizeof` 결과는 일부 자료형에서 플랫폼과 컴파일 환경에 따라 달라질 수 있다.
- `const`는 변수의 값을 바꾸지 못하도록 제한한다.
- `const` 변수는 선언할 때 초기값을 지정해야 한다.
