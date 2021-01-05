---
title: Git command & Flow
categories:
- Git
toc: true
toc_sticky: true
---

# 여러 상황에 따른 Git command와 flow
> Git을 사용하다보면 자주하는 명령어는 외울 수 있지만, 가끔 사용하는 명령어를 바로 기억해내기는 쉽지 않다. <br>
> Github 위주로 Git을 이용하는 것을 가정한다.


## Command
``` bash
# 현재 저장소 상태 확인
git status

# 저장소 HEAD와 워킹 디렉토리의 차이점을 보여주는 명령어
git diff
git diff [파일명]
git diff --word-diff (지워진/추가된 단어를 명확하게 보여줌)

# 새로운 파일을 추적하거나, 수정한 파일을 staged 상태로 만들기
git add [파일명]
git add . (gitignore은 제외하고 staged로 만듦)

# git add 취소
git reset: 전체 파일 add 취소
git reset HEAD [파일]: 특정 파일 add 취소

# staged 상태의 존재하는 변경사항들 commit
git commit -m "message"
# git add와 commit을 한번에
git commit -am "message"

# 브랜치 변경 강제
git checkout -f [branch_name]
# 브랜치의 내용을 master로 덮어쓰는 방식의 merge
git checkout master (master 브랜치로 이동)
git merge -Xtheirs [branch] (덮어쓸 내용이 있는 branch를 merge)
git push origint master (리모트에 반영)
```

``` bash
# 저장소 로컬에 내려받기
git clone [url] (remote 저장소 이름의 기본값은 origin)

# 현재 연결되어 있는 git 저장소 확인
git remote -v
# 연결되어 있는 remote url 변경
git remote set-url origin [변경할 원격 저장소 주소]

# Remote 저장소를 local에 덮어쓰기 (remote에만 있는 데이터 가져옴)
git fetch --all
git reset --hard origin

# 리모트 저장소에 push
git push origin master
git push [remote 저장소 이름] [브랜치 이름]
# 최신 버전의 소스를 로컬 저장소에 내려받기
git pull
# Fork한 저장소의 기존 원격 저장소 연결하기
git remote add upstream [기존 저장소 주소]

```

## Flow
* 오픈소스를 내 상황에 맞는 버전으로 활용하고 싶을 때 (오픈소스에 기여를 하는 목적이 아님)
  * Fork한 저장소를 clone하여 로컬 저장소에 보관
  * Git remote add upstream(관례적인 이름) 원본저장소.git
  * 위와 같이 한 후, 나의 버전을 개발 하면서, Git fetch upstream 을 통해 원본 저장소에 최신 변경 사항을 반영할 수 있음

* 하위 디렉터리, 폴더만 클론(clone) 하기
  * 참고 [1](https://www.lesstif.com/gitbook/git-clone-20776761.html)
``` bash
# clone 할 로컬 저장소를 만든다.
git init my-proj
cd my-proj
# sparse Checkout 이 가능하도록 설정한다.
git config core.sparseCheckout true
# remote 를 추가한다.
git remote add -f origin <REMOTE_URL>
# checkout 하기 원하는 파일이나 폴더를 .git/info/sparse-checkout 파일에 기술하면 된다.
##  폴더일 경우 자동으로 하위 폴더가 포함된다.
echo "[하위 폴더 경로]" >> .git/info/sparse-checkout
echo "script/sys-script" >> .git/info/sparse-checkout
echo "script/user-script/user1" >> .git/info/sparse-checkout
# 이제 pull 로 원격 저장소에서 파일을 가져오면 sparse-checkout 에 기술한 경로의 파일만 가져온다.
git pull origin master
``` 

* 대용량 파일 git으로 관리하기
  * git lfs (large file storage) 이용
  * 참고 [1](https://confluence.atlassian.com/bitbucketserver/git-large-file-storage-794364846.html), [2](https://devlog.github.io/git-lfs/2015/12/09/git-lfs.html)

``` bash
# git lfs client 설치 후,
## initialize 하는 것
git lfs install
git lfs track "largefilename"
git add .gitattributes
git add "largefilename"
git commit -m "message"
git push
```

* git 특정 개체 제거
  * 참고 [1](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EB%82%B4%EB%B6%80-%EC%9A%B4%EC%98%81-%EB%B0%8F-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B3%B5%EA%B5%AC), [2](https://git-scm.com/docs/git-verify-pack), [3](http://minsone.github.io/git/github-advanced-remove-sensitive-data)
  
``` bash
# pack 파일 별 용량 확인
git verify-pack -v .git/objects/pack/[.idx]
# pack에 포함된 SHA 값의 파일 명
git rev-list --objects --all | grep [SHA 값]
# 히스토리에 있는 모든 Tree 개체에서 특정 파일을 추가한 커밋 확인
git log --oneline --branches -- [파일명]

```

## Gitlab
``` bash
# Git global setup
git config --global user.name "dghong"
git config --global user.email "dghong@saltlux.com"
		
# Create a new repository
git clone ssh://git@gitlab.ziny.us:2224/ai-labs-air/knowledge-extraction-from-documents.git
cd knowledge-extraction-from-documents
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
		
# Push an existing folder
cd existing_folder
git init
git remote add origin ssh://git@gitlab.ziny.us:2224/ai-labs-air/knowledge-extraction-from-documents.git
git add .
git commit -m "Initial commit"
git push -u origin master
		
# Push an existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin ssh://git@gitlab.ziny.us:2224/ai-labs-air/knowledge-extraction-from-documents.git
git push -u origin --all
git push -u origin --tags
```

* SVN to gitlab
  * 참고 [1](https://git-scm.com/book/ko/v2/Git%EA%B3%BC-%EC%97%AC%ED%83%80-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC-%EC%8B%9C%EC%8A%A4%ED%85%9C-Git%EC%9C%BC%EB%A1%9C-%EC%98%AE%EA%B8%B0%EA%B8%B0), [2](https://docs.gitlab.com/ee/user/project/import/svn.html)
``` bash
# Install git
sudo apt-get install git
# Install git-svn
sudo apt-get install git-svn
# Clone SVN repository by git-svn command.
git svn clone [YOUR_SVN_URL]
# Adding Gitlab url in the same directory.
git remote add [gitlab YOUR_GITLAB_URL]
# Finally, push your SVN repository to Gitlab.
git push — set-upstream gitlab master
## or 
git remote add origin git@gitlab.com:<group>/<project>.git
git push --all origin
git push --tags origin
```