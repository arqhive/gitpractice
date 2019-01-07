# gitpractice
Git 자습 레포지터리
Linux Mint 운영체제에서 진행한다.
모든 명령은 CLI 환경에서 진행한다.

## 시작하기

#### Git 설정하기

'git config’라는 도구로 설정 내용을 확인하고 변경할 수 있다. Git은 이 설정에 따라 동작한다. 이때 사용하는 설정 파일은 세 가지나 된다.
1. /etc/gitconfig 파일: 시스템의 모든 사용자와 모든 저장소에 적용되는 설정이다. git config --system
옵션으로 이 파일을 읽고 쓸 수 있다.
2. ~/.gitconfig, ~/.config/git/config 파일: 특정 사용자에게만 적용되는 설정이다. git config
--global 옵션으로 이 파일을 읽고 쓸 수 있다.
3. .git/config : 이 파일은 Git 디렉토리에 있고 특정 저장소(혹은 현재 작업 중인 프로젝트)에만 적용된다.
각 설정은 역순으로 우선시 된다. 그래서 .git/config 가 /etc/gitconfig 보다 우선한다.

#### 사용자 정보

`$ git config --global user.name "Gyu Young Hwang"`

`$ git config --global user.email example@example.com`

--global 옵션으로 설정하는 경우 딱 한번만 하면 된다. 해당 시스템에서 해당 사용자가 사용할 때는
이 정보를 사용한다. 만약 프로젝트마다 다른 이름과 이메일 주소를 사용하고 싶으면 --global 옵션을 빼고 명령을 실행한다.


#### 설정 확인

`$ git config --list`

설정한 모든 것을 보여준다.

Git은 같은 키를 여러 파일(/etc/gitconfig와 ~/.gitconfig 같은) 에서 읽기 떄문에 같은 키가 여러 개 있을 수도 있다. 그러면 Git은 나중 값을 사용한다.

`$ git config <key>`

해당 명령으로 Git이 특정 Key에 대해 어떤 값을 사용하는지 확인할 수 있다.


#### 도움말 보기

    git help <verb>
    git <verb> --help
    man git-<verb>


## Git의 기초

### Git 저장소 만들기

#### 기존 프로젝트를 Git 저장소로 만들고 싶은경우

해당 디렉토리로 이동후 아래와 같은 명령을 실행한다.

`$ git init`

이 명령은 .git 이라는 하위 디렉토리를 만든다. .git 디렉토리에는 저장소에 필요한 뼈대 파일(Skeleton)이 들어 있다. 이 명령만으로는 아직 프로젝트의 어떤 파일도 관리하지 않는다.

#### 기존 저장소를 Clone 하기

다른 프로젝트에 참여하려거나 Git 저장소를 복사하고 싶을 때 git clone 명령을 사용한다.

git clone [url] 명령으로 저장소를 클론한다. 예를 들어 libgit2의 소스코드를 클론하려면

`$ git clone https://github.com/libgit2/libgit2`

**TIP!** [url]뒤에 파라미터를 주게 된다면 다른 디렉토리명으로 clone 할 수 있다.

#### 파일의 상태 확인하기

` $ git status`
해당 명령으로 파일의 상태를 확인 할 수 있다.

TEST라는 파일을 하나 생성해 본 뒤 git status 명령을 실행해 보자.

결과를 확인해 보면 TEST는 Untracked files 부분에 속해 있는데 TEST파일이 Untracked(추적되지 않는) 상태라는 것을 말한다. Git은 Untracked 파일을 아직 스냅샷(커밋)에 넣어지지 않은 파일이라고 본다. 파일이 Tracked 상태가 되기 전까지는 Git은 절대 그 파일을 커밋하지 않는다. 이제 해당 파일을 Tracked(추적되는) 상태로 만든다.

#### 파일을 새로 추적하기

git add 명령으로 파일을 새로 추적할 수 있다.

`$ git add TEST`

git status 명령을 다시 실행하면 TEST 파일이 Tracked 상태이면서 커밋에 추가될 Staged 상태라는 것을 확인할 수 있다.

"Changes to be committed"에 들어 있는 파일은 Staged 상태라는 것을 의미한다. 커밋하면 git add를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다.

#### Modified(수정된) 상태의 파일을 Stage 하기

Modified라는 이름의 파일을 하나 생성한다. 그 후 git add Modified 로 Modified를 Tracked 상태에 놓는다. 이제 Modified를 수정 한 뒤 git status를 실행해 보자.

이 Modified 파일은 'Changes not staged for commit'에 있다. 이것은 수정한 파일이 Tracked 상태이지만 아직 Staged 상태는 아니라는 것이다. Staged 상태로 만들려면 git add 명령을 실행해야 한다. git add 명령은 파일을 새로 추적할 때도 사용하고, 수정한 파일을 Staged 상태로 만들 때도 사용한다. Merge 할 때 충돌난 상태의 파일을 Resolve 상태로 만들때도 사용한다. add의 의미는 프로젝트에 파일을 추가한다기 보다는 다음 커밋에 추가한다고 받아들이는게 좋다. git add 명령으로 Modified 파일을 Staged 상태로 만들기 git status명령으로 결과를 확인해보자.

이후 다시 Modified를 수정하고 git status명령을 실행하게 되면 Modified가 Staged 상태이면 동시에 Unstaged 상태로 나온다. 왜 이런일이 발생할까? Git은 git add 명령을 실행한 시점을 Staged 상태로 만들며 이후 commit시 add 명령을 실행한 시점으로 commit하게 된다. 그러니까 add 명령을 실행한 후에 또 파일을 수정했다면 다시 add 명령으로 최신 버전을 Staged 상태로 만들어야 한다.

#### 파일 상태를 짤막하게 확인하기
git status 명령을 간단하게 보여주는 옵션이 있다.

`$ git status -s 혹은 git status --short`

다음과 같은 결과를 확인 할 수 있다. (예제)

    $ git status -s
    M README
    MM Rakefile
    A lib/git.rb
    M lib/simplegit.rb
    ?? LICENSE.txt

아직 추적하지 않는 새 파일 앞에는 ?? 표시가 붙는다. Staged 상태로 추가한 파일 중 새로 생성한 파일 앞에는 A 표시가, 수정한 파일 앞에는 M 표시가 붙는다. 위 명령의 결과에서 상태정보 컬럼에는 두 가지 정보를 보여준다. 왼쪽에는 Staging Area에서의 상태를, 오른쪽에는 Working Tree에서의 상태를 표시한다. README 파일 같은 경우 내용을 변경했지만 아직 Staged 상태로 추가하지는 않았다. lib/simplegit.rb 파일은 내용을 변경하고 Staged 상태로 추가까지 한 상태이다. 위 결과에서 차이점을 비교해보자. Rakefile 은 변경하고 Staged 상태로 추가한 후 또 내용을 변경해서 Staged 이면서 Unstaged 상태인 파일이다.

#### 파일 무시하기

어떤 파일은 Git이 관리할 필요가 없다. 보통 로그 파일이나 빌드 시스템이 자동으로 생성한 파일이 그렇다. 그런 파일을 무시하려면 .gitignore 파일을 만들고 그 안에 무시할 파일 패턴을 적는다.

.gitignore 파일에 입력하는 팬턴은 아래 규칙을 따른다.

• 아무것도 없는 라인이나, '#'로 시작하는 라인은 무시한다.
• 표준 Glob 패턴을 사용한다.
• 슬래시(/)로 시작하면 하위 디렉토리에 적용되지(Recursivity) 않는다.
• 디렉토리는 슬래시(/)를 끝에 사용하는 것으로 표현한다.
• 느낌표(!)로 시작하는 패턴의 파일은 무시하지 않는다.

Glob 패턴은 정규표현식을 단순하게 만든 것으로 생각하면 되고 보통 쉘에서 많이 사용한다. 애스터리스크(*)는 문자가 하나도 없거나 하나 이상을 의미하고, [abc] 는 중괄호 안에 있는 문자 중 하나를 의미한다(그러니까 이 경우에는 a, b, c). 물음표(?)는 문자 하나를 말하고, [0-9] 처럼 중괄호 안의 캐릭터 사이에 하이픈(-)을 사용하면 그 캐릭터 사이에 있는 문자 하나를 말한다. 애스터리스크 2개를 사용하여 디렉토리 안의 디렉토리 까지 지정할 수 있다. a/**/z 패턴은 a/z, a/b/z, a/b/c/z 디렉토리에 사용할 수 있다.

.gitignore 예제

    # 확장자가 .a인 파일 무시
    *.a

    # 윗 라인에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않음
    !lib.a

    # 현재 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않음
    /TODO

    # build/ 디렉토리에 있는 모든 파일은 무시
    build/

    # doc/notes.txt 파일은 무시하고 doc/server/arch.txt 파일은 무시하지 않음
    doc/*.txt

    # doc 디렉토리 아래의 모든 .pdf 파일을 무시
    doc/**/*.pdf

.gitignore 파일은 보통 처음에 만들어 두는 것이 편리하다. 그래서 Git 저장소에 커밋하고 싶지 않은 파일을 실수로 커밋하는 일을 방지할 수 있다.

**TIP!** GitHub은 다양한 프로젝트에서 자주 사용하는 .gitignore 예제를 관리하고 있다. 어떤 내용을 넣을지 막막하다면 https://github.com/github/gitignore 사이트에서 적당한 예제를 찾을 수 있다.

#### Staged와 Unstaged 상태의 변경 내용을 보기

Staged인지 Unstaged인지 만을 확인 하는게 아니라 어떤 내용이 변경됐는지 살펴보려면 git diff 명령을 사용해야 한다.

git diff 명령을 실행하면 수정했지만 아직 staged 상태가 아닌 파일을 비교해 볼 수 있다. 이 명령은 워킹 디렉토리에 잇는 것과 Staging Area에 있는 것을 비교한다. 그래서 수정하고 아직 Stage 하지 않은 것을 보여준다.

만약 커밋하려고 Staging Area에 넣은 파일의 변경 부분을 보고 싶으면 git diff --staged 옵션을 사용한다. 이 명령은 저장소에 커밋한 것과 Staging Area에 있는 것을 비교한다.

꼭 잊지 말아야 할 것이 있는데 git diff 명령은 마지막으로 커밋한 후에 수정한 것들 전부를 보여주지 않는다. git diff 는 Unstaged 상태인 것들만 보여준다. 이 부분이 조금 헷갈릴 수 있다. 수정한 파일을 모두 Staging Area에 넣었다면 git diff 명령은 아무것도 출력하지 않는다.

--staged와 --cached는 같은 옵션이다.

#### 변경 사항 커밋하기

수정한 것을 커밋하기 위해 Staging Area에 파일을 정리했다. 그 후에 git commit을 실행하여 커밋한다.

`$ git commit`

자동으로 생성되는 커밋 메시지의 첫 라인은 비어 있고 둘째 라인부터 git status 명령의 결과가 채워진다. 커밋한 내용을 쉽게 기억할 수 있도록 이 메시지를 포함할 수도 있고 메시지를 전부 지우고 새로 작성할 수 있다 (정확히 뭘 수정했는지도 보여줄 수 있는데, git commit 에 -v 옵션을 추가하면 편집기에 diff 메시지도 추가된다). 내용을 저장하고 편집기를 종료하면 Git은 입력된 내용(#로 시작하는 내용을 제외한)으로 새 커밋을 하나 완성한다.

-m 옵션으로 메세지를 인라인으로 첨부할 수도 있다.

`$ git commit -m "inline message"`

#### Staging Area 생략하기

`$ git commit -a`

-a 옵션을 추가하면 Git은 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다. 그래서 git add 명령을 실행하는 수고를 덜 수 있다.

#### 파일 삭제하기

Git에서 파일을 제거하려면 git rm명령으로 Tracked 상태의 파일을 삭제한 후에(정확하게는 Staging Area에서 삭제하는 것)커밋해야 한다. 이 명령은 워킹 디렉토리에 있는 파일도 삭제하기 때문에 실제로 파일도 지워진다.

Git 명령을 사용하지 않고 단순히 워킹 디렉터리에서 파일을 삭제하고 git status 명령으로 상태를 확인하면 Git은 현재 "Changes not staged for commit"(즉, Unstaged 상태)라고 표시해준다.

커밋하면 파일은 삭제되고 Git은 이 파일을 더는 추적하지 않는다. 이미 파일을 수정했거나 Index에(Staging Area을 Git Index라고도 부른다) 추가했다면 -f 옵션을 주어 강제로 삭제해야 한다. 이 점은 실수로 데이터를 삭제하지 못하도록 하는 안전장치다. 커밋 하지 않고 수정한 데이터는 Git으로 복구할 수 없기 때문이다.
또 Staging Area에서만 제거하고 워킹 디렉토리에 있는 파일은 지우지 않고 남겨둘 수 있다. 다시 말해서 하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않게 한다. 이것은 .gitignore 파일에 추가하는 것을 빼먹었거나 대용량 로그 파일이나 컴파일된 파일인 .a 파일 같은 것을 실수로 추가했을 때 쓴다. --cached 옵션을 사용하여 명령을 실행한다.

`$ git rm --cached README`

여러 개의 파일이나 디렉토리를 삭제하는 경우 file-glob 패턴을 사용한다.

`$ git rm log/\*.log`

#### 파일 이름 변경하기

`$ git mv file_from file_to`

#### 커밋 히스토리 조회하기

`$ git log`

옵션
-p: 각 커밋의 diff 결과를 보여준다.
-n: 최근 n 개의 결과만 보여준다.
--stat: 각 커밋의 통계 정보
--pretty=oneline: 각 커밋을 한 라인으로 보여준다.
--pretty=short(, full, fuller): 정보를 조금씩 가감하여 보여준다.

나만의 포맷으로 결과를 출력하고 싶을때

`$ git log --pretty=format:"%h - %an, %ar : %s"`

이 외에도 여러 옵션이 있다.

#### 되돌리기

종종 완료한 커밋을 수정해야 할 때가 있다. 다시 커밋하고 싶으면 --amend 옵션을 사용한다.

`$ git commit --amend`

이 명령은 Staging Area를 사용하여 커밋한다. 만약 마지막으로 커밋하고 나서 수정한 것이 없다면(커밋하자마자 바로 이명령을 실행하는 경우) 조금 전에 한 커밋과 모든 것이 같다. 이때는 커밋 메시지만 수정한다.
편집기가 실행되면 이전 커밋 메시지가 자동으로 포함된다. 메시지를 수정하지 않고 그대로 커밋해도 기존의 커밋을
덮어쓴다. 커밋을 했는데 Stage 하는 것을 깜빡하고 빠트린 파일이 있으면 아래와 같이 고칠 수 있다.

    $ git commit -m 'initial commit'
    $ git add forgotten_file
    $ git commit --amend

#### 파일을 Unstage로 변경하기

다음은 Staging Area와 워킹 디렉토리 사이를 넘나드는 방법을 설명한다. 두 영역의 상태를 확인할 때마다 변경된 상태를 되돌리는 방법을 알려주기 때문에 매우 편리하다. 예를 들어 파일을 두 개 수정하고서 따로따로 커밋하려고 했지만, 실수로 git add * 라고 실행해 버렸다. 두 파일 모두 Staging Area에 들어 있다. 이제 둘 중 하나를 어떻게 꺼낼까?

`$ git reset HEAD <file> ....`

이 명령으로 파일을 Unstaged상태로 변경할 수 있다.

git reset 명령은 매우 위험하다. --hard 옵션과 함께 사용하면 더욱 위험하다. 하지만 위에서 처럼 옵션 없이 사용하면 워킹 디렉토리의 파일은 건드리지 않는다.
