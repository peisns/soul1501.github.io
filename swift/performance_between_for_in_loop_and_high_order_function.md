[🏠 처음으로](/README.md)

# for in 반복문과 고차함수(forEach, map, filter, reduce, compactMap)의 성능 비교

> 테스트 환경은 온라인 컴파일러, xcode(MacBook Pro 14 (M2 Pro)를 사용했으며, 실행 환경마다 값이 다를 수 있습니다.

### 결론

> 1. for in loop, for 반복문이 대부분의 경우에서 더 빠른 성능을 보여줌
> 2. map은 for 보다 빠르다!

### 벤치마크 코드

    import Foundation
    import CoreFoundation

    // 배열 생성
    let largeArray = Array(1...10_000_000)

    // 벤치마크 함수
    func benchmark(label: String, block: () -> Void) {
        let startTime = CFAbsoluteTimeGetCurrent()
        block()
        let timeElapsed = CFAbsoluteTimeGetCurrent() - startTime
        print("\(label): \(timeElapsed) seconds")
    }

<br>

## forEach 비교

- 테스트1

        // for 루프 성능 측정
        benchmark(label: "For loop") {
            var sum = 0
            for i in 0..<largeArray.count {
                sum += largeArray[i]
            }
            print("Sum (for loop): \(sum)")
        }

        // foreach 루프 성능 측정
        benchmark(label: "Foreach loop") {
            var sum = 0
            (0..<largeArray.count).forEach { sum += largeArray[$0] }
            print("Sum (foreach loop): \(sum)")
        }

- 결과1

        Sum (for loop): 50000005000000
        For loop: 0.04157698154449463 seconds
        Sum (foreach loop): 50000005000000
        Foreach loop: 4.035375952720642 seconds

    - 인덱스에 접근해서 계산하는 방식은 forEach가 너무 느림 ➞ 100배 차이 발생
    - forEach의 클로저에서 요소를 가져오기 때문에 호출과 관련된 오버헤드가 발생할 수 있다고 보임

- 테스트2

        // for 루프 성능 측정
        benchmark(label: "For loop") {
            var sum = 0
            for number in largeArray { sum += number }
            print("Sum (for loop): \(sum)")
        }

        // foreach 루프 성능 측정
        benchmark(label: "Foreach loop") {
            var sum = 0
            largeArray.forEach { sum += $0 }
            print("Sum (foreach loop): \(sum)")
        }

- 결과2

        Sum (for loop): 50000005000000
        For loop: 0.38792598247528076 seconds
        Sum (foreach loop): 50000005000000
        Foreach loop: 0.4245239496231079 seconds

    - 여전히 for in 반복문이 더 빠른 성능, 약 10% 정도 빠르다

## map 비교

- 테스트1

        // 변환 작업을 위한 함수 (예: 각 요소를 제곱)
        func transform(_ number: Int) -> Int {
            return number * number
        }

        // for 반복문 성능 측정
        benchmark(label: "For loop") {
            var squaredArray = [Int]()
            squaredArray.reserveCapacity(largeArray.count)
            for i in largeArray {
                squaredArray.append(transform(i))
            }
        }

        // map 고차함수 성능 측정
        benchmark(label: "Map function") {
            let _ = largeArray.map { transform($0) }
        }

- 결과1

        For loop: 0.5298689603805542 seconds
        Map function: 0.3687119483947754 seconds

- 테스트2

        // for 반복문 성능 측정
        benchmark(label: "For loop") {
            var newList = [String]()
            for element in largeArray {
                newList.append(String(element))
            }
        }

        // reduce 고차함수 성능 측정
        benchmark(label: "map function") {
            let _ = largeArray.map { String($0) }
        }

- 결과2

        For loop: 1.1192800998687744 seconds
        map function: 0.6375410556793213 seconds    

    - map이 더 빠르다!



## filter 비교

- 코드

        // for 반복문 성능 측정
        benchmark(label: "For loop") {
            var evenNumbers = [Int]()
            evenNumbers.reserveCapacity(largeArray.count / 2)
            for number in largeArray {
                if number.isMultiple(of: 2) {
                    evenNumbers.append(number)
                }
            }
        }

        // filter 고차함수 성능 측정
        benchmark(label: "Filter function") {
            let _ = largeArray.filter { $0.isMultiple(of:2) }
        }

- 결과

        For loop: 1.9820810556411743 seconds
        Filter function: 2.2588289976119995 seconds


## reduce 비교

- 코드1

        // for 반복문 성능 측정
        benchmark(label: "For loop") {
            var sum = 0
            for number in largeArray { sum += number }
            print("Sum (for loop): \(sum)")
        }

        // reduce 고차함수 성능 측정
        benchmark(label: "Reduce function") {
            let sum = largeArray.reduce(0, +)
            print("Sum (reduce): \(sum)")
        }

- 결과1

        Sum (for loop): 50000005000000
        For loop: 0.3916400671005249 seconds
        Sum (reduce): 50000005000000
        Reduce function: 0.4641209840774536 seconds

- 코드2

        // reduce 고차함수 성능 측정
        benchmark(label: "Reduce function") {
            let sum = largeArray.reduce(0) { $0 + $1 }
            print("Sum (reduce): \(sum)")
        }

- 결과2

        Sum (for loop): 50000005000000
        For loop: 0.36506402492523193 seconds
        Sum (reduce): 50000005000000
        Reduce function: 0.4651780128479004 seconds

    - 인자를 사용한 경우, 클로저를 사용한 경우 모두 for in 반복문에 비해 느린 성능을 보여줌


## compactMap 비교

- 코드

        // 크기가 큰 배열 생성
        let largeArray = Array(repeating: "1", count: 10_000_000) + Array(repeating: "a", count: 10_000_000)

        // for 반복문 성능 측정
        benchmark(label: "For loop") {
            var newList = [Int]()
            for element in largeArray {
                if let element = Int(element) {
                    newList.append(element)
                }
            }
        }

        // reduce 고차함수 성능 측정
        benchmark(label: "Reduce function") {
            let _ = largeArray.compactMap { Int($0) }
        }

- 결과(다회 실행)

        For loop: 4.902529001235962 seconds
        compactMap function: 5.389550089836121 seconds

        For loop: 4.8605769872665405 seconds
        compactMap function: 5.896746039390564 seconds

        For loop: 5.175652980804443 seconds
        compactMap function: 5.459721088409424 seconds

    - 값이 비슷한 경우도 있지만 결국 for in 반복문이 더 빠른 성능을 보임