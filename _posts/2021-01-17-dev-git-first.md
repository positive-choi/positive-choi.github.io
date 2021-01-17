---
layout: post
title:  "[Git] 기초"
subtitle:   "Git"
categories: dev
tags: git
comments: true
---
## Git

> 처음부터 `Git` 시작하기.

---

* Terminal로 접근해서 사용하기(Mac:iTerm2, Window:cmder 툴 사용 추천)
* GUI Tool로 사용한다면 Source Tree를 사용면된다.

### Git 버전 확인하기

* Cmd 창에서 git --version으로 설치 되어있는지 확인.

* `git config --list`로 설정 확인하기

  ```bash
  mac@MacBookPro 2021git % git config --list
  credential.helper=osxkeychain
  user.name=아이디
  user.email=메일
  core.excludesfile=/Users/mac/.gitignore_global
  difftool.sourcetree.cmd=opendiff "$LOCAL" "$REMOTE"
  difftool.sourcetree.path=
  mergetool.sourcetree.cmd=/Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh "$LOCAL" "$REMOTE" -ancestor "$BASE" -merge "$MERGED"
  mergetool.sourcetree.trustexitcode=true
  commit.template=/Users/mac/.stCommitMsg
  core.repositoryformatversion=0
  core.filemode=true
  core.bare=false
  core.logallrefupdates=true
  core.ignorecase=true
  core.precomposeunicode=true
  ```

* `git config --global -e` 파일로 열어 수정하기

* `git config user.name "이름"`,`git config user.email "이메일"`로 전역으로 계정을 등록한다.

* 전역에 설정된 계정,메일 확인하기``git config user.name`,`git config user.email` 를 입력하면 나온다.

  ```bash
  mac@MacBookPro 2021git % git config user.name
  설정된 아이디
  mac@MacBookPro 2021git % git config user.email
  설정된 메일
  ```

* 줄바꿈 문자열 호환되게하기 **window** :`git config --global core.autocrlf true`, **Mac** : `git config --global core.autocrlf input`

---

## git cmd 보게 편하기 쓰기(Mac)

* iTerm2 설치

* zsh 설치  **명령어** : `brew install zsh`

* Oh my ZSH 설치 **명령어** :  `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

* **명령어** : `vi ~/.zshrc` 입력 후

  `변경전`

  ```bash
  # Set name of the theme to load --- if set to "random", it will
  # load a random theme each time oh-my-zsh is loaded, in which case,
  # to know which specific one was loaded, run: echo $RANDOM_THEME
  # See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
  ZSH_THEME="robbyrussell"
  ```

  `변경후`

  ```bash
  # Set name of the theme to load --- if set to "random", it will
  # load a random theme each time oh-my-zsh is loaded, in which case,
  # to know which specific one was loaded, run: echo $RANDOM_THEME
  # See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
  ZSH_THEME="agnoster"
  ```

* 명령어를 통해 소스 적용 **명령어** : `source ~/.zshrc`

* 글자깨짐 처리 폰트 설치 [다운로드 링크](https://github.com/naver/d2codingfont) 다운받아 설치한다.

* iTerm 설정에 들어가 `Profiles` > `Text` 들어가 **font** 변경

* `Syntax Hightlight`*명령어 체크* 적용하기 **명령어** : `brew install zsh-syntax-highlighting`,

  **소스 적용 명령어** :`source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh`

   

---

## Git Workflow

| working directory                               | staging area                                                 | .git directory                                               | Remote      |
| ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------- |
| 작업하고 있는 장소(개발하는 장소)               | 파일을 옮겨놓는 개념(저장소에 저장하기전 저장하는 장소)      | commit한 파일을 버전(스냅샷) 별로 저장하는장소(개발완료된 코드를 저장하는 장소) | 서버 저장소 |
| **명령어**:`add`를 통해  staging area로 옮긴다. | **명령어** : `commit`을 통해 .git directory으로 옮긴다.      | **명령어** : `push`를 통해 Remote서버에 저장한다.<br />**명령어** : pull을 통해 Remote 서버에서 가져온다. |             |
|                                                 | **명령어** : `git rm --cached`로 staging area 에서 제거할 수 있다. |                                                              |             |

### working directory에서 특정파일 staging area에 파일 올리지 않기

* **.gitignore**를 생성해 그안에 파일 명을 기록한다.(아래 처럼 생성할 경우 .log파일로 끝나는 파일은 제외된다.)

  ```bash
  echo *.log > .gitignore
  ```

  

----

## git 명령어

### [git 명령어확인(git 공식페이지)](https://git-scm.com/docs)

* **git global -h** : git 명령어 보기(명령어 뒤에 -h를 붙이면 사용할 수 있는게 나온다.)

* **git status** : git 프로젝트 변경상황보기

  1. `git status -s`:로 간략하게 볼 수 있다.

* **git diff** : 파일 내부의 변경상태 확인

  1. `git diif` working directory와 staging area의 차이점을 보여준다.

     a/a.txt 는 staging area이고 b/a.txt는 working directory 이다. 아래결과를 보면 World!가 추가 된걸 확인할 수 있다.

     ```bash
     diff --git a/a.txt b/a.txt
     index ce01362..b4d60f6 100644
     --- a/a.txt
     +++ b/a.txt
     @@ -1 +1,2 @@
      hello
     +World!
     \ No newline at end of file
     (END)
     ```

  2. `git diff --staged` staging area영역의 과거 버전과 비교할 수 있다.(전버전은 없었기에 /dev/null로 표현된다.)

     ```bash
     diff --git a/a.txt b/a.txt
     new file mode 100644
     index 0000000..ce01362
     --- /dev/null
     +++ b/a.txt
     @@ -0,0 +1 @@
     +hello
     diff --git a/b.txt b/b.txt
     new file mode 100644
     index 0000000..ce01362
     --- /dev/null
     +++ b/b.txt
     @@ -0,0 +1 @@
     +hello
     diff --git a/c.txt b/c.txt
     new file mode 100644
     index 0000000..ce01362
     ```

* **git add** : working directory 에서 staging area으로 보낸다.

* **git commit** : staging area에서 .git directory로 보낸다.

  1. `git commit -m "커밋메시지"`를 입력하면 커밋에 메시지를 남긴다.

     ```bash
     git status -s
     A  1.txt
     M  a.txt
     git commit -m "add후 commit 테스트"
     [master 96b7ba6] add후 commit 테스트
      2 files changed, 2 insertions(+)
      create mode 100644 1.txt
     ```

  2. `git commit -am "커밋메시지"` staging area에 있는파일을 변경 후  working directory에서  staging area를 거치지 않고 .git directory로 바로 보낸다.

     ```bash
     git status -s
      M 1.txt
     git commit -am "add없이 commit 테스트"
     [master e1b1d82] add없이 commit 테스트
      1 file changed, 1 insertion(+)
     ```

  3. Git commit 은 디테일하게 나누어서 하는게 좋다. 버전별로 저장 개념

* **git log** : commit된 로그들을 확인 할 수있다.

  ```bash
  commit e1b1d8230b33473b2608c58ea0a9f948b878cf57 (HEAD -> master)
  Author: positive-choi <choi901015@gmail.com>
  Date:   Wed Jan 13 23:00:29 2021 +0900
  
      add없이 commit 테스트
  
  commit 96b7ba6f4ebc00c96869b32ee9464313b2be74c6
  Author: positive-choi <choi901015@gmail.com>
  Date:   Wed Jan 13 22:56:04 2021 +0900
  
      add후 commit 테스트
  
  commit e4912d18814a8eb7fc9ae022f650a067ebe9b8b5
  Author: positive-choi <choi901015@gmail.com>
  Date:   Wed Jan 13 22:46:08 2021 +0900
  
      test
  ```

  


