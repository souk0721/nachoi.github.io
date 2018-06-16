---
layout: post
title:  "[GitHub] 깃허브 브랜치 기초 사용"
author: Yena Choi
categories: studynote
tags: [github, branch]
comments: true
---

깃허브에 연습용 프로젝트를 올리면서 드디어 `git branch`의 필요성을 느꼈다. 이전에는 무조건 master 브랜치로만 작업하고, 틀렸을땐 강제로 `reset`과 `--force`로 돌려놓는 식으로 일했었다. 그러지 않기 위해서, 일단 깊은 이해보다는 빠르게 적용하기 위한 최소한의 내용만 기재했다.

## git branch
처음에 기본 브랜치는 자동으로 master로 생성, 설정된다. 이 브랜치 하나로만 작업을 하면 이전 버전으로 돌아가거나 다시 최신 버전으로 돌아가는 일이 번거로울 것이다. 여러 개의 버전을 편리하게 컨트롤하기 위해서, 혹은 실패 가능성이 있는 테스트를 하기 위해서 branch를 사용한다.

우선은 아주 기초적인 branch 명령어만 정리했다.

- `git help branch` : git branch 관련된 도움말이 표시된다.
- `git branch` : git branch만 입력하면 현재 repository의 branch를 보여준다. 현재 branch 앞에는 별(\*)표시가 되어 있다.
- `git branch [<branchname>]` : git branch 뒤에 이름을 붙이면, 해당 이름의 branch가 생성된다.
- `git branch -m [<oldbranch>] <newbranch>` : branch를 이동하거나 이름을 변경헌다.
- `git branch -d [<branchname>]` : 해당 branch를 삭제한다.

~~사실, 맨 처음 branch를 사용할 때 `git branch help` 라고 쳤다가 'help'라는 이름의 branch가 생성되고, 또 지우느라 애를 먹었다.~~
<br><br>

## git checkout
branch 간의 이동은 `git checkout` 명령어를 통해서 할 수 있다.

- `git checkout <branch>` : 해당 branch로 이동한다.
- `git checkout -b <new_branch>` : 새로운 브랜치를 생성하고, 바로 그 브랜치로 이동한다.
<br><br>

## git merge
test branch에서 commit 한 코드는 matser branch로 돌아와서 합칠 수 있다.

- `git merge <branch>` : 합칠 브랜치에서 'git merge <합쳐질 브랜치>' 형식으로 사용한다.

### git merge 실패
두 브랜치에서 같은 파일을 동시에 수정하고 merge하려고 할 경우, merge에 실패할 수 있다. 실패하게 되면 Git은 충돌(Conflict) 메세지를 출력한다.

이때 어떤 파일에서 오류가 났는지 `git status` 명령어로 확인해볼 수 있다. 해당 파일을 수정한 후에 다시 merge를 진행한다.
<br><br>

### References
- https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge%EC%9D%98-%EA%B8%B0%EC%B4%88
