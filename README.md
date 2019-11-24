# Git

## 저장소 만들기

`mkdir exercise`

`vim f1.txt` 파일작성

`cat f1.txt` 파일읽기

### 파일복사 `cp`

f1.txt 파일 f2.txt 로 복사

```
cp f1.txt f2.txt
```

### 폴더복사 `cp -r`

/dev/aaa 라는 폴더를 /var/www/html/aaa로 복사하기 위해서는?

```
cp -r /dev/aaa /var/www/html/aaa
```

## 버전 만들기

버전에 포함될 버전을 만든 사람에 대한 정보를 설정한다. 이 설정은 **~/.gitconfig** 파일에 저장되고 한번만 해주면 된다.

```
git config --global user.name  "자신의 닉네임"
git config --global user.email "자신의 이메일"
```

f1.txt 파일 수정하고 확인하기

```
vim f1.txt
git status
git add f1.txt
git commit
git log
```

```
git commit -a
git commit -am "First commit"
```

```
git status
```

### 버전 변경사항 확인하기

`-p` 를 추가하면, 모든 커밋과 커밋 사이의 변경사항에 대해서 확인할 수 있다.

```
git log -p
```

특정한 버전 간의 차이점을 비교할 때

```
git diff '버전 id'..'버전 id2'
git diff 38c9827e8f1e448506abfcf2b1c77ffd00dfd391..7d1bd71aa04221e98bd8b917c34ec88c42962bb6
```

`git diff` working directory와 staging area 의 파일을 비교한다.

`git diff --staged` staging area와 repository 에 있는 파일들을 비교해준다.

## 과거의 버전으로 돌아가기

### Reset

#### Reset --hard
  
대부분의 깃 명령어는 이전 커밋의 복원으로 되돌릴 수 있지만 *working directory* 와 *staging area* 에 속해있는 파일들은 커밋을 하지 않았기 때문에 다음 명령어를 사용하면 **되돌릴 수 없다**.

다음 아이디에 해당하는 커밋으로 돌아가고 이 이후의 모든 커밋은 삭제된다.

```
git reset 457f9e76737417dda36104452a434ff01a300673 --hard
```

^ 수 만큼 뒤로 돌아가고 이 이후의 모든 커밋은 삭제한다.

```
git reset --hard HEAD^
```

원격저장소 Origin 에 현재 HEAD 가 가리키고 있는 커밋보다 앞선 커밋이 있는 경우 오류가 발생한다. 이 경우 강제로 push 해야한다.

```
git push origin master --force
```

#### Reset --soft
  
커밋만 삭제하고 삭제된 변경사항들은 **Staging Area** 에 둔다. Unstaged 파일을 변경하고 있는 경우 Soft Reset 을 사용하면 된다.

#### Reset --mixed (DEFAULT)

커밋만 삭제하고 변경된 사항들은 **Unstaged** 상태로 둔다.
 
## Amending Commits

`--no-edit` 커밋 메세지는 수정하고 싶지 않을 때 추가하면 된다. 파일만 추가되고 커밋 메세지는 이전과 동일하다.

```
git commit --amend
git commit --amend --no-edit
```

## Staging Area 에서 제거

```
git rm <file_name>
git rm -r <folder_name>
```

Staging Area에 있던 images 폴더 제거 후 **캐시에서도 삭제 처리**

```
git rm -r images/ --cached
```

## Git branch 소개

### 브랜치 생성 및 사용

* `git branch` - 브랜치 전체보기
* `git branch exp` - exp 브랜치 생성
* `git checkout exp` - exp 브랜치 사용

### 브랜치 확인

* `git log --branches --decorate` - 모든 브랜치를 다 보여준다.
* `git log --branches --decorate --graph` - 그래프 추가

### 브랜치 차이

master 브랜치와 exp 브랜치의 차이를 보여준다. master 브랜치에는 없고 exp 브랜치에는 있는 것들을 보여준다.

```
git log master..exp
```

master에는 없고 exp에는 있는 것들을 코드와 함께 보여준다.

```
git log -p master..exp
```

master 브랜치와 exp 브랜치의 차이점을 볼 수 있다.
```
git diff master..exp
```

### 브랜치 병합

exp 브랜치 내용을 master 브랜치에 병합

```
git checkout master
git merge exp
```

exp가 master과 똑같은 커밋을 최신 커밋으로 갖고 있고 똑같은 부모를 가지고 있게 만든다. 즉, exp와 master가 똑같아지게 만들었다.

```
git checkout exp
git merge master
```

exp 브랜치 삭제
```
git branch -d exp
```

## Stash

Working directory의 변경사항을 감춘다. (버전관리가 되고 있는 파일만 감춘다.)

```
git stash
git stash apply

git reset --hard (git stash apply로 복원가능)
git stash list   (stash 기록)
git stash apply  (가장 최신 stash로 복원)

git stash apply drop (최신 stash 삭제)
git stash list       (기록에서 삭제여부 확인)

git stash apply; git stash drop;
git stash pop (위 코드 shorthand)
```

## 용어

- **stage**: 커밋 대기하고 있는 파일들이 가는 곳
- **repository**: 커밋이 된 파일이 저장되는 곳
- **ls -al**
- **HEAD**: HEAD는 현재 브랜치를 가리키는 포인터이며, 브랜치는 브랜치에 담긴 커밋 중 가장 마지막 커밋을 가리킨다. 지금의 HEAD가 가리키는 커밋은 바로 다음 커밋의 부모가 된다. 단순하게 생각하면 HEAD는 **현재 브랜치 마지막 커밋의 스냅샷**이다.
- **Index**: Index는 바로 다음에 커밋할 것들이다. 이미 앞에서 우리는 이런 개념을 “Staging Area” 라고 배운 바 있다. “Staging Area” 는 사용자가 `git commit` 명령을 실행했을 때 Git이 처리할 것들이 있는 곳이다.
- **M** stands for Modified.
- **U** stands for Untracked (아직은 Git이 이 파일을 등록되지 않았고, Git이 관찰하지 않고 있다.)
- `git commit -help` 어떤 옵션들이 있는지 보여준다.


## Areas

- **Working Directory / Working Area**: 현재 작업하고 있는 곳
- **Staging Area**: Which files are going to be committed.
- **Repository Area / Commit Area**: Git Version Control 에 추가된 Data files.

## Git Remote

연결되어 있는 원격 서버 목록을 보여준다

```
git remote -v
```

연결되어 있는 원격 서버 추가

```
git remote add <name> <원격서버주소>
```

연결되어 있는 원격 서버 삭제

```
git remote remove <name>
```

## Git Clone

Fork 하지 않은 Repository를 Clone 하면 파일을 내려받을 수는 있지만 작업한 내용을 업로드 할 수는 없다.

```
git clone <주소>
```

---

# Github

## Fork

해당 Repository 를 나의 Repository 로 복사하는 것이다.

## Pull Requests

Pull requests let you tell others about changes you've pushed to a branch in a repository on GitHub. [More info](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)
