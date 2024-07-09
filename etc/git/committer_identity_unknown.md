[🏠 처음으로](/README.md)

# 환경
- Git, Fork

# 에러 발생 상황
- Fork를 통해 commit 하려고 할 때 에러 발생

# 에러 메시지

    $ git commit --커밋정보

    Committer identity unknown

    *** Please tell me who you are.

    Run

    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"

    to set your account's default identity.
    Omit --global to set the identity only in this repository.

    fatal: empty ident name (for <>) not allowed

- 메인 에러 메시지: `Committer identity unknown`


# 해결 과정
1. 메시지를 잘 읽어본다
2. 시키는 대로 해본다
    - terminal에 아래 코드에 알맞게 email과 name을 설정

            git config --global user.email "you@example.com"
            git config --global user.name "Your Name"

3. 그래도 fork에서는 되지 않음
4. 설정 확인해보기
    1. 설정 - Git - Global User Information
    2. 해당 부분이 공백
    3. 알맞게 이름과 이메일 입력
5. 해결