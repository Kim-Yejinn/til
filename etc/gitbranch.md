### 1-git branch

- git 브랜치 전략 : Git 브랜치를 효과적으로 관리하기 위한 워크플로우  
  ex) Git Flow, Github Flow etc.
- Git Flow
  ![git-flow](/image/etc/git-flow.png) - Branch 구분 : Main, Develop, Supporting(Feature, Release, Hotfix) - Main, Develop 브랜치는 개발 프로세스 전반에 걸쳐 유지되는 브랜치 - Supporting 브랜치는 필요할때 생성되는 브랜치 - Main : 출시 가능한 프로덕션 코드 모음 - Develop : 다음 버전 개발을 위한 코드 모음 - Future : 하나의 기능을 개발하기 위한 브랜치 - Release : 소프트웨어 배포를 준비하기 위한 브랜치 - Hotfix : 이미 배포된 버전에 문제를 생겼을 경우 해결

- Github Flow  
  ![github-flow](/image/etc/github-flow.png)
  - Branch구분 : Main, Topic
    - Main : 언제 배포하든지 문제가 없어야 하며, 새로운 브랜치를 만들어도 문제가 없어야 함.
    - Topic : Feature 브랜치와 동일 역할, 기능이 완성하지 않아도 꾸준히 push 함으로서 구성원 모두가 끊임없이 커뮤니케이션 가능
- Git Flow와 Github Flow
  - Git Flow는 명시적으로 버전 관리가 필요한 프로젝트에 적합  
    ex) 스마트폰 어플리케이션, 오픈소스 라이브러리/프레임워크
  - Git Flow의 경우 Web 어플리케이션에는 적합하지 않음
  - Github Flow 는 소규모 애자일 팀, 제품이 단일 릴리즈 버전일 때 적절함.  
    ex) 웹 어플리케이션
