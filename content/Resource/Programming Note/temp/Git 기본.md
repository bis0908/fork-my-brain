  

2022년 1월 10일  
월요일

오전 11:41

디렉토리 선택 후

git init  
// 깃 초기화

git remote add (branch name) [https://github.com/iconcert8/test  
//](https://github.com/iconcert8/test%20/) 원격저장소 연결

git fetch (branch name) // 원격저장소 정보 가져오기(branch등)

git pull (branch name) master(or main) // 원격저장소 소스 가져오기

1. 해당 명령어 입력시 오류가 'fatal: couldn't find remote ref master'로 뜨는 경우 기본 생성 브랜치 이름 입력이 입력한 것과 달라서 나타난 것으로 Repo에서 확인 후 name을 다시 입력하면 된다.

git checkout -b feature/test // 새로운 branch (feature/test) 만들기

git checkout -t origin/develop // 원격저장소 develop branch로 바꾸기

git push origin develop // 원격 branch에 push, 원격에 해당 branch 없을경우  
원격에 brach 생성됨

git status // 변경사항 확인

git add --all // 변경사항 staging 올리기

git commit -m 'test.txt 내용추가' // staging commit

git push origin feature/test //원격 brach에 push

git merge feature/test // develop에서 merge

git push origin develop // merge 결과 push

git remote -v //remote 확인

- ---------------------------------------------------------------
- 자주 사용하는 기초 Git 명령어 정리

[https://pks2974.medium.com/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B8%B0%EC%B4%88-git-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-533b3689db81](https://pks2974.medium.com/%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B8%B0%EC%B4%88-git-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-533b3689db81)

git pull // 코드를 받아와  
변경점을 merge한다.

git fetch // 코드를 받아온다.  
git branch -r : 원격 브랜치 목록보기

git branch -a : 모든 브랜치 목록보기

git push -u origin my_branch:remote_branch: 현재 브랜치를 다른 브랜치에 push

Git Error 모음

1. error: Cannot delete branch 'feature' checked out at ~
    1. 현재 브랜치가 삭제하고자 하는 브랜치이기 때문. 브랜치를 전환한 후에 삭제하면 된다 (ex. git checkout master)
2. Remote origin already exists
    1. 기존에 연결되어 있는 레파지토리가 다시 새로운 레파지토리에 소스코드를 올리려고 하면 발생되는 에러
    2. 해결 방법: git remote remove 'repo_name'
3. git 브랜치가 목록에서 보이지 않을 때
    1. git remote update