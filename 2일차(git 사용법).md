## git 설치하기 ( CentOS )
```
yum install git \\CentOS
apt-get install git \\ubuntu
```
## git Bash 오픈 or 깃저장소(디렉터리 등) 가기
```git init``` \\저장소 초기화

## 이름,이메일 세팅
```
git config --global user.name "[이름]" \\이름입력

git config --global user.email "[github 이메일]" \\이메일 입력

git config --list \\확인
```
## Project 적용하기
VScode로 작업폴더에서 터미널 오픈(Git Bash) 또는 터미널환경에서 시작
```
git init \\git 저장소 초기화

git add . \\( .)은 해당 프로젝트 폴더내 모든파일을 뜻함.

git status \\현재상태 조회

git commit -m "[메세지]" \\준비된 파일을 commit 할때 commit에 대한 메시지 입력

git remote add [이름] [github 리포지터리 주소] \\Git remote 주소입력

git remote -v \\remote 확인
git remote remove [이름] \\원격저장소 연결 끊기

git push [이름] master \\연동된 원격서버로 push (token 발급받고 입력)
```
그 이후로는 git add , git commit, git push origin master으로 연동

## commit명에 대한 일반적인 규칙들
```
git commit -m "커밋 명" 
```
커밋명은 type title으로 이루어짐

#### type
Feat(새 기능 추가), Fix(버그 픽스), Style(포맷, clean up), Test(테스트 코드), Docs(문서 수정), Chore(빌드 관련 업무 수정), Refactor(코드 리펙토링),Update 등
#### title
무었때문에 어떤 작업을 했는지 간결하게 명령어 형태로 적어준다.

## Github에서 가져오기
```
git pull origin master
```
## git push 자동화 (Bash Shell)
```
vi gitpush.sh \\쉘 스크립트 생성(파일 참고)

chmod 750 gitpush.sh \\실행권한 부여

./gitpush.sh \\스크립트 실행
 
cat push.log \\로그파일 열어보기
```
