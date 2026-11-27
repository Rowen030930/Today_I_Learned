# TIL: 6강 함수-1 — 유틸리티 함수

<br>

## `exit()`, `system()`, `Sleep()`

<br>

## 오늘 배운 내용

유틸리티 함수는 프로그램의 핵심 계산을 하기보다는 프로그램을 종료하거나, 운영체제 명령을 실행하거나, 일정 시간 기다리는 등 주변 작업을 도와주는 함수다.

오늘 본 함수는 다음과 같다.

| 함수 | 하는 일 | 종류 |
|---|---|---|
| `exit()` | 실행 중인 프로그램을 종료한다 | C 표준 라이브러리 |
| `system()` | 운영체제의 명령 처리기에 명령을 전달한다 | C 표준 라이브러리 |
| `Sleep()` | 현재 실행 중인 스레드를 일정 시간 멈춘다 | Windows API |

---

<br>

## 1. `exit()` — 프로그램 종료하기

`exit()`는 호출되는 즉시 프로그램 전체를 종료한다. 아래 예제처럼 반복문 안에서 호출하면, 반복문뿐 아니라 프로그램 자체가 끝난다.

```c
#include <stdio.h>
#include <stdlib.h>  // exit(), EXIT_SUCCESS 사용

int main(void)
{
    int cnt = 0;
    int hap = 0;

    while (1) {
        hap += cnt;

        if (hap >= 100) {
            printf("합계가 100 이상이 되었습니다.\n");
            exit(EXIT_SUCCESS);  // 프로그램 전체 종료
        }

        printf("현재 cnt: %d, 합계: %d\n", cnt, hap);
        cnt++;
    }

    // exit()가 실행되므로 이 위치까지 도달하지 않는다.
    printf("이 문장은 실행되지 않습니다.\n");

    return 0;
}
```

`hap`에는 매 반복마다 `cnt`가 더해진다.

```text
cnt = 0일 때 hap = 0
cnt = 1일 때 hap = 1
cnt = 2일 때 hap = 3
...
cnt = 14일 때 hap = 105
```

합계가 100 이상이 된 순간 `exit(EXIT_SUCCESS)`가 실행되므로 프로그램은 그 자리에서 끝난다. 따라서 그 뒤의 `printf()`와 `return 0`은 실행되지 않는다.

### 종료 상태 값

`exit()`에 전달하는 값은 운영체제에 프로그램의 종료 결과를 알려준다.

```c
exit(EXIT_SUCCESS);  // 정상 종료를 나타내는 값
exit(EXIT_FAILURE);  // 실패 종료를 나타내는 값
```

보통 `0`은 성공을 나타내지만, 의미가 분명하도록 `EXIT_SUCCESS`와 `EXIT_FAILURE`를 사용하는 편이 좋다.

### `exit()`와 `return`의 차이

- `return`은 현재 실행 중인 **함수**를 끝내고 호출한 곳으로 돌아간다.
- `main()` 안의 `return 0;`은 `main()`을 끝내므로 프로그램이 정상 종료된다.
- `exit()`는 어느 함수에서 호출하든 **프로그램 전체**를 종료한다.
- 반복문만 빠져나오려면 `exit()`가 아니라 `break`를 사용한다.

일반적으로 `main()`의 끝에서 정상 종료할 때는 `return 0;`을 사용한다. 반복문 안에서 특정 조건에 따라 프로그램 전체를 끝내야 하는 경우에는 `exit()`를 사용할 수 있다. `exit()`는 정상 종료 절차를 수행하고 종료 상태를 운영체제에 전달한다. :chatgpt-content-reference{index="0"}

---

<br>

## 2. `system()` — 운영체제 명령 실행하기

`system()`은 문자열로 전달한 명령을 운영체제의 명령 처리기에 전달한다. 화면 속 `title`, `dir`, `cls`는 Windows 명령 프롬프트에서 사용하는 명령이다.

```c
#include <stdio.h>
#include <stdlib.h>  // system()

int main(void)
{
    system("title system() 함수 실습");  // 콘솔 창 제목 변경

    printf("현재 작업 디렉터리의 내용을 출력합니다.\n");
    system("dir");                       // 디렉터리 내용 출력

    printf("아무 키나 누르면 화면을 지웁니다.\n");
    system("pause");                     // Windows 명령으로 키 입력 대기

    system("cls");                       // 콘솔 화면 지우기

    return 0;
}
```

각 명령의 역할은 다음과 같다.

- `system("title ...")`: Windows 콘솔 창 제목을 바꾼다.
- `system("dir")`: 현재 작업 디렉터리의 파일과 폴더를 출력한다.
- `system("pause")`: Windows 명령 프롬프트에서 키 입력을 기다린다.
- `system("cls")`: Windows 콘솔 화면을 지운다.

`system()`은 C 코드가 직접 `dir`이나 `cls`의 기능을 수행하는 것이 아니다. 전달받은 문자열을 명령 처리기에 넘기고, 명령 처리기가 해당 명령을 실행한다. 그래서 명령 이름과 동작은 운영체제에 따라 달라질 수 있다. :chatgpt-content-reference{index="1"}

### `system()` 사용 시 주의점

`system()`으로 실행하는 명령은 운영체제와 명령 처리기에 따라 다르다. 예를 들어 `cls`는 Windows 명령이므로 다른 운영체제에서는 동작하지 않을 수 있다.

또한 사용자 입력을 그대로 명령 문자열에 붙여 실행하면 위험하다. 입력에 추가 명령이 포함되어 실행될 수 있기 때문이다. 실제 프로그램에서는 `system()` 사용을 꼭 필요한 경우로 제한하고, 외부에서 받은 문자열을 명령으로 실행하지 않도록 주의한다.

---

<br>

## 3. `_getch()` — 키 하나 입력받기

화면의 `system()` 예제에는 아무 키나 누르게 하는 부분도 나온다. 이때 사용한 `_getch()`는 키보드에서 문자 하나를 입력받는다. 입력한 문자를 화면에 표시하지 않고, 보통 Enter를 누르지 않아도 바로 입력을 받는다.

```c
#include <stdio.h>
#include <conio.h>  // _getch()

int main(void)
{
    printf("아무 키나 누르세요: ");
    _getch();

    printf("\n키 입력이 끝났습니다.\n");
    return 0;
}
```

`_getch()`는 C 표준 함수가 아니라 Microsoft의 C 런타임에서 제공하는 함수다. 따라서 컴파일러나 운영체제가 달라지면 사용할 수 없을 수 있다. Microsoft 문서에서는 `_getch()`를 `<conio.h>`에서 제공하는 함수로 설명한다. :chatgpt-content-reference{index="2"}

`_getch()`와 `system("pause")`는 학습용 콘솔 프로그램에서 입력을 잠시 기다릴 때 쓸 수 있지만, 동작 방식과 지원 환경이 서로 다르다. 둘 중 하나만 사용해도 된다.

---

<br>

## 4. `Sleep()` — 현재 스레드 잠시 멈추기

`Sleep()`은 Windows API 함수다. 괄호 안에 넣는 시간의 단위는 **밀리초(ms)**다.

```c
#include <stdio.h>
#include <windows.h>  // Sleep()

int main(void)
{
    printf("프로그램이 10초 뒤에 다음 작업을 합니다.\n");

    Sleep(10000);  // 10,000밀리초 = 10초

    printf("기다리기가 끝났습니다.\n");
    return 0;
}
```

시간 단위는 다음과 같다.

```text
1,000밀리초 = 1초
10,000밀리초 = 10초
```

`Sleep(10000)`은 프로그램 전체를 멈추는 명령이 아니라, 이 함수를 호출한 **현재 스레드**의 실행을 지정한 시간 동안 멈춘다. 시간이 지난 뒤에도 스레드가 즉시 실행된다고 보장되지는 않는다. 운영체제의 스케줄링 상황에 따라 실제로 다시 실행되는 시점은 조금 늦어질 수 있다. :chatgpt-content-reference{index="3"}

`Sleep()`은 Windows API이므로 Windows 전용이다. `#include <windows.h>`를 사용하며, 다른 운영체제에서는 같은 코드를 그대로 사용할 수 없다.

---

<br>

## `Sleep()` 예제 흐름

```c
#include <stdio.h>
#include <stdlib.h>
#include <windows.h>

int main(void)
{
    system("title Sleep() 함수 실습");

    printf("현재 작업 디렉터리 정보를 출력합니다.\n");
    system("dir");

    printf("프로그램은 10초 뒤에 다음 작업을 합니다.\n");
    Sleep(10000);  // 현재 스레드가 약 10초 동안 대기

    system("cls");  // 기다린 뒤 콘솔 화면 지우기

    return 0;
}
```

프로그램은 위에서 아래로 실행된다.

1. `system("title ...")`이 콘솔 제목을 바꾼다.
2. `system("dir")`이 디렉터리 내용을 출력한다.
3. `Sleep(10000)`에서 현재 스레드가 기다린다.
4. 기다림이 끝난 다음 `system("cls")`가 실행된다.
5. `return 0;`으로 `main()`이 끝나고 프로그램이 정상 종료된다.

---

<br>

## 헷갈린 점과 해결 과정

### `exit()`와 `break`는 같은가?

같지 않다. `break`는 가장 가까운 반복문이나 `switch` 문만 빠져나온다. `exit()`는 프로그램 자체를 종료한다.

### `return 0;`과 `exit(0);`은 같은가?

둘 다 `main()` 안에서 사용하면 프로그램이 종료되지만, 동작 범위가 다르다. `return 0;`은 `main()`을 끝내는 방식이고, `exit(0);`은 현재 위치와 관계없이 프로그램 전체를 종료하는 함수 호출이다.

### `Sleep(10000)`의 단위는 초인가?

초가 아니라 밀리초다. 따라서 `Sleep(10000)`은 10초를 기다리도록 요청한다.

### `Sleep()`이 CPU를 10초 동안 계속 바쁘게 만드는가?

`Sleep()`은 현재 스레드를 실행 가능한 상태에서 잠시 제외하도록 요청한다. 반복문으로 계속 시간을 확인하는 방식과 다르다. 다만 지정한 시간이 끝난 순간 정확히 실행된다는 뜻은 아니다.

### `system("dir")`은 모든 컴퓨터에서 동작하는가?

아니다. `system()`은 운영체제의 명령 처리기에 명령을 전달하므로, `dir`, `cls`, `pause` 같은 명령은 Windows 환경을 전제로 한다.

---

<br>

## 오늘 배운 내용 정리

- `exit()`는 프로그램 전체를 즉시 종료하고 종료 상태를 전달한다.
- `return`은 현재 함수에서 반환하며, `main()`의 `return 0;`은 프로그램을 정상 종료한다.
- `break`는 반복문이나 `switch`만 빠져나온다.
- `system()`은 명령 문자열을 운영체제의 명령 처리기에 전달한다.
- `dir`, `cls`, `pause`는 Windows 명령이므로 운영체제에 따라 사용할 수 없다.
- `_getch()`는 키 하나를 입력받는 Microsoft 전용 함수다.
- `Sleep()`은 Windows API이며, 시간 단위는 밀리초다.
- `Sleep()`은 호출한 현재 스레드를 기다리게 하며, 요청한 시간이 지나자마자 정확히 실행된다고 보장하지 않는다.
