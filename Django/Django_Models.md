# Django Models

## Models
> 객체의 특별한 종류  
> **Model** 을 저장하면 그 내용이 **DataBase**에 저장 된다.  
> **Model** 은 **DataBase Table** 에 매핑 된다.  
> **Model** 은 `django.db.models.Model` 에 상속 된다.  
> **Model** 의 각 속성은 **DataBase Field** 를 나타낸다.  
> 이를 통해 **Django** 는 자동으로 **DataBase**를 생성하고, 접근이 가능하다.  

### Example
```
from django.db import models

class Person(models.Model):
	first_name = models.CharField(max_length=30)
	last_name = models.CharField(max_length=30)
```

* **first_name** 과 **last_name** 은 **Model**의 **Field**를 나타낸다.
* 각 **Field**들은 클래스 **속성**으로 지정된다.
* 각 **속성**은 **DataBase**의 **열(column)** 에 매핑 된다.

### Person 이라는 모델은 다음과 같은 DataBase Table 을 생성한다.  

```
CREATE TABLE myapp_person (  
	"id" serial NOT NULL PRIMARY KEY,  
	"first_name" varchar(30) NOT NULL,  
	"last_name" varchar(30) NOT NULL  
	);
```

* myapp_person 은 자동으로 생성되지만 override 가 가능하다.
* **id** field 는 자동으로 추가 된다. 역시 override 가능하다.


## Using models
> **Model**을 정의 한 후에는 **Django**에게 해당 모델을 사용한다고 알려줘야 한다.  
> 이 작업들은 `settings.py` 에서 해준다.  
> `INSTALLED_APPS` 에서 `models.py` 가 포함 된 모듈의 이름을 추가 해 준다.  

### Example
```
INSTALLED_APPS = [
	#...
	'myapp',
	#...
]
```
> `INSTALLED_APPS` 를 변경할 때 반드시 `manage.py migrate` 를 실행하고 선택적으로 `manage.py makemigrations`를 실행해야 한다.

## Field
> **Model** 에서 가장 중요하고 유일하게 요구되는 부분은 **DataBase field** 를 정의하는 것이다.  
> **Field** 는 클래스 속성에 의해 지정된다.  
> **models API** 와 겹치지 않는 **field name** 을 지정하도록 주의해야 한다. (clean, save, delete.. 등)  

### Example
```
from django.db iport models

class Musicion(models.Model):
	first_name = models.CharField(max_length=50)
	last_name = models.CharField(max_length=50)
	instrument = models.CharField(max_length=100)

class Album(models.Model):
	artist = models.ForeignKey(Musicion, on_delete=models.CASCADE)
	name = models.CharField(max_length=100)
	release_date = models.DateField()
	num_stars = models.IntegerField()
```

**Musicion** 모델의 **Field**  

* first_name  
* last_name  
* instrument  

**Album** 모델의 **Field**  

* artist
* name
* release_date
* num_stars

## Field types




