# Git

## Git 개념
<br>

![git_repo](./img/git_repo.png)

- 중요 명령어
    + git clone : 원격 저장소를 로컬로 clone
    + git add <파일이름> : rocal에서의 수정 이력을 stage 영역으로 add
    + git commit -m 'message' : stage 영역을 commit으로 생성
    + git push [remote] [rocal] : rocal의 commit을 origin으로 push
    + git fetch : origin의 commit을 rocal로 fetch
    + git merge : commit을 rocal diretory에 저장
    + git pull : fetch + merge를 한꺼번에 진행
    + git reset : 해당 commit으로 branch를 이동
    + git checkout : 해당 commit 또는 branch로 head를 이동
- 명령어 추가 설명
    + git clone --bare & --mirror : 현재 repo를 다른 repo로 복제할때 이용
        * --bare : 원격저장소 정보를 같이 저장 X, 추후에 원격저장소를 추적 X
        * --mirror : 원격저장소 정보를 같이 저장
    + git push -u [remote] [rocal] : rocal을 처음 remote에 push할때에 push할 원격저장소를 지정 