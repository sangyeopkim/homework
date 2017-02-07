# Django

## 설치
* 해당 경로로 이동 (작업 할 공간)
* `mkdir django_tutorial`(작업 할 폴더 생성)
* `pyenv virtualenv 3.4.3 django_tutorial_env`
* `pyenv local django_tutorial_env`
* `pip install django`
* `pip list`
* `django-admin startproject mysite`
* `brew search tree` -> tree 형태로 출력 가능한 패키지
* `mv mysite django_app` -> 최상위 mysite 폴더명 변경
* `pip freeze > requirements.txt`
* `git init`
* `vi .gitignore`
* gitignore.io 사이트에서 python, pycharm, django 내용 복붙 -> `vi .gitignore`

## Git 연결
* `Git New Repository` 생성
* `git init`
* `git remote add origin https://github.com/sangyeopkim/Django_Tutorial.git`
* `git remote -v` 로 확인
* `git status`
* `git add -A`
* `git commit`
* `git push origin master` push
* `rm -rf .git` git 날리기(삭제)
* `git push origin master --force` git 날리고 새로운 git repository 생성해서 기존 repository 에 덮어 씌울 때 강제로 사용

## 실행
* Terminal (iterm) 에서 `py .`명령어로 Pycharm 실행  
* 해당 경로 (manage.py)있는 경로 에서 `./manage.py runserver` 로 서버 동작 확인  
* python -m django --version 으로 버전 확인


