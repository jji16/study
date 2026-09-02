Git 기본 명령어 정리

1. 저장소 생성 및 상태 확인

git init
- 새로운 Git 저장소 생성

git status
- 현재 파일의 상태 확인

git log
- 커밋 이력 확인


2. 변경 사항 추가

git add 파일이름
- 특정 파일의 변경 사항을 Staging Area에 추가

예시:
git add index.html

git add .
- 모든 변경 사항을 Staging Area에 추가


3. 커밋

git commit -m "커밋 메시지"
- 변경 사항을 커밋

예시:
git commit -m "로그인 기능 추가"

git commit
- -m을 사용하지 않으면 커밋 메시지를 작성하는 편집기가 실행됨

Vim 편집기가 실행되었을 경우:
ESC
:wq
Enter
- 저장 후 종료


4. 브랜치

git branch
- 브랜치 목록 확인

git branch 브랜치이름
- 새로운 브랜치 생성

예시:
git branch develop


git switch 브랜치이름
- 해당 브랜치로 이동

예시:
git switch develop


git switch -c 브랜치이름
- 브랜치를 생성하고 해당 브랜치로 바로 이동

예시:
git switch -c feature/login


5. 브랜치 합치기

git merge 브랜치이름
- 현재 브랜치에 다른 브랜치를 합침

예시:
git switch main
git merge develop



6. 원격 저장소

git remote
- 원격 저장소 관리

git remote -v
- 연결된 원격 저장소 확인

git remote add origin URL
- 원격 저장소 연결

예시:
git remote add origin https://github.com/사용자이름/저장소이름.git

origin
- 원격 저장소를 가리키는 이름(별칭)


7. 원격 저장소 복제

git clone URL
- 원격 저장소의 소스 코드를 내 컴퓨터로 복제

예시:
git clone https://github.com/사용자이름/저장소이름.git

git init
- 새로운 Git 저장소 생성

git clone
- 이미 존재하는 원격 저장소를 내 컴퓨터로 복제


8. 원격 저장소에 업로드

git push origin 브랜치이름
- 로컬의 커밋을 원격 저장소에 업로드

예시:
git push origin main

또는:
git push origin develop


9. 기본 작업 흐름

git init
git add .
git commit -m "첫 번째 커밋"
git remote add origin URL
git push origin main


10. 브랜치를 만들어 작업하는 경우

git switch -c feature/login
git add .
git commit -m "로그인 기능 추가"
git push origin feature/login


11. 브랜치를 main에 합치는 경우

git switch main
git merge feature/login
git push origin main


12. 핵심 명령어

git init
- Git 저장소 생성

git add
- 변경 사항 추가

git status
- 현재 상태 확인

git commit
- 변경 사항 저장

git log
- 커밋 기록 확인

git branch
- 브랜치 확인 및 생성

git switch
- 브랜치 이동

git switch -c
- 브랜치 생성 후 이동

git merge
- 브랜치 병합

git remote
- 원격 저장소 관리

git clone
- 원격 저장소 복제

git push
- 원격 저장소에 업로드


Git 기본 흐름

작업 -> git add -> git commit -> git push -> 원격 저장소(GitHub)
ff