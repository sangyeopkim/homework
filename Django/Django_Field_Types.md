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



