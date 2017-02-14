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
기존의 모델 객체에 `primary_key` 필드 값을 변경하고 저장하는 경우, 기존 모델 객체의 `primary_key` 필드값이 바뀌는 것이 아니라, 새로운 모델 객체가 생긴다. 기존 모델객체는 DB 상에서 지워지지 않는다.  
기존 'Apple' 객체에 'Pear' 객체가 추가된 것을 볼 수 있다.  

--

#### unique

> 참이면, 필드는 테이블 전체에서 고유/유일(unique) 해야 한다.  

--

Details..  
[commom model field option reference](https://docs.djangoproject.com/en/1.10/ref/models/fields/#common-model-field-options)

## Automatic primary key fields

기본적으로 **Django** 는 각 모델에 다음과 같은 **field** 를 제공한다.  

```
id = models.AutoField(primary_key=True)
```
이것은 자동으로 증가하는 `primary_key` 이다.  
직접 `primary_key` 를 지정하려면, 모델의 필드 중 하나에 `primary_key=True` 라는 옵션을 줘야 한다.  
`primary_key=True` 라는 명시적인 옵션이 있으면 Django 는 자동적으로 id 필드를 생성하지 않는다.  
Django 의 모델은 필수적으로 (자동 or 수동) 하나의 `primary_key` 를 가져야 한다.  

## Verbose field names

읽기 좋은 형태의 필드 이름.  
**ForeignKey**, **ManyToManyField**, **OneToOneField** 를 제외한 나머지 필드들은 공통적으로 첫 번째 인자(optional)로 **verbose name** 값을 받는다.  
만약, **verbose name** 이 주어지지 않으면 Django 는 필드의 속성 이름을 **verbose name** 으로 사용한다. 이 때, Django 는 속성 이름에서 밑줄은 공백으로 변환하여 **verbose name** 으로 사용한다.  

> Ex) **verbose name** 을 `person's first name` 으로 정해줬을 때    
> (**verbose name** = `person's first name`)   

```
first_name = models.CharField("person's first name", max_length=30)
```

> Ex) **verbose name** 을 지정하지 않았을 때  
> (**verbose name** = `first name`)

```
first_name = models.CharField(max_length=30)
```

**ForeignKey**, **ManyToManyField**, **OneToOneField** 는 첫 번째 인자로 모델 클래스를 받기 때문에, 해당 타입의 필드에 **verbose name** 을 지정하려면 아래와 같이 한다.  
```
poll = models.ForeignKey(Poll, verbose_name="the related poll")  
site = models.ManyToManyField(Site, verbose_name="list of sites")   
place = models.OneToOneField(Place, verbose_name="related place")  
```
**verbose name** 을 지정할 때 는 관습적으로 첫 글자를 대문자화 하지 않고 소문자로 지정한다. Django 가 필요한 경우 알아서 대문자화 하기 때문

## Relationships

관계형 데이터베이스의 가장 큰 장점은 테이블들을 서로 연결시키는 데 있다.  
Django 는  대표적인 데이터베이스 관계 형태인 **many-to-one**(일대다), **many-to-many**(다대다), **one-to-one**(일대일) 3가지를 제공한다.   

### Many-to-one relationships
일대다 관계를 정의하려면 `django.db.models.ForeignKey` 클래스를 이용하여 필드를 선언하면 된다.  
**ForeignKey** 필드를 선언하려면 관계를 맺을 모델 클래스를 인자로 넘겨주어야 한다.  
> Ex) 제조업체가 여러 자동차를 생산하고, 각 자동차에는 하나의 제조업체가 있는 경우 (Manufacturer - Car 모델이 일대다 관계일 때)

```
from django.db import models

class Manufacturer(models.Model):
	#...
	pass
	
class Car(models.Model):
	manufacturer = models.ForeignKey(Manufacturer)
	#...
```
이 때, 재귀적 관계(클래스 자체와 앨디다 관계가 있는 객체)로 선언할 수 도 있다. 재귀적 관계로 **ForeignKey** 필드를 선언할 때, 첫 번째 인자를 클래스가 아닌 **클래스명**으로 문자열을 전달해야 한다. 필드가 선언되는 시점에는 아직 클래스가 생성되지 않았기 때문이다. 클래스 대신 **클래스명**을 인자로 사용함으로써 클래스 선언 순서에 관계없이 참조가 가능해 진다.  

```
class Employee(models.Model):
	boss = models.ForeignKey('Employee')
	#...
```
--

```
from django.db import models

class Manufacturer(models.Model):
	#...
	pass
	
class Car(models.Model):
	company_that_makes_it = models.ForeignKey(
		Manufacturer,
		on_delete=models.CASCADE,
	)
	#...
```
* **ForeignKey** 필드의 이름은 **소문자로** 하는것을 권장한다.
* `on_delete=models.CASCADE`  
	* 계단식 삭제  
	* Django는 **ON DELETE CASCADE SQL** 제약 조건의 동작을 에뮬레이션하고, **ForeignKey**가 포함 된 객체도 삭제한다.(???)  

### Many-to-many relationships

다대다 관계를 선언할때는 **ManyToManyField** 를 사용한다.  
**ForeignKey** 필드와 마찬가지로, 관계를 가지는 모델 클래스를 첫 번째 인자로 받는다.  
다대다 관계는 위치 인수가 필요하다.  

> Ex) 피자에 여러 개의 토핑 객체가 있는경우 (토핑이 여러 피자에 있을 수 있으며, 각 피자에 여러 토핑이 있는 경우)   

```
from django.db import models

class Topping(models.Model):
	#...
	pass
	
class Pizza(models.Model):
	#...
	toppings = models.ManyToManyField(Topping)
```
* **ManyToManyField** 의 필드 이름은 복수로 하는것을 추천한다.
* **ManyToManyField** 는 관계를 가지는 두 모델 중 한쪽에만 선언하면 된다.
* 일반적으로 **form** 에서 모델을 편집하기 때문에 편집하기 편한 모델에 선언하는것이 일반적이다. (피자에 토핑을 올리니까 피자 모델에 토핑을 추가)

### Extra fields on many-to-many relationships(중간자 모델)

때로는 피자와 토핑처럼 단순한 다대다 관계 처리 외에 두 모델 사이의 관게에 관한 데이터를 연결해야 하는 경우도 있다. 이 때 사용하는 것이 **중간자모델**이다.