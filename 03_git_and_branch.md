# 3장 : 깃과 브랜치

---

### 1. 브랜치(branch)
- 분기점
    ![이미지](/images/branch.PNG)
    - master에서 commit한다.
    - 현 상태에서 Feature-1 브랜치를 만들어 새로운 내용을 추가하여 commit한다.
    - master에서 새로운 내용을 넣어 commit한다.
    - 현 상태에서 Feature-2 브랜치를 만들어 새로운 내용을 추가하여 commit 한다.
    - Feature-1과 master를 merge(병합)한다.
    - Feature-2와 master를 merge(병합)한다.

### 2. 브랜치 만들기
- 실습 환경 설정하기
    ```
    $ mkdir branchtest
    $ cd branchtest
    $ git init
    $ vim work.txt
    $ git add work.txt
    $ git commit -m "work 1"
    ```
    - 새로운 저장소를 만든다.
    - 새로운 파일을 만들고 commit까지 실시한다.
- 새 브랜치 만들기
    - 브랜치 확인 :: `git branch`
    - 브랜치 생성 :: `git branch [브랜치 이름]`
    ```
    $ git branch
    $ git branch apple
    $ git branch ms
    $ git branch google
    ```
- 브랜치 이동하기 :: `git checkout [브랜치 이름]`
    - master에서 work.txt를 수정하고 커밋한다.
    - `git log --oneline`으로 로그를 보면 master와 다른 브랜치의 상태가 다른 것을 확인 할 수 있다.
    - apple 브랜치로 이동하여 `git log --oneline`으로 로그를 보면 HEAD가 apple를 가리키고 있는 것을 볼 수 있다.
    - work.txt를 cat으로 확인해보면 master에서 수정한 내용이 반영되지 않았음을 확인할 수 있다.

### 3. 브랜치 정보 확인하기
- apple에서 work.txt를 수정하고 커밋한다.
- `git log --oneline --branches --graph`를 한다면 분기되었음을 그래프로 확인할 수 있다.
- `git log master..apple` : master 브랜치를 기준으로 apple과 비교하여 차이점을 알 수 있다. 
- `git log apple..master` : apple 브랜치를 기준으로 master와 비교하여 차이점을 알 수 있다.

### 4. 브랜치 병합하기
- 서로 다른 파일 병합
    - 새로운 저장소를 만든다.
    - work.txt 파일을 만들어 '1'을 입력하고 저장
    - add 해주고 commit을 실행하는데 메시지를 "work 1"으로 한다.
    - b1 브랜치를 생성해준다.
    - master 브랜치에서 master.txt 파일을 만들어 'master 2'를 입력하고 저장
    - "master work 2"라는 메시지와 함께 커밋
    - b1 브랜치로 이동하여 b1.txt 파일을 만들어 'b1 2'를 입력하고 저장
    - "b1 work 2"라는 메시지와 함께 커밋
    - `git log --oneline --branches --graph`로 현재 상태를 확인한다.
    - master 브랜치로 이동하여 `git merge b1`을 입력하여 병합한다. :: master 브랜치를 b1과 병합한 결과를 master 브랜치에 적용한다.
    ```
    $ mkdir man1
    $ cd man1
    $ git init
    $ vim work.txt
    $ git add work.txt
    $ git commit -m "work 1"
    $ git branch b1
    $ vim master.txt
    $ git add master.txt
    $ git commit -m "master work 2"
    $ git checkout b1
    $ vim b1.txt
    $ git add b1.txt
    $ git commit -m "b1 work 2"
    $ git log --oneline --branches --graph
    $ git checkout master
    $ git merge b1
    $ ls -al
    ```
- 같은 파일 다른 위치 수정후 병합
    - 새로운 저장소를 만든다.
    - work.txt 파일을 만들어 아래와 같이 입력하고 저장
        ```
        #title
        content
        

        #title
        content
        ```
    - add 해주고 commit을 실행하는데 메시지를 "work 1"으로 한다.
    - b1 브랜치를 생성해준다.
    - master 브랜치에서 work.txt 파일을 아래와 같이 수정하여 "master work 2" 메시지로 커밋한다.
        ```
        #title
        master content 2
        

        #title
        content
        ```
    - b1 브랜치로 이동하여 work.txt 파일을 아래와 같이 수정하여 "b1 work 2" 메시지로 커밋한다.
        ```
        #title
        content
        

        #title
        b1 content 2
        ```
    - master 브랜치로 이동하여 merge를 실시한 뒤 work.txt를 cat으로 확인하면 자연스럽게 합쳐진 모습을 볼 수 있다.
    ```
    $ git init man2
    $ cd man2
    $ vim work.txt
    $ git add work.txt
    $ git commit -m "work 1"
    $ git branch b1
    $ vim work.txt
    $ git commit -am "master work 2"
    $ git checkout b1
    $ vim work.txt
    $ git commit -am "b1 work 2"
    $ git checkout master
    $ git merge b1
    $ cat work.txt
    ```
- 같은 파일 같은 위치 수정후 병합
    - ***깃에서는 줄 단위로 변경여부를 확인한다.***
    - ***같은 줄을 수정했을 때 브랜치를 병합하면 브랜치 충돌이 발생***
    - 새로운 저장소를 만든다.
    - work.txt 파일을 만들어 아래와 같이 입력하고 저장
        ```
        #title
        content

        #title
        content
        ```
    - add 해주고 commit을 실행하는데 메시지를 "work 1"으로 한다.
    - b1 브랜치를 생성해준다.
    - master 브랜치에서 work.txt 파일을 아래와 같이 수정하여 "master work 2" 메시지로 커밋한다.
        ```
        #title
        content
        master content 2
        #title
        content
        ```
    - b1 브랜치로 이동하여 work.txt 파일을 아래와 같이 수정하여 "b1 work 2" 메시지로 커밋한다.
        ```
        #title
        content
        b1 content 2
        #title
        content
        ```
    - master 브랜치로 이동하여 merge를 실시하면 CONFLICT 메시지가 출려된다.
    - 편집기로 work.txt를 열어서 필요없는 내용 `<<<<HEAD`, `>>>>b1`, `=====`을 삭제한 뒤 저장하고 편집기를 종료한다.
    - 수정한 work.txt를 스테이지에 올리고 커밋하면 충돌을 해결하고 커밋한 것이다.
    - `git log --oneline --branches --graph`로 지금까지 만든 브랜치와 커밋의 관계를 한눈에 확인할 수 있다.
    ```
    $ git init man2
    $ cd man2
    $ vim work.txt
    $ git add work.txt
    $ git commit -m "work 1"
    $ git branch b1
    $ vim work.txt
    $ git commit -am "master work 2"
    $ git checkout b1
    $ vim work.txt
    $ git commit -am "b1 work 2"
    $ git checkout master
    $ git merge b1
    $ vim work.txt
    $ git commit -am "merge b1 branch"
    $ git log --oneline --branches --graph
    ```
- 브랜치를 병합할 때 편집기
    - 편집기 열리게 하기 : `git merge b1 --edit`
    - 편집기 열리지 않게 하기 : `git merge b1 --no-edit`
- 병합 및 충돌 해결 프로그램

|프로그램 이름|설명|
|:------------|:----|
|P4Merge|무료이고 직관적이며 사용이 편리하고 병합 기능이 뛰어남. 단축키가 지원되지 않는 단점이 있다.|
|Meld|무료이며 오픈소스. 파일을 비교하는 것뿐만 아니라 직접 편집할 수 있다.|
|Kdiff3|무료이고 사용이 편리하고 병합 기능이 뛰어나지만 한글이 깨져보일 수 있다.|
|Araxis Merge|유료지만 용량이 큰 파일에서도 잘 동작함|

- 병합이 끝난 브랜치 삭제
    - `git branch`로 남은 브랜치 확인
    - 저장소의 기본 브랜치는 master 브랜치이므로 master 브랜치로 이동하여 브랜치를 삭제하는 것이 안전하다.
    - b1 브랜치의 삭제
        - `git branch -d b1`
        - `git branch --delete b1`
        - `git branch -D b1`

### 5. 브랜치 관리하기
- 커밋 해시를 통해서 브랜치들을 관리하고 커밋 해시(버전)으로 rollback 할 수 있다.
- 수정 중인 파일 감추기 및 되돌리기 :: `git stash`
    - 아직 커밋하지 않고 작업 중인 파일들을 잠시 감춰둘 수 있다.
    - stash 명령을 사용하기 위해선 파일이 tracked 상태여야 한다.(한 번은 커밋한 상태여야 한다)
    - 새로운 저장소를 만들고 f1.txt, f2.txt를 생성하고 커밋한다.
    - f1.txt와 f2.txt를 수정하고 저장만 한 뒤 `git status`로 상태를 확인한다.
    - 각각의 파일이 modified 상태인 것을 확인 할 수있다.
    - `git stash`를 실시하고 다시 `git status`를 해보면 modified 메시지가 사라진 것을 확인 할 수 있다.
    - 같은 방법으로 여러 파일을 수정한 후 따로 보관할 수 있다.
    - stash가 보관되는 공간은 stack으로 이루어져 LIFO이 적용된다.
        - 가장 최근에 보관한 stash는 stash@{0}에 담기게 된다.
    - 감추어둔 파일을 꺼내와 계속 수정하거나 커밋할 수 있다. 이때 가장 최근 항목을 되돌리기 위해서 `git stash pop`을 사용한다.
    ```
    $ git init st
    $ cd st
    $ vim f1.txt
    $ vim f2.txt
    $ git add .
    $ git commit -m "add f1, f2"
    $ vim f1.txt
    $ vim f2.txt
    $ git stash
    $ git stash pop
    ```
- stash 관련 명령어
    - `git stash` : stash 스택에 modified 상태의 작업물을 담는다.
    - `git stash pop` : 가장 최근의 항목을 되돌리고 stash 스택에서 제거한다.
    - `git stash list` : stash 스택의 목록을 확인한다.
    - `git stash apply` : 가장 최근의 항목을 되돌리고 stash 스택에 남겨둔다.
    - `git stash drop` : 가장 최근의 항목을 삭제한다.