---
title: <파이썬 알고리즘 인터뷰> - 3장. 파이썬 기초
categories:
  - Tech
  - 알고리즘
tags: [Python]
date: 2023-01-10 20:55:38
thumbnail: https://user-images.githubusercontent.com/1250095/86745916-a62e9a00-c075-11ea-9aa5-8455e2527f87.png
---

> 도서 파이썬 알고리즘 인터뷰의 내용을 정리했습니다. <br> <a href="https://github.com/onlybooks/algorithm-interview">깃허브 주소</a> > {% asset_img algorithm.png 763 512 '"전체 개념 요약" "전체 개념 요약"' %}

# 3장: 파이썬

## ✅ 파이썬 문법

---

### 1. 인덴트

- **공백 4칸 들여쓰기**: 파이썬 개선 제안서 8(PEP 8, Python Enhancement Proposals) 기준

### 2. 네이밍 컨벤션

- **변수, 함수명**: **스네이크 케이스**

  - 예시
    모듈 상수는 모두 대문자를 사용하고 단어마다 밑줄로 연결하는 ALL_CAPS 포맷으로 명명

    ```
    MAX_COUNT = 100
    ```

    클래스의 public attribute는 밑줄로 시작하지 말아야 함

    ```python
    class Student:
        schoolName = 'XYZ School' # class attribute

        def __init__(self, name, age):
            self.name=name # instance attribute
            self.age=age # instance attribute
    ```

    클래스의 protected instance attribute는 하나의 밑줄로 시작

    ```python
    class Modifiers:

        def __init__(self,name):
            self._protected_member = name # Protected Attribute
    ```

    클래스의 private instance attribute는 2개의 밑줄로 시작

    ```python
    class Modifiers:

        def __init__(self, name):
            self.__private_member = name  # Private Attribute
    ```

    인스턴스 메서드는 (객체 자신을 가리키기 위해) self 를 사용

    ```python
    class DecoratorExample:
      """ Example Class """
      def __init__(self):
        """ Example Setup """
        print('Hello, World!')
        self.name = 'Decorator_Example'
      def example_function(self):
        """ This method is an instance method! """
        print('I\'m an instance method!')
        print('My name is ' + self.name)
    ```

    클래스 메서드는 (클래스 자신을 가리키기 위해) cls 를 사용

    ```python
    class DecoratorExample:
      """ Example Class """
      def __init__(self):
        """ Example Setup """
        print('Hello, World!')
      @classmethod
      def example_function(cls):
        """ This method is a class method! """
        print('I\'m a class method!')
        cls.some_other_function()
      @staticmethod
        def some_other_function():
        print('Hello!')
    ```

- **클래스: 카멜 케이스**
- **상수: 대문자 + 밑줄**

### 3. 타입 힌트

- 명시적 선언을 통해 가독성 up, 버그 발생 확률 down

```python
a: str = "1"
b: int = 1
```

```python
def fn(a: int) -> bool:
    ...
```

- 강제 규약이 아니기에 동적 할당될 수 있으므로 주의 필요

```python
>>> a: str = 1
>>> type(a)
<**class 'int'**>
```

### 4. 리스트 컴프리헨션

- 한 줄로 간결하게 작성 가능하여, 가독성이 좋은 편
- 대체로 표현식은 두개를 넘지 않아야

```python
>>> [n * 2 for n in range(1, 10+1) if n % 2 == 1]
[2, 6, 10, 14, 18]
```

그냥 작성시 → 길고, 별도의 리스트 변수까지 필요

```python
>>> a = []
>>> for n in range (1, 10+1):
>>>     if n % 2 == 1:
>>>         a.append(n * 2)
```

리스트 외의 딕셔너리 등이 가능

```python
a = {}
for key, value in original.items():
    a[key] = value
```

다음과 같이 처리

```python
a = {key: value for key, value in original.items()}
```

### 5. 제너레이터

- 루프의 반복 동작을 제어할 수 있는 루틴 형태
- 임의의 조건으로 숫자 1억 개를 만들어내 계산하는 프로그램을 작성해야 할 때, 메모리 어딘가에 보관이 필요

```python
def get_natural_number():
		n = 0
    while True:
				n += 1
				yield # return이 아닌 yield를 사용하면 제너레이터가 반환된다.
```

```python
>>> g = get_natural_number()
>>> for _ in range(0, 100):
				print(next(g)) # next를 통해 다음 결과를 반환
```

```python
>>> def generator():
			yield 1
			yield 'string'
			yield True
>>> g = generator
>>> next(g)
1
>>> next(g)
'string'
>>> next(g)
True
```

### 6. Range

- 제너레이터 중 하나
- 제너레이터의 공간 효율 관점에의 장점을 알 수 있음.

```python
>>> a = [n for n in range(1000000)]
>>> b = range(1000000)
>>> len (a) == len(b)
True
>>> sys.getsizeof(a)
8697464
>>> sys.getsizeof(b)
48
```

```python
# 용법
range(A, B, C) # A부터 B-1까지, C만큼씩

# 역순
range(7, 0-1, -1) # 7부터 0까지 역순으로
```

### 7. enumerate

- 열거하다 라는 뜻의 함수
- 인덱스를 포함한 enumerate 객체 반환
- a = [’a1’, ‘a2’, ‘a3’]를 인덱스와 함께 출력해야 할 때

```python
# 방식 1 => 불필요한 a[i] 조회 작업, len 사용하는 루프처리 형태
for i in range(len(a)):
		print(i, a[i])

# 방식 2 => enumereate 활용
for i, v in enumerate(a):
		print(i, v)
```

### 8. 나눗셈 연산자

- **/ : 실수형 몫**
  - 5 / 3 = 1.666666667
- **// : 정수형 몫 (int(5 / 3))**
  - 5 // 3 = 1
- **%: 나머지 (modulo, 모듈로 연산)**
  - 5 % 3 = 2
- **divmod(): 몫과 나머지 한번에**
  - divmod(5, 3)
  - (1, 2)

### 9. print

- 디버깅 할 때 자주 쓰인다.

```python
>>> print('aa', end=' ')
>>> print('bb')
aa bb

>>> a = ['A', 'B']
>>> print(' '.join(a))
A B

>>> print(f'{idx + 1}: {fruit}')
2: Apple
```

### 10. pass

- 일단 코드의 전체 골격을 잡아 놓고 내부에서 처리할 내용을 차근차근 만들고 싶을때

```python
class MyClass(object):
		def method_a(self):
				pass
		def method_b(self):
				print("Method B")
c = MyClass()
```
