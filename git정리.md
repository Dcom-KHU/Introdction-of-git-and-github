GIT
===

Git 이전의 VCS(중앙집중식 버전 관리)는 각 파일의 변화를 시간순으로 관리하면서 파일들의 집합을 관리 하지만 Git은 파일의 차이가 아니라 **스냡샷의 스트림**으로 파일을 관리한다.

### 파일의 세가지 상태

1. Modified(Working Directory): 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것

2. Staged(Staging Area): 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태

3. Committed(.git directory): 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것

   > Index라고도 불리우지만, Staging Area라는 명칭이 표준이 되어가고 있다.

   ![](https://cdn-images-1.medium.com/max/1600/1*diRLm1S5hkVoh5qeArND0Q.png)

> Tracked(관리대상) 파일과 Untracked(관리대상이 아님)의 차이란?
>
> 1. Tracked파일 : 이미 스냅샷에 포함돼 있던 파일로 다음 세 가지 상태 중 하나이다.
>    - Unmodified(수정하지 않음) 상태
>    - Modified(수정함) 상태
>    - Staged(커밋으로 저장소에 기록할) 상태
>
> 2. Untracked파일 : Working Directory에 있는 파일 중 스냅샷에도 Staging Area에도 포함되지 않은 파일

![](https://git-scm.com/book/en/v2/images/lifecycle.png)



### 파일의 상태 확인하기

```{.bash}
$ git status
$ git status -s #파일 짤막하게 보기
```



### Modified 상태의 파일을 Stage 하기

파일이 수정되고 git add를 하지 않고 git commit을 하면 수정되어진 내용은 커밋되지 않는다. 따라서 git commit -a옵션을 사용하던, git add를 꼭 해줘야한다.

```{.bash}
$ git add 파일이름
```



### 파일 무시하기

```{.bash}
$ cat .gitignore
*.[oa]
*~
```



### Staged와 Unstaged 상태의 변경 내용을 보기

```{.bash}
$ git diff
$ git diff --cached #만약 커밋하려고 Staging Area에 넣은 파일의 변경 부분을 보고 싶으면
```



### 변경사항 커밋하기

git commit 명령을 실행할 때 -a 옵션을 추가하면 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다.

```{.bash}
$ git commit -m "커밋할 메세지"
$ git commit -a -m '커밋할 메세지' #Staging Area 생략하기
```



### 파일 삭제하기

Git에서 파일을 제거하려면 git rm 명령으로 Staging Area에서 파일을 삭제 한 후에 커밋해야한다.

```{.bash}
$ git rm 파일이름 #실제 파일이 지워진다.
$ git rm -f 파일이름 #Staging Area에서 지우면서 실제 파일도 지움.
$ git rm --cached 파일이름 #Staging Area에서만 파일을 지운다.
```



### 파일 이름 변경하기

```{.bash}
$ git mv 파일이름 바꿀파일이름 #실제 파일도 변경되며 Staging Area 파일도 변경된다.
```



### 커밋 히스토리 조회하기

-p 옵션은 각 커밋의 diff 결과를 보여주고 -2는 최근 두 개의 결과만 보여주는 옵션이다.

```{.bash}
$ git log
$ git log -p -2
```



### 이전 커밋 되돌리기

이미 커밋을 하였으나 새로운 파일을 추가해서 다시 커밋하고 싶을때 사용한다. 아래와 같이 사용하면 두 번째 커밋을 첫 번째 커밋을 덮어써 모두 하나의 커밋으로 기록된다.

```{.bash}
$ git commit -m '커밋할 메세지'
$ git add 실수로못올린파일
$ git commit --amend
```



### 파일 상태를 Unstage로 변경하기

두 개의 파일을 모두 실수로 Staging Area에 올려버렸을 때, 그 중 한 개의 파일만 Working Directory로 가져올 때reset 명령을 사용한다.

```{.bash}
$ git add * #실수로 두 개의 파일을 모두 Staging Area에 올렸다.
$ git reset HEAD 파일이름
```



### Modified 파일 되돌리기

최근 커밋된 버전(처음 git clone 했을 때나 Working Directory에 처음 Checkout한 내용)으로 되돌리라면 checkout -- 명령어를 사용한다. 하지만 이 명령어는 수정된 내용을 모두 지울 수 있으니 주의해야한다.

```{.bash}
$ git checkout -- 파일이름
```



### 리모트 저장소

저장소를 clone하면 명령은 자동으로 리모트 저장소를 "origin" 이라는 이름으로 추가한다.  

git fetch 명령은 리모트 저장소의 데이터를 모두 로컬로 가져오지만 자동으로 Merge하지 않는다.  

git pull 명령은 리모트 저장소 브랜치에서 데이터를 가져올 뿐만 아니라 로컬 브랜치와 Merge할 수 있다.

```{.bash}
$ git remote -v #리모트 저장소 확인하기
$ git remote add 단축이름 URL #리모트 저장소 추가하기
$ git fetch 단축이름 #리모트 저장소에서 데이터를 가져오기
$ git pull 단축이름 #리모트 저장소에서 데이터를 가져오기
$ git push origin master #origin서버에 master 브랜치를 push
$ git remote show origin #리모트의 구체적인 정보를 확인
$ git remote rename 단축이름 바꿀단축이름 #리모트 저장소의 이름 변경
$ git remote rm 단축이름 #리모트 저장소 삭제
```



## 브랜치

- 새 브랜치 생성하기

```{.bash}
$ git branch 브랜치이름
```
- 브랜치 이동하기

```{.bash}
$ git checkout 브랜치이름
$ git checkout -b 브랜치이름 #브랜치를 만들면서 Checkout까지 한 번에 하려면 -b 옵션을 추가한다.
```
- 브랜치 Merge
  merge는 [브랜치이름]을 현재 checkout되어있는 브랜치에 병합시킨다. 
```{.bash}
$ git merge 브랜치이름
```
1. **Merge할 브랜치가 가리키는 커밋이 현 브랜치 커밋의 Upstream 브랜치일경우**

![](https://git-scm.com/book/en/v2/images/basic-branching-4.png)
```{.bash}
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

![](https://git-scm.com/book/en/v2/images/basic-branching-5.png)

2. **Merge할 브랜치가 가리키는 커밋이 현 브랜치 커밋의 Upstream 브랜치이 아닌 경우(3-way Merge)**
   ![](https://git-scm.com/book/en/v2/images/basic-branching-6.png)
```{.bash}
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

![](https://git-scm.com/book/en/v2/images/basic-merging-2.png)



### 이미지 출처

- [https://medium.com/@lucasmaurer/git-gud-the-working-tree-staging-area-and-local-repo-a1f0f4822018](https://medium.com/@lucasmaurer/git-gud-the-working-tree-staging-area-and-local-repo-a1f0f4822018)
- [https://git-scm.com/book/ko/v2/Git의-기초-수정하고-저장소에-저장하기](https://git-scm.com/book/ko/v2/Git의-기초-수정하고-저장소에-저장하기)
