[🏠 처음으로](/README.md)

# unpacking sequence 시퀀스 언패킹과 Starred Expression 별표연산자

- write: 2024-07-20
- a, b = [1, 2, 3]이 어떻게 가능할까?
- `*` 별표 연산자를 사용한 표현식

### TOC

# Unpacking Sequence

> 언패킹, 시퀀스의 값을 여러 변수에 저장할 수 있다  

- 예를 들면, 리스트의 여러 값을 각각의 변수에 한 줄의 코드로 할당하는 것
    - words = ["left", "right"]  
    - left_number, right_number = [3, 6]


### 변수의 개수

- 변수의 개수는 시퀀스의 element 개수와 같아야한다

        a, b = [1, 2, 3]
        print(a, b)
        # ValueError: too many values to unpack (expected 2)

        a, b, c = [1, 2]
        print(a, b, c)
        # ValueError: not enough values to unpack (expected 3, got 2)

        a, b, c, d = [1, 2, 3, 4]
        print(a, b)
        # 1 2
        print(c, d)
        # 3 4
        print(a, b, c, d)
        # 1 2 3 4


### 튜플, 문자열 등의 언패킹

- 튜플도 리스트처럼 언패킹이 가능하다

        tuple_variable = ("tuple1", "tuple2")
        tuple1, tuple2 = tuple_variable
        print(tuple1, tuple2)
        # tuple1 tuple2

- 문자열의 경우 문자 character 하나씩 언패킹된다

        string = "string"
        s, t, r, i, n, g = string
        print(s, t, r, i, n, g)
        # s t r i n g

- dictionary의 경우, key가 할당된다

        dictionay = {"key1": "value1", "key2": "value2"}
        first, second = dictionay
        print(first, second)
        # key1 key2


## Starred expression, *(별표) 연산자를 이용한 언패킹

    *a, b, c, d = [1, 2, 3, 4, 5]
    print(a, b, c, d)
    # [1, 2] 3 4 5

    a, *b, c, d = [1, 2, 3, 4, 5]
    print(a, b, c, d)
    # 1 [2, 3] 4 5

    *a, *b, c, d = [1, 2, 3, 4, 5] 
    # SyntaxError: multiple starred expressions in assignment

- 특정 부분의 모든 요소를 받는 표현식
- 별표 연산자는 하나만 사용할 수 있으며, 2개 이상을 사용하면 SyntaxError가 발생한다
- 함수에서 사용하는 `*args`, 가변인자도 비슷한 사용법 

### 중첩 시퀀스의 별표 연산자

    nested_list = [1, [2, 3, 4], 5]
    a, b, c = nested_list
    print(a, b, c)
    # 1 [2, 3, 4] 5

    a, *b, c = nested_list
    print(a, b, c)
    # 1 [[2, 3, 4]] 5

    a, *b, c = [1, 2, [3, 4, 5], 6, 7]
    print(a, b, c)
    #1 [2, [3, 4, 5], 6] 7


### 리스트와 딕셔너리 요소앞의 별표연산자
    list1 = [0, 1]
    list2 = [2, 3]
    merged_list = [list1, list2]
    print(merged_list)
    # [[0, 1], [2, 3]]

    merged_list = [*list1, *list2]
    print(merged_list)
    # [0, 1, 2, 3]

    dict1 = {"first1": 1, "first2": 2}
    dict2 = {"second1": 3, "second2": 4}

    # merged_dict = {dict1, dict2}  # TypeError: unhashable type: 'dict'

    merged_dict = {**dict1, **dict2}
    print(merged_dict)
    # {'first1': 1, 'first2': 2, 'second1': 3, 'second2': 4}


<br>

### 참고

- Rapid Coders. “Unpacking Sequences in Python.” [rapidcoders.in/article/unpacking-sequences-in-python?course_url=python-programming-for-aktu](https://rapidcoders.in/article/unpacking-sequences-in-python?course_url=python-programming-for-aktu). Accessed 20 July 2024. 
