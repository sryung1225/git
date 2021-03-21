# Git

## Git이란?
- 소스 관리를 위한 분산 버전 관리 시스템(형상관리 도구)
- 같은 파일을 여러 명이서 동시에 작업하는 등 병렬 개발이 가능해 생산성이 증가함
- 프로그램의 변동 과정을 체계적으로 관리할 수 있음
- 새로운 기능을 추가할 경우 branch로 충분한 실험을 해본 뒤 프로그램에 합쳐볼 수 있음(merge)
- 중앙 저장소가 폭파되어도 다시 원상복구할 수 있음

### 관련 주요 용어
- Repository (저장소)
    - 소스코드가 저장된 여러 개의 브랜치들이 모여 있는 물리적 공간
    - Local (로컬) / Remote (원격)
    - 원격 저장소에서 로컬 저장소로 소스코드를 Clone해서 사용하고 Push하면 원격에 반영됨
- Stage
    - 작업한 내용이 올라가는 임시 저장 영역
    - 이 영역을 이용해 작업한 내용 중 커밋에 반영할 파일만 선별하여 커밋을 수행함
- Branch
    - 커밋을 단위로 구분된 소스코드 타임라인에서 분기해서 새로운 커밋을 쌓을 수 있는 가지를 만드는 것, 또는 그 가지
    - 독립적인 작업 공간을 생성
    - 다른 브랜치의 영향을 받지 않기 때문에 여러 작업을 동시에 진행할 수 있음
    - 모든 브랜치는 master에서 분기되어 최종적으로 marster에 다시 병합(merge)함
- HEAD
    - 현재 위치를 가리키는 포인터
    - 현재 사용중인 브랜치의 선두 부분을 나타내는 이름
    - HEAD를 이동하면, 사용하는 브랜치가 변경됨


### 작동 원리
- Local
    - working directory (local) : 현재 프로젝트 폴더에 존재하는 파일들 그 자체
    - staging area (index) : 로컬에 변동 사항이 생겼을 경우, `git add` 명령어를 수행하면 해당 변동 사항을 인덱스 영역에 반영. commit이 이뤄질 준비가 된 파일의 내용들이 위치하는 영역
    - local repository : git이 버전 관리를 하기 위해 필요로 하는 데이터들을 저장하는 곳
- Remote
    - remote repository
- 작업한 내용을 스테이지에 올려서 로컬 저장소에 커밋하고, 이를 푸쉬해서 원격 저장소로 보낸다



## Git 기본 명령어

### git init
현재 디렉토리에 .git 폴더를 만들어 이제부터 버전관리를 할 수 있게끔 만듦
```
$ git init
```
이 때 master 브랜치가 기본으로 자동 생성됨 

### git remote
로컬 저장소와 연결된 원격 저장소를 확인
```
$ git remote
```
위에서 출력되는 메세지가 없다면 연결된 원격 저장소가 없음을 의미. `add`를 이용하여 새로 등록이 가능
```
$ git remote add {별칭} {원격저장소 주소}
```
별칭은 주로 origin을 사용함
```
$ git remote add origin https://....git
```
뒤에 `show`를 붙이면 원격 저장소의 세부정보를 볼 수 있음
```
$ git remote show <원격 저장소명>
```

### git status
현재 working directory의 상태를 확인. 변동사항이 있는 파일이 있다면 해당 파일들을 보여줌
```
$ git status
```

### git add
새롭게 추가된 파일이나 기존과 변동 사항이 있는 파일을 스테이징 함 (인덱스에 반영함)
```
$ git add <파일명>
```
<파일명> 대신에 `.`를 사용하면 변동 사항을 한 번에 모두 포함함
```
$ git add .
```
잘못된 파일을 add한 경우 이를 취소, 즉 stagining area에 머물고 있는 파일은 제거하고 working directory에는 유지시킴
```
$ git rm --cached <파일명>
```

### git commit
staging한 파일들을 확정 시키고 해당 작업이 어떤 작업인지 메세지를 남기면서 버전을 확정시킴
```
$ git commit -m "edit README.md"
```
만일 커밋 메세지를 잘못 작성했을 경우, `--amend` 옵션으로 다시 커밋하여 메세지를 수정할 수 있음
```
$ git commit --amend -m "수정된 메세지"
``` 

### git push
staging한 파일들을 원격 저장소에 업로드
```
$ git push <원격 저장소명 또는 별칭> <브랜치명>
```
git clone을 통해 저장소를 복제했다면 일반적으로 origin(별칭)
```
$ git push origin <브랜치 이름>
```
`git remote` 명령어를 이용해 정확한 저장소명을 알아낼 수 있음
```
$ git remote
> origin
```
`-u`를 push 뒤에 붙이면 최초에 한 번만 저장소명과 브랜치명을 입력하고 그 이후 생략할 수 있음
```
$ git push -u origin master
$ git push
```
`--delete`를 push 뒤에 붙이면 원격 저장소에 있는 브랜치를 삭제할 수 있음
```
$ git push --delete {원격 브랜치 이름}
```

### git branch
로컬 저장소의 branch의 목록을 보여줌
```
$ git branch
> * master
```
뒤에 `<브랜치이름>`을 붙일 경우, 로컬 저장소에 새로운 브랜치를 생성함
```
$ git branch main
$ git branch
> * master
> main
```
뒤에 `-r`을 붙일 경우, 원격 저장소의 브랜치 목록을 확인
```
$ git branch -r
> remote/origin/release
```
뒤에 `-a`를 붙일 경우, 로컬 저장소와 원격 저장소의 브랜치 목록을 모두 확인
```
$ git branch -a
> * master
> main
> remote/origin/release
```
뒤에 `-d`를 붙일 경우, 로컬 저장소의 브랜치를 삭제
```
$ git branch -d main
> Deleted branch main.
$ git branch
> * master
```
원격 저장소의 브랜치를 삭제하기 위해서는 추가적인 작업이 필요. 이유는 `git branch -r`로 보여주는 원격 저장소의 브랜치들은 참조내역이지 직접 영향을 주려면 다른 작업이 필요하기 때문.
```
$ git branch -r
> origin/release
$ git branch -d release
> Deleted branch release (wae ....).
$ git remote prune origin
```
뒤에 `-m`을 붙일 경우, 로컬 저장소의 브랜치의 이름을 변경 (해당 브랜치에 checkout한 경우라면 <새로운 브랜치이름>만 작성하여도 무방)
```
$ git branch -m <이전 브랜치이름> <새로운 브랜치이름>
```
원격 저장소에 이미 생성되어 있는 브랜치의 이름을 변경하기 위해서는 해당 브랜치에서 변경하고 싶은 이름의 브랜치를 새로 생성한 뒤 이전 브랜치를 삭제하는 방법을 사용
```
$ git checkout <이전 브랜치이름>
$ git branch -m <새로운 브랜치이름>
$ git push origin <새로운 브랜치이름>
$ git push origin --delete <이전 브랜치이름>
```


### git checkout
현재 branch에서 다른 branch로 이동함
```
$ git branch
> * master
> main
$ git checkout main
> Switched to branch `main`
$ git branch
> master
> * main
```
사이에 `-b`를 붙일 경우, 새로운 브랜치를 생성함과 동시에 이동함 (branch와 checkout 명령을 함께 함)
```
$ git checkout -b origin
$ git branch
> master
> main
> * origin
```

### git fetch
원격 저장소의 데이터를 로컬 저장소에 가져오기만 함
```
$ git fetch {원격 저장소 별칭}
```

### git merge
로컬 저장소에서 브랜치를 생성하여 작업한 후, 현재 브랜치에서 대상 브랜치를 병합 시킴. 즉, 대상 브랜치가 현재 브랜치에 포함되고 운영환경에 적응할 수 있는 상태가 됨
```
$ git merge <대상 브랜치 이름>
```
병합 과정에서 만일 기준 브랜치와 대상 브랜치가 같은 파일의 같은 부분을 수정할 경우 충돌이 발생함
```
$ git merge issue
> Auto-merging index.html
> CONFLICT (content) : Merge conflict in index.html
> Automatic merge failed; fix conflicts and then commit the result.
```
변경사항 충돌을 개발자가 해결하지 않는 이상 Merge 과정을 진행할 수 없음. 어떤 파일을 Merge 할 수 없었는지 확인은 `git status`를 이용
```
$ git status
> On branch issue
> You have unmerged paths.
    (fix conflicts and run "git commit")
> 
> Unmerged paths:
>    (use "git add <file>..." to mark resolution)
>
>        both modified:  index.html
>
> no changes added to commit (use "git add" and/or "git commit -a")
```
충돌이 일어난 파일은 unmerged 상태로 표시됨. 그리고 충돌이 일어난 파일을 열람하면 내용이 수정되어있음을 확인할 수 있으며 이를 개발자가 직접 수정해야 함
```
$ git add index.html
$ git commit -m "issue 브랜치 병합"
> On branch master
> nothing to commit (working directory clean)
```
실무에서 충돌을 예방하기 위해서는 master 변화를 지속적으로 내 브랜치로 가져와서 충돌나는 부분을 매번 제거하며 내 브랜치를 만드는 것을 추천


### git pull
원격 브랜치의 내용을 가져온 뒤(`git fetch`), 가져온 원격 브랜치의 내용을 현재 로컬 브랜치로 가져와 결합(`git merge`)함
```
$ git pull {원격 저장소 별칭} {브랜치명} 
```

### git stash
작업 중 잠시 멈추고 브랜치를 변경할 일이 생겼을 때 강제로 commit하지 않고 잠시 스택에 저장할 수 있도록 함
```
$ git stash
> Saved working directory and index state WIP on <브랜치명> : 
```
뒤에 `save`를 붙여도 동일. 특정이름을 지정할 수 있음
```
$ git stash save
$ git stash save <stash 이름>
```
stash가 제대로 됐는지 확인하는 방법으로 working directory가 잘 비워졌는지 확인하는 `git status`나 stash에 잘 들어갔는지 확인하는 뒤에 `list` 붙이기가 있음
```
$ git status
> On branch <브랜치명>
> No commits yet
> nothing to commit (create/copy file and use "git add" to track)
$ git stash list
> stach @{0}: WIP on <브랜치명> : ...
```
이후 넣어둔 작업을 다시 가져오기 위해 `apply`를 사용
```
$ git stash apply
```
사용한 stash가 필요가 없어질 경우 `drop`을 사용해 삭제
```
$ git stash drop stash@{0}
Dropped stash@{0} (.....)
```

### git log
저장소의 이력을 확인
```
$ git log
> commit ....
> Author : yourname <yourname@yourmail.com>
> ...
```

### git reset
commit 했던 내용이 잘못된 경우 과거의 commit으로 이동. 해당 commit 이후에 작성된 commit은 모두 삭제 됨
```
$ git reset [--옵션] {돌아갈 커밋의 해시주소}
```
`--hard` 옵션을 사용할 경우, 돌아간 commit 이후의 변경 이력을 전부 삭제함 (첫번째 커밋으로 돌아가면 두번째 커밋부터 전부 삭제)
```
$ git reset --hard
```
단, `--hard` 옵션으로 reset을 진행한 다음, push를 하려 하면 오류가 생기므로 강제로 덮어쓰는 push의 옵션 `-f` 나 `--force`를 주어야 함
```
$ git push -f origin master
```
`--mixed` 옵션을 사용할 경우(기본값), 변경 이력은 전부 삭제하지만 변경된 내용에 대해서는 남아있으며 unstage 상태이기에 커밋을 진행하기 전에 stage에 먼저 추가해야 함(`git add`)
```
$ git reset
$ git reset --mixed
```
`--soft` 옵션을 사용할 경우, 변경 이력은 전부 삭제하지만 변경된 내용에 대해서는 남아있으며 stage에 올라간 상태이기에 바로 커밋을 진행하면 됨
```
$ git reset --soft
```
commit을 상대적으로 지정할 수도 있음 (예시는 현재부터 6개 이력으로 돌아가기)
```
$ git reset HEAD~6
```
그리고 `git reset`은 다른 사람들과 브랜치를 공유할 경우에는 이력이 남기 때문에 주로 혼자만 사용하는 브랜치인 경우 사용. 이외의 경우에 `revert`를 이용

### git revert
reset과 같이 과거로 이동. 대신에 이 작업 역시 하나의 commit으로 간주하여 commit 히스토리에 추가함. 즉 `reset --soft`나 `reset --mixed`와 동일한 결과를 가지지만 이력은 다름
```
$ git revert {돌아갈 커밋}
```
만일 자동으로 커밋을 진행하고 싶지 않다면 뒤에 `--no-commit`을 붙임
```
$ git revert --no-comit {돌아갈 커밋}
```
되돌릴 커밋이 여러개일 경우 범위를 지정하여 진행 가능
```
$ git revert {돌아갈 커밋}..{돌아갈 커밋}
```

### git reflog
작업이 날라가는 경우 등에 대비해 저장소 tree에 일반적으로 보이지 않는 모든 commit들을 살펴볼 수 있음. reference logs
```
$ git reflog
```
유실된 commit을 찾았을 때 해당 커밋만 HEAD로 하는 tree에 가져오려면(복원하려면) `git reset`을 응용
```
$ git reset --hard {커밋ID}
```
현재 브랜치로 가져오려면 `git cherry-pick`을 응용
```
$ git cherry-pick {커밋ID}
```


## Branch 관리
- 주요 브랜치
    - master
        - 곧 배포할 코드를 보관
    - develop (dev)
        - 매일 빌드하는 개발중인 코드를 보관
        - 보관하는 코드를 배포할 준비가 완료되면 master에 merge
        - (이 과정을 새로운 릴리즈 버전 출시로 보면 됨)
- 보조 브랜치
    - feature
        - 곧 배포할(또는 언젠가) 기능을 개발
        - develop에서 파생되며, 기능 개발이 완료되는 시점에 develop에 merge
        - 일반적으로 로컬에서 생성하는 브랜치. 작업에 용이함
    - release
        - 배포를 준비. develop에서 작업하던 코드를 배포 하기에 앞서 버전 넘버 부여, 버그 수정, 검증 등의 활동을 함
        - devlop을 릴리즈 할 준비가 완료되었을 시점에 생성. develop에서 파생
        - 'release-버전넘버'로 작명 (ex.release-2.1)
        - 배포를 위한 검증이 모두 끝나고 준비가 완료되면 master에 merge, develop에도 merge
    - hotfix
        - 배포 이후 버그가 등장했을 때 수정하기 위함
        - 'hotfix-버전넘버'로 작명 (ex. release-2.1.1)
        - master에서 파생 (배포 이후의 수정이므로)
        - 버그 수정이 끝나면 master에 merge, develop애도 수정사항 merge


> git에 대한 내용은 여기서 참고하였습니다. - [git](https://git-scm.com/book/ko/v2) / [TUWLAB](https://www.tuwlab.com/ece/22202) / [IT엘도라도](https://it-eldorado.tistory.com/4) / [victolee](https://victorydntmd.tistory.com/73) / [backlog](https://backlog.com/git-tutorial/kr/intro/intro1_1.html)
> git shash는 여기서 참고하였습니다. - [NTS-WIT](https://wit.nts-corp.com/2014/03/25/1153) / [WikiDocs](https://wikidocs.net/17169)
> git revert/reset/reflog는 여기서 참고하였습니다 - [brownbear](https://brownbears.tistory.com/477) / [ha0kim](https://velog.io/@ha0kim/GIT-작업-되돌리는-명령어-Reset-Revert)
> branch 관리에 대한 내용은 여기서 참고하였습니다. - [Gunilog](http://amazingguni.github.io/blog/2016/03/git-branch-%EA%B7%9C%EC%B9%99)