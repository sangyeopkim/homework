# Django Tutorial

## Part 1

### 1.1 설치된 Django 버전 확인
* `python -m django --version`    

### 1.2 프로젝트 만들기
* 작업 할 폴더 생성 및 작업 폴더로 경로 이동
	1. `mkdir django_tutorial`
	2. `cd django_tutorial`
* 해당 경로에서 프로젝트 생성
	1. `django-admin startproject mysite` 
	2. 해당 경로에서 `mysite` 라는 디렉토리 생성하는 명령
	3. `django-admin` : 관리자 작업을 위한 Django의 명령형 유틸리티

### 1.3 startproject 에서 생성된 파일들

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

* `mysite/` : 단순히 프로젝트를 담는 공간
* `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티
* `mysite/` : Project를 위한 실제 Python 패키지들이 저장되는 공간. 이 디렉토리 내의 이름을 이용하여 project 어디서나 Python 패키지들을 import할 수 있음
* `mysite/__init__.py` : Python 으로 하여금 이 디렉토리를 패키지 처럼 다루라고 알려주는 용도의 단순한 빈 파일
* `mysite/settings.py` : 현재 Django project의 환경/구성을 저장
* `mysite/urls.py` : 현재 Django project 의 URL 선언을 저장. Django 로 작성된 사이트의 "목차"라고 할 수 있음
* `mysite/wsgi.py` : 현재 project를 서비스 하기 위한 WSGI 호환 웹 서버의 진입점

### 1.4 개발 서버
* `manage.py`가 있는 `mysite`로 이동
* `python manage.py runserver`
* `./manage.py runserver`
* 위 두개의 명령어 중 하나로 server 동작 확인 가능  

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

February 03, 2017 - 15:50:53
Django version 1.10, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
* 다음과 같은 출력으로 정상 작동 확인 가능
* 웹 브라우져에서 `http://127.0.0.1:8000/`를 통해 접속 가능
* `python manage.py runserver 8080` 명령어를 통해 내부 IP 포트 변경 가능
* `python manage.py runserver 0.0.0.0:8000` 명령어를 통해 서버의 IP 변경 가능

### 1.5 설문조사 앱 만들기
* app 생성
* `python manage.py startapp polls` 명령어를 통해 `polls` app 생성

* `polls` 디렉토리에 자동 생성 된 파일들

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

### 1.6 첫 번째 뷰 작성하기
`polls/view.py` 파일을 열어 python 코드 입력

```
from django.http import HttpResponse

def index(request):
	return HttpResponse("Hello, World")
```

--
view를 호출하려면 이와 연결된 URL이 있어야 한다.  
이를 위해 URLconf가 사용 된다.  
`polls/urls.py` 파일을 열어 코드 입력/확인

```
from django.conf.urls import url

from . import views

urlpatterns = [
	url(r'^$', views.index, name='index'),
]
```
--
project 최상단의 URLconf에서 `polls/urls` 모듈을 바라보도록 설정  
`mysite/urls.py` 파일 오픈  
다음과 같은 코드 추가

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
	url(r'^polls/', include('polls.urls')),
	url(r'^admin/', admin.site.urls),
]
```

* `include()` : 다른 URLconf를 참조할 수 있도록 도와줌  
* 정규표현식에서 끝을 의미하는 `$` 대신 `/`로 끝을 나타냄
* Django가 `include()`를 만나게 되면, 그 지점까지 일치하는 URL의 부분을 잘라내고, 남은 부분을 후속 처리하기 위해 include 된 URLconf로 전달
* `/polls/some/method` 를 요청 받으면, `some/method`가 `polls/urls.py`의 URLconf로 넘어간다.

--

* index view가 URLconf에 잘 연결되었는지 확인
* `python manage.py runserver`  
* 웹 브라우저에서 `http://localhost:8000/polls/` 입력 후 확인 가능 


## Part 2

### 2.1 데이터베이스 설치
* 데이터베이스 생성
* `python manage.py migrate`
* `migrate` 명령은 `setting.py` 안에`INSTALLED_APPS`에서 데이터베이스 설정과 app과 함께 제공되는 데이터베이스 migrations에 따라, 필요한 데이터베이스 테이블을 생성한다.

### 2.2 모델 만들기
#### 모델 ?
> 모델("model")은 정의한 내용들(데이터들)을 데이터베이스에 저장 해 주는 역할을 한다.

* `Question`, `Choice` 라는 두 개의 모델 생성 
* `Question`은 `question(질문)`, `publication date(발행일)` 을 위한 두 개의 필드를 가짐
* `Choice`는 `choice(선택지)` 와 `vote(표)` 계산을 위한 두 개의 필드를 가짐
* 각 `Choice` 모델은 `Question` 모델과 연관된다.

#### Python 에 코드 입력
* `polls/models.py` 실행

```
from django.db import models

class Question(models.Model):
	question_text = models.CharField(max_length=200)
	pub_date = models.DateTimeField('date published')

class Choice(models.Model):
	question = models.ForeignKey(Question, on_delete=models.CASCADE)
	choice_text = models.CharField(max_length=200)
	votes = models.IntegerField(default=0)
```

#### django.db.models.Model
* 각 모델들이 서브클래스로 표현된다는 뜻
* 각 모델은 여러개의 클래스 변수를 가질 수 있음
* 각각의 클래스 변수들은 모델의 데이터베이스 필드를 의미

#### Field
* 데이터베이스의 각 필드는 Field 클래스의 인스턴스로 표현
* `CharField` : 문자 필드를 표현
* `DateTimeField` : 날짜와 시간 필드를 표현
* 각 필드가 어떤 자료형을 가질 수 있는지 Django에 알려줌
* 몇몇 Field 클래스들은 필수 인수가 필요함. 예를 들어, `CharField`의 경우 `max_length`를 입력해야함
* 다양한 선택적 인수들을 가질 수 있음. 예를 들어, `default` 로 하여금 `votes`의 기본값을 `0`으로 설정

#### ForeignKey
* 각각의 `Choice`가 하나의 `Question`에 관계된다는 것을 Django에 알려주는 역할  
* Django 는 `다-대-일`, `다-대-다`, `일-대-일` 과 같은 모든 일반 데이터베이스의 관계들을 지원

### 2.3 모델의 활성화





