# TIL: C 수학 함수 실습 — `ceil`, `floor`, `pow`, `sqrt`, `fabs`

<br>

## 오늘 배운 내용

C에서 수학 함수를 사용하려면 `<math.h>`를 포함한다.

```c
#include <math.h>
```

이번 실습에서는 실수를 올림·내림하고, 거듭제곱과 제곱근을 계산하고, 실수의 절댓값을 구하는 함수를 다뤘다.

---

<br>

## 1. `ceil()`과 `floor()`

- `ceil(x)`: x보다 크거나 같은 정수 중 가장 작은 값으로 올림한다.
- `floor(x)`: x보다 작거나 같은 정수 중 가장 큰 값으로 내림한다.
- 두 함수 모두 결과를 `double`로 반환한다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double result = 0.0;
    double target = 3.58;

    printf("target의 값: %.2f\n", target);

    result = ceil(target);
    printf("ceil(target) 결과: %.2f\n", result);

    result = floor(target);
    printf("floor(target) 결과: %.2f\n", result);

    return 0;
}
```

실행 결과는 다음과 같다.

```text
target의 값: 3.58
ceil(target) 결과: 4.00
floor(target) 결과: 3.00
```

`ceil()`은 소수 부분을 무조건 1 더하는 함수가 아니다. 수직선에서 입력값보다 크거나 같은 가장 가까운 정수를 찾는다. 그래서 음수에서는 다음처럼 동작한다.

```text
ceil(-3.58)  = -3
floor(-3.58) = -4
```

---

<br>

## 2. `fabs()`

`fabs(x)`는 실수 x의 절댓값을 반환한다. 입력값이 음수이면 양수로 바꾸고, 양수이면 그대로 반환한다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double weight1 = 0.0;
    double weight2 = 0.0;
    double difference = 0.0;

    printf("첫 번째 몸무게 입력: ");
    scanf_s("%lf", &weight1);

    printf("두 번째 몸무게 입력: ");
    scanf_s("%lf", &weight2);

    difference = weight1 - weight2;
    printf("뺄셈 결과: %.2f - %.2f = %.2f\n",
           weight1, weight2, difference);

    difference = fabs(weight1 - weight2);
    printf("절댓값 결과: |%.2f - %.2f| = %.2f\n",
           weight1, weight2, difference);

    return 0;
}
```

예를 들어 첫 번째 몸무게가 `68.2`, 두 번째 몸무게가 `70.5`라면 뺄셈 결과는 `-2.30`이지만, `fabs()`를 사용하면 차이의 크기인 `2.30`을 얻는다.

```text
뺄셈 결과: 68.20 - 70.50 = -2.30
절댓값 결과: |68.20 - 70.50| = 2.30
```

`fabs()`는 실수형 절댓값 함수다. 정수형 절댓값을 구할 때는 `<stdlib.h>`의 `abs()`를 사용할 수 있다.

---

<br>

## 3. `pow()`

`pow(x, y)`는 x의 y제곱을 계산한다.

```text
pow(x, y) = xʸ
```

`pow()`는 결과를 `double`로 반환한다. 밑과 지수를 실수로 입력할 수도 있다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double base = 0.0;
    double exponent = 0.0;
    double result = 0.0;

    printf("밑 입력: ");
    scanf_s("%lf", &base);

    printf("지수 입력: ");
    scanf_s("%lf", &exponent);

    result = pow(base, exponent);

    printf("%.2f의 %.2f승 결과: %.2f\n",
           base, exponent, result);

    return 0;
}
```

예를 들어 밑에 `2`, 지수에 `3`을 입력하면 다음 결과가 나온다.

```text
pow(2, 3) = 8
```

실수 계산에는 부동소수점 오차가 생길 수 있다. 따라서 소수 결과를 출력할 때는 원하는 자릿수에 맞춰 표시하고, 결과를 정수와 정확히 같다고 가정하지 않는 것이 좋다.

---

<br>

## 4. `sqrt()`


`sqrt(x)`는 x의 제곱근을 계산한다.

```text
sqrt(x) = √x
```

예를 들어 `sqrt(25)`의 결과는 `5`다. `sqrt()`는 음수가 아닌 값을 대상으로 사용한다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double number = 0.0;
    double result = 0.0;

    printf("제곱근을 구할 실수 입력: ");
    scanf_s("%lf", &number);

    if (number < 0.0)
    {
        printf("음수의 실수 범위 제곱근은 계산할 수 없습니다.\n");
        return 1;
    }

    result = sqrt(number);
    printf("%.2f의 제곱근: %.2f\n", number, result);

    return 0;
}
```

`pow(x, 0.5)`로 제곱근을 구할 수도 있지만, 제곱근을 구하는 목적이라면 `sqrt(x)`를 쓰는 편이 의미가 분명하다.

---

<br>

## 5. 메뉴로 `pow()`와 `sqrt()` 실행하기

강의 화면에서는 `do-while` 반복문과 `switch`문을 사용해 제곱값, 제곱근, 종료 메뉴를 만들었다.

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    int choice = 0;
    double result = 0.0;
    double num1 = 0.0;
    double num2 = 0.0;

    do
    {
        printf("1. 제곱값 출력\n");
        printf("2. 제곱근 출력\n");
        printf("3. 프로그램 종료\n");
        printf("번호 선택(1~3): ");
        scanf_s("%d", &choice);

        switch (choice)
        {
        case 1:
            printf("제곱할 실수 입력: ");
            scanf_s("%lf", &num1);

            printf("지수 입력: ");
            scanf_s("%lf", &num2);

            result = pow(num1, num2);
            printf("%.2f의 %.2f승 결과값: %.2f\n",
                   num1, num2, result);
            break;

        case 2:
            printf("제곱근을 구할 실수 입력: ");
            scanf_s("%lf", &num1);

            if (num1 < 0.0)
            {
                printf("음수의 실수 범위 제곱근은 계산할 수 없습니다.\n");
                break;
            }

            result = sqrt(num1);
            printf("%.2f의 제곱근 결과값: %.2f\n",
                   num1, result);
            break;

        case 3:
            printf("프로그램을 종료합니다.\n");
            return 0;

        default:
            printf("선택 오류! 1, 2, 3 중에서 선택하세요.\n");
            break;
        }
    } while (1);

    return 0;
}
```

`switch`문은 사용자가 입력한 메뉴 번호에 따라 실행할 코드를 선택한다.

- `case 1`: 밑과 지수를 입력받아 `pow()`로 거듭제곱을 계산한다.
- `case 2`: 숫자를 입력받아 `sqrt()`로 제곱근을 계산한다.
- `case 3`: `return 0`으로 프로그램을 종료한다.
- `default`: 메뉴에 없는 번호를 입력했을 때 오류 안내를 출력한다.
- `break`: 현재 `case` 실행을 끝내고 `switch`문 밖으로 이동한다.

---

<br>

## 입력 함수와 출력 형식 지정자

이번 코드에서 `double` 값을 입력받을 때는 다음처럼 `%lf`를 사용한다.

```c
scanf_s("%lf", &number);
```

`printf`에서 `double` 값을 출력할 때는 `%f`를 사용한다. `%.2f`는 소수점 아래 둘째 자리까지 표시한다.

```c
printf("%.2f\n", number);
```

강의 코드에 보이는 `scanf_s`는 Visual Studio에서 사용하는 입력 함수다. 다른 컴파일러에서는 `scanf`를 사용해야 할 수 있다.

---

<br>

## 함수 한눈에 보기

| 함수 | 역할 | 예시 | 결과 |
| --- | --- | --- | --- |
| `ceil(x)` | x 이상인 가장 가까운 정수로 올림 | `ceil(3.58)` | `4.0` |
| `floor(x)` | x 이하인 가장 가까운 정수로 내림 | `floor(3.58)` | `3.0` |
| `pow(x, y)` | x의 y제곱 | `pow(2, 3)` | `8.0` |
| `sqrt(x)` | x의 제곱근 | `sqrt(25)` | `5.0` |
| `fabs(x)` | 실수 x의 절댓값 | `fabs(-2.5)` | `2.5` |

---

<br>

## 헷갈렸던 점과 해결 과정

- **`ceil()`은 반올림 함수인가?**  
  아니다. `ceil()`은 입력값보다 크거나 같은 정수 방향으로 올린다. `floor()`는 입력값보다 작거나 같은 정수 방향으로 내린다.

- **음수에서 올림과 내림은 어떻게 될까?**  
  `ceil(-3.58)`은 `-3`, `floor(-3.58)`은 `-4`다. 소수점 자릿수를 단순히 지우는 동작이 아니라 수직선의 정수 방향을 기준으로 계산한다.

- **`fabs()`와 `abs()`는 같은 함수인가?**  
  둘 다 절댓값을 구하지만 `fabs()`는 실수형, `abs()`는 정수형에 사용한다.

- **`scanf_s`에서 `double`을 입력받을 때 어떤 형식 지정자를 쓸까?**  
  `%lf`를 사용한다. `printf`로 `double`을 출력할 때는 `%f`를 사용한다.

---

<br>

## 오늘 배운 내용 요약

`<math.h>`를 포함하면 실수 계산에 사용할 수학 함수를 사용할 수 있다. `ceil()`과 `floor()`는 각각 올림과 내림, `pow()`는 거듭제곱, `sqrt()`는 제곱근, `fabs()`는 실수의 절댓값을 구한다. `double` 입력에는 `scanf_s`의 `%lf`를 사용하고, 출력에는 `printf`의 `%f`를 사용한다.
