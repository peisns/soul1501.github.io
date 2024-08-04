[🏠 처음으로](/README.md)

# iOS, macOS 등 프로젝트의 기기 최소 버전 설정하기(Xcode)


## 설정하기 (Xcode 프로젝트)

1. PROJECT
    1. Inf
        1. Deployment Target 설정
1. TARGET
    1. General
        1. Minimum Deployments 설정
    1. Build Setting
        1. Deployment
            1. iOS Deployment Target 등 설정

- Deployment Target으로 버전을 설정하면, 해당 버전보다 낮은 버전을 사용하는 디바이스에서는 앱을 설치하거나 사용할 수 없게 된다


### 설정별로 무엇이 다를까?

- Project와 Target의 차이를 알아야하는데, 이 곳에서 다루지는 않는다(설정과 관련된 글이기 때문!)
- 모든 Target은 상위 프로젝트와 플랫폼 SDK로부터 설정을 상속받는다
    - 따라서 Deployment Target은 Project의 Deployment Target을 상속받음
- Target에서 다른 설정을 지정해서 Project로부터 받은 Setting을 Override 할 수 있다.


### Project와 Target

- Project (프로젝트)
    - 프로젝트는 Xcode에서 애플리케이션을 개발하기 위한 기본 단위
    - 소스 코드, 리소스 파일, 설정 정보 등을 포함
    - 하나의 프로젝트는 하나 이상의 타겟을 가질 수 있음
    - 프로젝트 설정
        - 공통적인 빌드 설정, 환경 설정, 의존성 등을 설정할 수 있음
- Target (타겟)
    - 타겟은 특정 빌드 결과물을 만들기 위한 설정 집합
    - 프로젝트 내에서 독립적으로 구성되어 각기 다른 애플리케이션,프레임워크, 라이브러리 등을 생성할 수 있음
    - 특정 제품(예: 앱, 테스트, 확장 프로그램 등)을 빌드하고 실행하는 방법을 정의함
    - 하나의 프로젝트 안에 여러 타겟을 두어 다양한 제품을 만들 수 있음
    - 빌드 설정, 코드 서명, 정보 plist, 소스 파일 포함/제외 등이 포함됨
- 차이
    - 프로젝트는 전체 개발 환경과 리소스를 관리하는 단위이며, 타겟은 특정 빌드 결과물을 생성하는 설정 집합
    - 프로젝트는 하나 이상 타겟을 포함할 수 있으며, 각 타겟은 별도의 애플리케이션이나 구성 요소를 만들기 위한 설정을 가짐
    - 프로젝트 수준에서는 공통 설정을 관리하고, 타겟 수준에서는 특정 제품을 빌드하기 위한 세부 설정을 관리



<br>

---

### 참고

- “Build system.” 
Apple Developer Documentation, [developer.apple.com/documentation/xcode/build-system](https://developer.apple.com/documentation/xcode/build-system). Accessed 4 Aug. 2024. 