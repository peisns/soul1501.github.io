[🏠 처음으로](/README.md)

# python's Errors and Exceptions


## Syntax Error

- 기본적인 문법 오류, 구문 오류입니다.

        print(if)


## Exceptions

- 문장이나 문법이 올바르더라도 예외가 발생할 수 있습니다.
- 정의, 선언되지 않은 변수에 접근 ➞ NameError
    ```python
    print(main)

    Traceback (most recent call last):
        File "<main.py>", line 1, in <module>
    NameError: name 'main' is not defined

    - 0으로 나누기 ➞ ZeroDivisionError

    a = 5 / 0

    Traceback (most recent call last):
        File "<main.py>", line 1, in <module>
    ZeroDivisionError: division by zero

    - 서로 다른 타입을 연결하기 ➞ TypeError

    int("1" + 0)

    Traceback (most recent call last):
        File "<main.py>", line 1, in <module>
    TypeError: can only concatenate str (not "int") to str
    ```

## Handling Exceptions

- 아래 코드에서는 사용자가 숫자가 아닌 string을 입력하거나 값을 입력하지 않고 진행하면 `int(입력값)` 에서 에러가 발생하고, ValueError 에러가 발생하면 print()를 실행시킵니다.
    ```python
    while True:
    try:
        x = int(input("Please enter a number: "))
        break
    except ValueError:
        print("Oops!  That was no valid number.  Try again...")

    # Please enter a number: 
    # Oops!  That was no valid number.  Try again...
    # Please enter a number: a
    # Oops!  That was no valid number.  Try again...
    # Please enter a number: 1
    ```

    - 아래 코드처럼 except 이후에 여러가지 예외를 함께 처리할 수 있습니다.
    ```python
    try:
        a = 1 + "1"
    except (RuntimeError, TypeError, NameError) as e:
        print(e)
    ```

- 예외가 발생했을 때, except 다음에 나오는 예외 클래스이거나 그 클래스를 상속 받는 클래스일 때 예외 처리가 됩니다.
- 아래 코드의 첫번째 반복문의 경우 B가 D, C에서는 예외처리가 되지 않지만, C와 D는 B를 상속받기 때문에 B에서 예외 처리가 됩니다.
    ```python
    class B(Exception):
        pass

    class C(B):
        pass

    class D(C):
        pass

    for cls in [B, C, D]:
        try:
            raise cls()
        except D:
            print("D")
        except C:
            print("C")
        except B:
            print("B")

    # B
    # C
    # D

    for cls in [B, C, D]:
        try:
            raise cls()
        except B:
            print("B")
        except C:
            print("C")
        except D:
            print("D")

    # B
    # B
    # B
    ```

- 에러가 전달될 때, 연관 값을 사용할 수 있습니다.
    ```python
    try:
        raise ValueError("값 오류가", "발생!")
    except ValueError as e:
        print(e.args)
        print(e.__str__())

    # ('값 오류가', '발생!')
    # ('값 오류가', '발생!')
    ```


## try except else와 raise, 그리고 finally

- else 키워드를 사용해서 예외가 발생하지 않았을 때 필요한 코드를 실행할 수 있도록 해줍니다.
    ```python
    try:
        a = 4 / 2
    except Exception as e:
        print(e)
    else:
        print("오류가 없군요")
        
    try:
        a = 4 / 0
    except Exception as e:
        print(e)
    else:
        print("오류가 발생하면 출력되지 않습니다")

    # 오류가 없군요
    # division by zero
    ```
- finally를 사용해서 오류가 발생해도 실행시킬 코드를 작성할 수 있습니다.
    ```python
    try:
        a = 4 / 0
    except Exception as e:
        print(e)
    else:
        print("오류가 발생하면 출력되지 않습니다")
    finally:
        print("무조건 출력됩니다")

    # division by zero
    # 무조건 출력됩니다
    ```
- raise로 강제로 예외를 발생시킬 수 있습니다.
    ```python
    try:
        a = 4 / 2
        raise ValueError("문제없지만 그냥 에러 발생시킵니다")
    except Exception as e:
        print("문제발생")
        raise

    # 문제발생
    # ERROR!
    # Traceback (most recent call last):
    #   File "<main.py>", line 3, in <module>
    # ValueError: 문제없지만 그냥 에러 발생시킵니다
    ```


## Exception chainging

- 예외 처리 내에서 예외가 발생한 경우 연결되서 표시됩닌다.
    ```python
    # try:
    #     a = 4 / 0
    # except Exception:
    #     raise 1 + "1"
        
    try:
        raise ValueError
    except Exception:
        raise NameError
        
    # Traceback (most recent call last):
    #   File "<main.py>", line 7, in <module>
    # ValueError

    # During handling of the above exception, another exception occurred:

    # Traceback (most recent call last):
    #   File "<main.py>", line 9, in <module>
    # NameError
    ```
- from을 사용해서 다른 예외의 직접적인 결과를 나타낼 수도 있고, from None으로 체이닝을 비활성화할 수도 있습니다.
    - from 다른에러
        ```python
        try:
            raise ValueError
        except Exception as e:
            raise NameError from e
            
        # Traceback (most recent call last):
        #   File "<main.py>", line 7, in <module>
        # ValueError

        # The above exception was the direct cause of the following exception:

        # Traceback (most recent call last):
        #   File "<main.py>", line 9, in <module>
        # NameError
        ```
    - from None
        ```python
        try:
            raise ValueError
        except Exception as e:
            raise NameError from None
            
        # Traceback (most recent call last):
        #   File "<main.py>", line 4, in <module>
        # NameError
        ```


## User-defined Exceptions, 사용자 정의 예외

- 새로운 예외 클래스를 생성할 수 있습니다.
- 빌트인 예외 처럼 Error로 끝나는 이름으로 정의합니다.
    ```python
    class InvalidURLError(Exception):
        def __init__(self, message="The provided URL is invalid"):
            super().__init__(message)


    class PageNotCrawlableError(Exception):
        def __init__(self, message="The page cannot be crawled"):
            super().__init__(message)
    ```