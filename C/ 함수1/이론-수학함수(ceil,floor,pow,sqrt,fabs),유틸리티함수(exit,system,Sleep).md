# TIL: C 표준 라이브러리 함수

## 오늘 배운 내용

라이브러리 함수는 C 언어에서 자주 쓰는 기능을 미리 만들어 둔 함수다. 함수를 사용하려면 해당 함수의 선언이 들어 있는 헤더 파일을 포함해야 한다.

오늘은 수학 관련 함수와 유틸리티 함수를 순서대로 살펴봤다.

---

<br>

## 1. 수학 함수 사용 준비

수학 함수는 `math.h` 헤더 파일을 포함해서 사용한다. 이 헤더에는 올림, 내림, 절댓값, 제곱, 제곱근 등을 계산하는 함수의 선언이 들어 있다.

```c
#include <stdio.h>
#include <math.h>
```

수학 함수는 소수점이 있는 값을 다루는 경우가 많으므로, 보통 `double`형 변수와 함께 사용한다.

---
<br>

## 2. `ceil()`과 `floor()`

`ceil()`은 값을 올림하고, `floor()`는 값을 내림한다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double value = 35.8;

    printf("올림: %.2f\n", ceil(value));
    printf("내림: %.2f\n", floor(value));

    return 0;
}
```

```text
올림: 36.00
내림: 35.00
```

양수에서 `ceil()`은 소수점 이하가 있으면 다음 정수로 올리고, `floor()`는 소수점 이하를 버려 아래 정수로 내린다.

정확하게는 소수점 자릿수를 단순히 없애는 기능이 아니라, 각각 **양의 무한대 방향으로 올림**하고 **음의 무한대 방향으로 내림**한다. 따라서 음수에서는 결과가 달라질 수 있다.

```text
ceil(-3.2)  = -3
floor(-3.2) = -4
```

두 함수의 반환형은 `double`이다.

---

<br>

## 3. `fabs()`

`fabs()`는 실수의 절댓값을 구한다. 음수의 부호를 없애 양수로 바꿔 준다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double weight1 = 79.0;
    double weight2 = 85.0;
    double difference = weight1 - weight2;

    printf("뺄셈 결과: %.2f\n", difference);
    printf("절댓값: %.2f\n", fabs(difference));

    return 0;
}
```

```text
뺄셈 결과: -6.00
절댓값: 6.00
```

`fabs()`는 실수형 절댓값을 구할 때 사용한다. 정수의 절댓값에는 `abs()`를 사용할 수 있다.

---

<br>

## 4. `pow()`와 `sqrt()`

`pow()`는 거듭제곱을 계산하고, `sqrt()`는 제곱근을 계산한다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double number = 5.0;

    printf("제곱: %.2f\n", pow(number, 2.0));
    printf("제곱근: %.2f\n", sqrt(number));

    return 0;
}
```

```text
제곱: 25.00
제곱근: 2.24
```

함수의 형태는 다음과 같다.

```c
pow(밑, 지수);
sqrt(값);
```

`pow(5.0, 2.0)`은 `5²`을 계산하고, `sqrt(5.0)`은 5의 제곱근을 계산한다. 두 함수 모두 실수 결과를 반환한다.

`sqrt()`에 음수를 전달하면 실수 범위에서 계산할 수 없으므로, 입력값이 0 이상인지 확인해야 한다.

---

<br>

## 5. `exit()`

`exit()`는 프로그램을 즉시 종료하는 함수다. 사용하려면 `stdlib.h`를 포함한다.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    printf("프로그램을 종료합니다.\n");
    exit(0);

    printf("이 문장은 실행되지 않습니다.\n");
}
```

괄호 안에는 종료 상태를 전달한다.

- `exit(0)`: 일반적으로 정상 종료를 의미한다.
- `exit(1)`: 일반적으로 오류가 발생한 종료를 의미한다.

`return 0;`은 `main()` 함수에서 반환하면서 프로그램을 끝낸다. `exit(0);`은 호출된 위치에서 프로그램 전체를 종료한다.

---

<br>

## 6. `system()`

`system()`은 문자열로 전달한 명령을 운영체제의 명령 처리기에 전달해 실행한다. 사용하려면 `stdlib.h`를 포함한다.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    printf("현재 디렉터리의 파일 목록:\n");
    system("dir");

    return 0;
}
```

강의 예제에서는 Windows 명령어인 `dir`로 현재 디렉터리의 파일 목록을 출력하고, `cls`로 콘솔 화면을 지웠다.

```c
system("dir");
system("cls");
```

`dir`과 `cls`는 Windows 명령어이므로 다른 운영체제에서는 동작하지 않을 수 있다. 또한 `system()`에 외부 입력을 그대로 넣으면 의도하지 않은 명령이 실행될 수 있으므로 주의해야 한다.

---

<br>

## 7. `Sleep()`

`Sleep()`은 Windows에서 지정한 시간 동안 프로그램의 실행을 잠시 멈춘다. 함수 이름의 첫 글자는 대문자 `S`이며, `windows.h`에 선언되어 있다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <windows.h>

int main(void)
{
    printf("10초 뒤 화면을 지웁니다.\n");

    Sleep(10000);
    system("cls");

    return 0;
}
```

`Sleep()`의 인자는 밀리초 단위다.

```text
1000밀리초 = 1초
Sleep(10000) = 10초 대기
```

`Sleep()`은 Windows 전용 함수다. 다른 운영체제에서 사용하는 대기 함수와 단위나 이름이 다를 수 있다.

---

<br>

## 헷갈린 점과 해결 과정

- `ceil()`은 항상 숫자에 1을 더하는 함수가 아니다. 이미 정수인 값은 그대로 반환하며, 실수에서는 올림한 정수값을 `double`로 반환한다.
- `floor()`는 양수에서 소수 부분을 버리는 것처럼 보이지만, 정확히는 아래 방향으로 내림한다. 그래서 `floor(-3.2)`는 `-4`다.
- `fabs()`는 실수의 절댓값을 구하고, `abs()`는 정수의 절댓값을 구한다.
- `Sleep(10000)`의 `10000`은 초가 아니라 밀리초이므로 10초를 의미한다.
- `exit()`와 `return`은 종료 범위가 다르다. `exit()`는 프로그램 전체를 종료하고, `return`은 현재 함수를 끝낸다.

---
<br>

## 오늘 배운 내용 정리

- 수학 함수의 선언을 사용하려면 `math.h`를 포함한다.
- `ceil()`은 올림, `floor()`는 내림, `fabs()`는 실수의 절댓값을 구한다.
- `pow()`는 거듭제곱, `sqrt()`는 제곱근을 계산한다.
- `exit()`는 프로그램을 종료하고 종료 상태를 전달한다.
- `system()`은 운영체제 명령을 실행하며, 명령어는 운영체제에 따라 다를 수 있다.
- Windows의 `Sleep()`은 인자로 받은 밀리초만큼 실행을 대기한다.
