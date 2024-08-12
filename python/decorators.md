[🏠 처음으로](/README.md)


# Decorator

> 다른 함수를 반환하는 함수

- 일반적으로 @wrapper 구문을 사용
- 예) classmethod(), staticmethod()
- 데코레이터 구문은 문법적으로 코드를 더 ***읽기 쉽고 간결하게*** 작성할 수 있도록 하는 기능
- 코드의 기본적인 동작이나 의미가 변경되지는 않음
- 아래 두 함수는 의미적으로 동일함

    ```python
    def f(arg):
        ...
    f = staticmethod(f)

    @staticmethod
    def f(arg):
        ...
    ```

<br>

## 데코레이터를 사용하지 않은 코드와 사용한 코드


### 데코레이터 사용하지 않음

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_hello():
    print("Hello!")

say_hello = my_decorator(say_hello)
say_hello()
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```

<br>

### 데코레이터 사용

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```







### 참고

- Muhammad Yasoob Ullah Khalid. “7. Decorators¶.” 7. Decorators - Python Tips 0.1 Documentation, [book.pythontips.com/en/latest/decorators.html](https://book.pythontips.com/en/latest/decorators.html). Accessed 10 Aug. 2024. 