# Field Types

## AutoField

`class AutoField(**options)`
> 사용 가능한 ID 에 따라 자동으로 증가하는 **IntegerField** 이다.  
> 보통 직접 사용 할 필요는 없다.  
> 별도로 지정하지 않으면 기본 키 필드가 자동으로 모델에 추가 된다.  

## BigAutoField

`class BigAutoField(**options)`
> 1 에서 9223372036854775807 까지의 숫자에 맞도록 보장된다는 점을 제외 하고는 **AutoField** 와 매우 유사한 64 비트 정수이다.  

## BigIntegerField

`class BigIntegerField(**options)`
> -9223372036854775808에서 9223372036854775807 까지의 숫자를 맞출 수 있다는 점을 제외하고는 **IntegerField**와 매우 흡사 한 64 비트 정수이다.  
> 기본 양식 위젯은 **TextInput** 이다.  

## BinaryField

`class BinaryField(**options)`
> 이진 데이터 (Binary Data) 를 저장하는 필드이다.  
>**bytes** 할당만 지원 한다.  
> 이 필드는 기능이 제한되어 있다.  
> **BinaryField** 값에 대한 **쿼리 집합**을 필터링 할 수 없다.  
> **BinaryField** 를 **ModelForm** 에 포함시킬 수 없다.  

## BooleanField

`class BooleanField(**options)`
> `true/false` field.  
> **CheckboxInput** 이 기본 양식 위젯이다.  
> **null** 값을 받고 싶으면 **NullBooleanField** 를 사용  
> **Field.default** 가 정의되어 있지 않으면,  **BooleanField** 의 기본값은 **None**이다.  

## CharField

`class CharField(max_length=None, **options)`
> 작은 크기 부터 큰 크기의 문자열을 나타내는 문자열 필드이다.  
> 텍스트의 양이 많으면 **TextField** 를 사용  
> 이 필드의 기본 양식 위젯은 **TextInput** 이다.   
> 하나의 추가 필수 argument 가 필요하다. (**max_length**)  
> 필드의 최대 길이 (문자 수)를 지정 해 줘야 한다.  
> **max_length** 는 데이터베이스 레벨과 Django 의 유효성 검사에서 적용 된다.  

## CommaSeparatedIntegerField

`class CommaSeparatedIntegerField(max_length=None, **options)`
> 1.9 version 이후에서는 사용하지 않는다.  
> 대신 **CharField** 를 사용한다.  
> 쉼표로 구분 된 정수 필드  
> **CharField** 와 마찬가지로 **max_length** 인수가 필요하다.  

## DateField

`class CommaSeparatedIntegerField(max_length=None, **options)`
> **Python** 에서 **datetime.date** 인스턴스로 표현되는 날짜이다.  
> 몇 가지 선택적 인수가 있다.  
 
* `DateField.auto_now`  
객체가 저장 될 때 마다 필드를 **지금**으로 자동 저장한다.  
"마지막으로 수정 된" 타임 스탬프에 유용하다.  
현재 날짜는 항상 사용된다. 재정의할 수 있는 기본값이 아니다.  

* `DateField.auto_now_add`  
객체가 처음 생성 될 때 필드를 **지금**으로 자동 설정한다.  
타임 스탬프 생성에 유용하다.  
현재 날짜는 항상 사용된다. 재정의할 수 있는 기본값이 아니다.  
따라서 객체를 만들 때 이 필드의 값을 설정하더라도 그 값은 무시된다.  
이 필드를 수정하려면 **auto_now_add=True** 대신 다음과 같이 설정해야 한다.  
	* For DateField: `default=date.today - from datetime.date.today()`
	* For DateTimeField: `default=timezone.now - from django.utils.timezone.now()`  

> 이 필드의 기본 양식 위젯은 **TextInput** 이다. 
> 관리자는 **JavaScript 캘린더** 와 **오늘** 에 대한 단축키를 추가한다.  
> 추가 **invalid_date** 오류 메시지 키가 포함된다.  

## DateTimeField

`class DateTimeField(auto_now=False, auto_now_add=False, **options)`
> **Python** 에서 **datetime.datetime** 인스턴스로 표현되는 **날짜** 와 **시간**  
> **DateField** 와 동일한 추가 인수를 사용한다.  
> 기본 양식 위젯은 **single TextInput** 이다.  
> 관리자는 **JavaScript shorcuts** 으로 두 개의 별도 **TextInput widgets** 를 사용한다.  

## DecimalField

`class DecimalField(max_digits=None, decimal_places=None, **options)`
> 고정 소수점 이하의 십진수로 **Python** 에서 **Decimal** 인스턴스로 나타낸다.  
> 두 가지 필수 인수가 있다.  

* `DecimalField.max_digits`  
	* 숫자에 허용되는 최대 자릿 수  
	* 이 수는 `decimal_places` 보다 크거나 같아야 한다.  
* `DecimalField.decimal_places`  
	* 숫자와 함께 저장 할 소수 자릿 수  
	* 최대 999 자리를 소수점 이하 2 자리로 저장하려면 다음과 같이 정의 해야 한다.  
	```
	models.DecimalField(..., max_digits=5, decimal_places=2)
	```
	
	* 소수점 이하 10자리의 해상도로 약 10억 개의 숫자 저장  
	```
	models.DecimalField(..., max_digits=19, decimal_places=10)
	```
* 이 필드의 기본 양식 위젯은 **localize** 가 **False** 일 때 **NumberInput** 이고, 다른 경우에는 **TextInput** 이다.

## DurationField

`class DurationField(**options)`
> 기간을 저장하는 필드 - **timedelta** 로 **Python** 에 모델링  
> **PostgreSQL** 에서 사용될 때, 사용되는 데이터 유형은 **interval** 이고, **Oracle** 에서 사용되는 데이터 유형은 **INTERVAL DAY(9) To SECOND(6)** 이다.  
> 그 외에는 마이크로 초의 **bigint** 가 사용된다.  

## EmailField

`class EmailField(max_length=254, **options)`  
> email 주소가 유아한 값인지 확인하는 **CharField**  
> **EmailValidator** 를 사용하여 입력값의 유효성을 검사한다.  

## FileField

`class FileField(upload_to=None, max_length=100, **options)`  
> 파일을 업로드 하는 field  
> **primary_key** 및 **unique** 인수는 지원되지 않으며, 사용되는 경우 **TypeError** 를 발생시킨다.  
> 2개의 선택적 인수를 가진다.  

* FileField.upload_to  
	* 이 속성은 업로드 디렉토리와 파일 이름을 설정하는 방법을 제공한다.  
	두 가지 방법으로 설정할 수 있다.  
	두 경우 모두 값은 **Storage.save()** 메서드에 전달된다.  
	* 문자열 값을 지정하면 **strftime()** 형식이 포함될 수 있다.  
	이 형식은 업로드 된 파일이 주어진 디렉토리를 채우지 않도록, 파일 업로드 날짜/시간 으로 대체된다.  
	```
	class Mymodel(models.Model):
		# file will be uploaded to MEDIA_ROOT/uploads  
		upload = models.FileField(upload_to='uploads/')  
		# or...  
		# file will be saved to MEDIA_ROOT/uploads/2015/01/30  
		upload = models.FileField(upload_to='uploads/%Y/%m/%d/')
	```  
	* 기본 **FileSystemStorage**를 사용하는 경우, 문자열 값은 **MEDIA_ROOT** 경로에 추가되어 업로드 된 파일이 저장 될 로컬 파일 시스템의 위치를 형성한다.  
	* 다른 저장소를 사용하는 경우, 해당 저장소의 설명서에서 업로드 처리 방법(**upload_to**)을 확인   
	* **upload_to** 는 함수와 같은호출 가능 함수 일 수 있다.  
	파일 이름을 포함하여 업로드 경로를 얻기 위해 호출한다.  
	이 호출 가능 객체는 두 개의 인수를 받아 들여 storage system에 전달되는 Unix-style 경로를 반환해야 한다.(슬래시 포함)  
	두 가지 인수는 다음과 같다.  
	
| Arguments | Description |
|:----------|:-----------:|  
|instance   |**FileField** 가 정의 된 모델의 인스턴스이다. 즉, 현재 파일이 첨부되는 특정 인스턴스. 대부분의 경우 이 객체는 아직 데이터베이스에 저장되지 않았으므로, 기본 **AutoField**를 사용하는 경우, 기본 키 필드의 값이 없을 수 있다. |
|filename   |원래 파일에 주어진 파일 이름. 최종 목적지 경로를 결정할 때 고려 될 수 있고 아닐 수 도 있다.|
For Example:  

```
def user_directory_path(instance, filename):
    # file will be uploaded to MEDIA_ROOT/user_<id>/<filename>
    return 'user_{0}/{1}'.format(instance.user.id, filename)

class MyModel(models.Model):
    upload = models.FileField(upload_to=user_directory_path)
```

* FileField.storage  
	* 파일의 저장 및 검색을 처리하는 저장 객체이다.  
	* 이 필드에 대한 기본 양식 위젯은**ClearableFileInput** 이다.  
	* 모델에서 **FileField** 혹은 **ImageField** 를 사용하면 몇 단계를 수행할 수 있다.
		1. 설정 파일에서 Django 가 업로드 된 파일을 저장할 디렉토리의 전체 경로로 **MEDIA_ROOT** 을 정의해야 한다. (성능 향상을 위해 이 파일들은 데이터베이스에 저장되지 않는다.) **MEDIA_URL** 을 해당 디렉토리의 기본 URL로 정의하고, 이 디렉토리에 웹 서버의 사용자 계정이 쓸 수 있는지 확인.
		2. 모델에 **FileField** 또는 **ImageField**를 추가하고, 옵션을 정의하여 업로드 된 파일에 사용할 **MEDIA_ROOT**의 하위 디렉토리를 지정한다.
		3. 데이터베이스에 저장되는 것은 모두 파일에 대한 경로이다. (**MEDIA_ROOT**에 상대적임) Django 가 제공하는 편리한 url 속성을 사용하고 싶을 것이다. 예를 들어 **ImageField** 의 이름이 **mug_shot**일 경우, **{{object.mug_shot.url}}** 템플릿을 사용하여 이미지의 절대 경로를 가져올 수 있다. 
	
	* 
    




