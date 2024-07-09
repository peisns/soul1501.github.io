[🏠 처음으로](/README.md)

# python, 가상환경, 협업, 깃 조건에서 의존성 관리하기

- write: 2024-07-09
- study period: 2024-07-09
- python + 가상환경 + 다양한 라이브러리 + 협업 + 깃 조건에서 의존성 관리를 어떻게 해야할까?


<!-- ### TOC -->

# 환경

- VSCode
- venv
- scrapy
- git


# 5W1H?

1. 로컬에 venv로 가상환경을 설정한 후 scrapy 테스트를 위해 라이브러리 설치
2. 깃에 커밋하기 위해서 git init 한 후 바로 publish
3. 파일 수가 엄청나서 로컬에서 폴더 크기를 확인해보니 158MB, 5,732개의 파일을 push했다는 것을 알게 되었다
4. swift에서는 spm, pod 등으로 쉽게 의존성 관리를 할 수 있고 프로젝트를 GitHub 등으로 공유할 때 라이브러리를 쉽게 ignore하고 다시 설정할 수 있나 확인해보려 한다
5. venv docs에서 requirement.txt 파일을 만들어서 환경을 공유한다는 내용을 본 적이 있어서, 그렇게 환경을 공유하고 라이브러리는 gitignore 하는 방식인가 하고 같은 카테부 참여자에게 물어보니 해당 방식은 맞다고 했다
6. 추가로, AI 관련 간단한 코드들은 굳이 로컬에서 가상환경 만들고 돌려보기보단 colab이나 주피터를 통해서 실행한다는 이야기도 들었다

--- 

TODO: requirement 통해서 라이브러리 환경 기록하고, 깃록 하는 과정 정리하기
TODO: 풀스택 팀원들에게 Python 또는 JS Node 사용 시 의존성 관리하는 방법에 대해서 이야기 하기

---

# 깃허브의 파일들 제거하기(GitHub Docs)
- [링크로 대체](https://docs.github.com/ko/repositories/working-with-files/managing-large-files/about-large-files-on-github)
