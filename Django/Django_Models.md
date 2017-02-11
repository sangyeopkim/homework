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

> **Model** 의 각 **Field**들은 해당 **Field** 클래스의 인스턴스여야 한다.  
> **Django** 는 **Field Class** type 을 사용해 몇 가지를 결정한다.  

* **column type** 은 **DataBase** 에 어떤 **Data**들이 저장되었는지 알려준다.(INTEGER, VARCHAR, TEXT)  
* **form field** 를 rendering 할 때, 기본 **HTML widget**을 사용한다.  
* **Django** 에서 사용되는 관리자 및 자동 생성 form 에 대한 최소한의 요구 사항이 있다.


## Field options
> 각 **Field** 는 특정한 **arguments** 를 가진다.  
> 예를 들어 **CharField(및 해당 하위 클래스)** 에는 데이터 저장에 사용되는 **VARCHAR** 데이터베이스 필드의 크기를 지정하는 **max_length** 인수가 필요하다.  


### Commom arguments
> 모든 필드 유형에서 사용할 수 있는 **common arguments** 도 있다. 모두 선택 사항이다.  

#### null  

>**True** 이면, **Django** 는 빈 값을 **NULL** 로 데이터 베이스에 저장한다. 기본값은 **False**.  

--

#### blank
  
> **True** 이면, 필드를 비워 둘 수 있다. 기본값은 **False**.  
`null` 과는 다른 개념.  
`null` 은 데이터베이스와 관련된 반면 `blank`는 유효성 검사와 관련이 있다.  
`blank=True` 가 있으면 빈 값을 입력 할 수 있다.  
`blank=False` 가 있으면 필드가 필요하다.  

--

#### choices 
 
> 반복적인 2개의 tuples 를 사용할 때 사용한다.   
`choice` 가 주어지면, 기본 양식 widget은 표준 텍스트 필드 대신 **선택 상자**가 된어 주어진 선택 사항으로 선택 항목을 제한한다.  

##### choices Example
```
YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
)
```

* 각 튜플의 첫 번째 요소는 데이터베이스에 저장 될 값이다.  
* 두 번째 요소는 기본 양식 위젯 또는 `ModelChoiceField` 에 표시 된다.  
* 모델 인스턴스가 주어지면 선택 필드의 표시 값은 `get_FOO_display()` 메소드를 사용하여 엑세스 할 수 있다.  

##### choice Example 2 (엑세스)
```
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```
```
>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'
```
`p.get_shirt_size_displat()` 를 통해 출력  

--

#### default

> 필드의 기본 값.   
> 값 또는 호출 가능 객체 일 수 있다.  
> 호출 가능하면 새로운 객체가 생성 될 때 마다 호출 된다.  

--

#### help_text

> 추가 "도움말" 텍스트를 표시 해 준다.  
> form 에서 필드를 사용하지 않아도 문서화에 유용하다.  

--

#### primary_key

> **True** 이면, 필드는 모델의 **기본 키** 이다.  
`primary_key=True` 를 지정 해 주지 않으면 **Django** 는 기본키를 보유 할 **IntegerField** 를 자동으로 추가한다.  
default primary-key 를 덮어 쓰지 않으려면 모든 필드에서 `primary_key=True` 를 설정 할 필요가 없다.

> **primary key** 는 읽기 전용이다.  
> 기존 객체의 **primary key** 값을 변경 한 다음 저장하면, 이전 객체와 함께 새 객체가 만들어 진다.

##### primary_key Example
```
from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)
```
```
>>> fruit = Fruit.objects.create(name='Apple')
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True)
['Apple', 'Pear']
```
`Apple` 만 있었는데, `Pear` 가 추가되고, 두 개 전부 출력 된 모습을 볼 수 있다.  

--

#### unique

> 참이면, 필드는 테이블 전체에서 고유/유일(unique) 해야 한다.  

--

Details..  
[commom model field option reference](https://docs.djangoproject.com/en/1.10/ref/models/fields/#common-model-field-options)

## Automatic primary key fields




