# learning_PlantUML

PlantUML을 익히는 repository 입니다.

# 공식 문서

- https://plantuml.com/ko/
- https://github.com/qjebbs/vscode-plantuml
- pdf: https://plantuml.com/ko/guide

# short cut

- PlantUML preview

  ```
    option + d
  ```

- preview md file
  ```
    shift + cmd + v
  ```

# 설치 방법

> 1->2 실패시 1->3번 과정 진행

1. vscode plantUML 설치

- https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml

2. plantUML에서 요구하는 dependency program 설치

- [plantUML 공식 github 기준](https://github.com/qjebbs/vscode-plantuml)으로는 아래 두개를 설치하면 된다. (23.03.03 기준)
- 그러나 temurin이 설치로 PlantUML Preview가 동작하지 않는다.
- 만약 설치가 되지 않는다면 3번까지 진행 필요

  ```jsx
  brew install --cask temurin
  brew install graphviz
  ```

3. jdk 설치

- 2번 과정으로 실패시 진행
- jdk11이 temurin으로 대체 됐으나 temurin으로 plantUML을 rendering할 수 없었다.
- 혹시 몰라 설치 해보니 … “no valid diagram found here plantuml” 이 문구가 나오면서 render 안되는 문제가 해결 됐다.

  ```jsx
  brew install --cask adoptopenjdk11

  ||

  brew install --cask adoptopenjdk
  ```
