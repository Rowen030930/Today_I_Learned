# TIL: 함수의 장점과 `rand()`, `srand()` 함수

<br><br>

## 오늘 배운 내용

함수는 프로그램에서 특정한 기능을 수행하도록 따로 정의한 코드의 묶음이다. 필요한 곳에서 함수를 호출하면 그 안에 작성한 기능을 실행할 수 있다.

`rand()`와 `srand()`는 C 표준 라이브러리에서 제공하는 함수다. `rand()`는 난수를 만들고, `srand()`는 난수 생성에 사용할 시작값(seed)을 설정한다.

<br>

## 함수의 장점

### 1. 코드 중복을 줄일 수 있다

같은 기능을 여러 번 사용해야 할 때, 매번 같은 코드를 작성하는 대신 함수로 만들어 호출할 수 있다.

```c
#include <stdio.h>

void printLine(void)
{
    printf("--------------------\n");
}

int main(void)
{
    printLine();
    printf("함수 예제\n");
    printLine();

    return 0;
}
```

`printLine()`을 호출하면 같은 구분선을 여러 곳에서 출력할 수 있다.

### 2. 복잡한 문제를 작은 단위로 나누어 단순화할 수 있다

프로그램을 기능별 함수로 나누면 각 함수가 맡은 일을 파악하기 쉽다.

```c
#include <stdio.h>

void printTitle(void)
{
    printf("숫자 출력 프로그램\n");
}

void printNumber(int number)
{
    printf("입력한 숫자: %d\n", number);
}

int main(void)
{
    printTitle();
    printNumber(10);

    return 0;
}
```

`printTitle()`은 제목을 출력하고, `printNumber()`는 숫자를 출력한다. 기능을 나누면 프로그램 전체를 한 번에 이해하는 것보다 각 부분을 살펴보기가 쉽다.

### 3. 함수를 삭제하거나 수정하기 전까지 반복해서 재사용할 수 있다

한 번 정의한 함수는 필요한 곳에서 여러 번 호출할 수 있다.

```c
#include <stdio.h>

void greet(void)
{
    printf("안녕하세요!\n");
}

int main(void)
{
    greet();
    greet();
    greet();

    return 0;
}
```

`greet()` 함수를 한 번 정의한 뒤 세 번 호출해 같은 인사말을 출력한다.

### 4. 기능을 수정할 때 함수만 고치면 된다

같은 기능을 여러 곳에서 사용하더라도, 함수 안의 코드를 수정하면 그 함수를 호출하는 곳마다 변경된 기능이 적용된다.

```c
#include <stdio.h>

void printMessage(void)
{
    printf("처음 메시지\n");
}

int main(void)
{
    printMessage();
    printMessage();

    return 0;
}
```

출력 문구를 바꾸고 싶다면 `printMessage()` 안의 `printf()`를 수정하면 된다. 호출한 부분을 하나씩 찾아 수정할 필요가 없다.

<br>

## 함수 호출

함수 이름 뒤에 소괄호 `()`를 붙여 실행하는 것을 **함수 호출**이라고 한다.

```c
rand(); // rand 함수 호출
```

함수에 전달할 값이 있다면 소괄호 안에 넣는다.

```c
srand(10); // seed 값 10을 전달하며 srand 함수 호출
```

<br>

## `rand()` 함수

`rand()`는 `0`부터 `RAND_MAX` 사이의 의사 난수를 반환한다. 반환되는 숫자의 구체적인 범위는 사용하는 환경의 `RAND_MAX` 값에 따라 달라진다.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    printf("%d\n", rand());

    return 0;
}
```

`rand()`만 호출하면 프로그램을 실행할 때마다 같은 순서의 숫자가 나올 수 있다. 실행할 때마다 다른 순서의 숫자를 얻고 싶다면 `srand()`로 seed를 설정해야 한다.

<br>

## `srand()` 함수로 seed 설정하기

`time(NULL)`은 현재 시간을 구하는 데 사용한다. 현재 시간을 `srand()`에 전달하면 프로그램을 실행한 시점에 따라 seed가 달라질 수 있다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(void)
{
    srand((unsigned int)time(NULL));

    printf("%d\n", rand());

    return 0;
}
```

`time(NULL)`의 반환값은 `time_t` 자료형이다. `srand()`는 `unsigned int` 값을 받기 때문에 예제에서는 `(unsigned int)`로 형 변환했다. 이는 시간값에 음수가 절대 없다는 뜻이 아니라, `srand()`의 매개변수 자료형에 맞춰 전달하는 것이다.

일반적으로 `srand()`는 난수를 만들 때마다 반복 호출하지 않고, 프로그램을 시작할 때 한 번 호출한다.

<br>

## `rand()`로 1부터 45까지의 숫자 만들기

`rand() % 45`의 결과는 `0`부터 `44`까지다. 여기에 `1`을 더하면 `1`부터 `45`까지의 숫자를 얻을 수 있다.

```c
1 + (rand() % 45)
```

강의 실습에서 사용한 예제는 다음과 같다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define NUM 45

int main(void)
{
    int cnt;

    srand((unsigned int)time(NULL));

    printf("1부터 45까지의 숫자 6개를 출력합니다.\n");

    for (cnt = 1; cnt <= 6; cnt++)
    {
        printf("%d ", 1 + (rand() % NUM));
    }

    printf("\n");

    return 0;
}
```

이 코드는 `for`문을 여섯 번 반복하면서 `1`부터 `45` 사이의 숫자를 출력한다. 중복을 허용하므로 같은 숫자가 여러 번 나올 수도 있다.

<br>

## 헷갈린 점과 해결 과정

- `rand()` 뒤의 `()`는 함수 호출을 나타낸다. 함수 이름만 쓰는 것과 실제로 함수를 실행하는 것은 다르다.
- `rand()`는 범위가 정해진 의사 난수를 반환한다. `rand() % 45`는 `0~44`, `1 + rand() % 45`는 `1~45` 범위로 조정된다.
- `srand()`는 난수를 직접 만드는 함수가 아니라 `rand()`가 사용할 seed를 설정하는 함수다.
- `srand((unsigned int)time(NULL))`에서 `unsigned int`는 `srand()`에 전달할 값의 자료형에 맞춘 형 변환이다.
- `rand()`로 숫자 여섯 개를 뽑아도 중복되지 않는다는 보장은 없다.

<br>

## 실습 기록

- `rand()`를 호출해 숫자를 출력하는 코드를 살펴봤다.
- `%` 연산과 `1 +`를 이용해 난수의 범위를 `1~45`로 조정했다.
- `for`문으로 난수를 여섯 번 출력했다.
- `time(NULL)`을 이용해 `srand()`의 seed를 설정하는 코드를 확인했다.

<br>

## 참고 자료

- 따즈아, **2강 함수-1 (실습: rand, srand 함수 예제 실습)**  
  함수의 장점과 `rand()`, `srand()`를 사용한 난수 생성 예제를 학습했다.

<br><br>

## 오늘 배운 내용 정리

함수는 특정 기능을 따로 정의해 필요할 때 호출하는 코드의 묶음이다. 함수를 사용하면 코드 중복을 줄이고, 복잡한 프로그램을 기능별로 나눌 수 있으며, 같은 기능을 재사용하고 수정하기도 편해진다.

`rand()`는 의사 난수를 반환하고, `srand()`는 난수 생성에 사용할 seed를 설정한다. `rand() % 45`에 `1`을 더하면 `1~45` 범위의 숫자를 만들 수 있다.
