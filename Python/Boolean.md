# TIL: 파이썬 Boolean 자료형

<br>

## 오늘 배운 내용

Boolean 자료형은 참인지 거짓인지 나타내며, `True`와 `False` 두 가지 값을 사용한다.

비교 연산의 결과도 Boolean 값으로 나온다.

```python
print(5 > 10)
print(5 < 10)
print(True)
print(False)
print(not True)
print(not False)
print(not (5 > 10))
```

실행 결과는 다음과 같다.

```text
False
True
True
False
False
True
True
```

`not`은 Boolean 값을 반대로 바꾸는 논리 연산자다. `True`에 사용하면 `False`가 되고, `False`에 사용하면 `True`가 된다.

<br>

## 헷갈렸던 점과 해결 과정

- **`not (5 > 10)`은 왜 `True`인가?** 먼저 `5 > 10`을 계산하면 `False`가 된다. 여기에 `not`을 적용하면 반대 값인 `True`가 된다.
- **`True`와 `False`를 소문자로 써도 되는가?** 파이썬에서는 첫 글자를 대문자로 써야 한다. `true`, `false`가 아니라 `True`, `False`라고 작성한다.

<br>

## 참고 자료

- 나도코딩 파이썬 강의의 `Boolean 자료형` 실습 화면

<br>

## 오늘 배운 내용 정리

Boolean 자료형은 참과 거짓을 나타내는 `True`, `False`를 사용한다. 비교 연산은 Boolean 값을 결과로 반환하며, `not` 연산자는 그 값을 반대로 바꾼다.
