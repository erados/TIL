# 모두를 위한 파이썬 : 필수 문법 배우기

[모두를 위한 파이썬 : 필수 문법 배우기](https://www.inflearn.com/course/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A4%91%EA%B3%A0%EA%B8%89/dashboard)

## Variable Scope

### Scope

* 지역 변수에서 확인 후 전역 변수 확인
* 값을 수정하기 위해서는 global 키워드 사용 필요
  * 디버깅이 힘든데 오픈 소스에서 사용하는지 확인

```python
c = 10

def foo():
	c = c + 10
	# c = 10
	print(c) # local variable before assignment

```

```python
def outer():
    e = 10
    def inner()
	    nonlocal e # !important
	    e += 10
	    print(e)
	return inner

```

* Nonlocal 조금 더 알아보기
* 힙 스택 관련하여 클로저 더 알아보기

```python
locals()
globals()
locals()['foo'] = 10
print(foo) # 10
```

## Lambda, Reduce, Map, Filter

### Lambda

* 힙 영역 사용 즉시 소멸 <-> 일반함수는 재사용위해 메모리사용
* [파이썬 가비지 컬렉션(Count=0)](https://medium.com/dmsfordsm/garbage-collection-in-python-777916fd3189)[ㅁ](https://velog.io/@jollyn/Python-%EC%A4%91%EA%B8%89-1-1)
* list comprehension
* iterable 과 iterator 와 generator

```python
# 바이트 코드 분석
def also_square(nums):
	def double(x):
		return x ** 2
	return map(double, nums)
```

### Filter

```python
result = list(filter(lambda x : x %2 == 0 , digits))
```

### Reduce

```python
from functools import reduce
digits = list(range(1,101))
result = reduce(lambda x,y:x+y, digits)
```

map 과 list comprehension 성능 비교

## Shallow / Deep Copy

### copy

* call by value, reference, share

### shallow copy

```python
import copy

a = [1,2,3,[]]
b = copy.copy(a)
print(id(a)==id(b)) # False
print(id(a[3])==id(b[3])) # True

```

어떤 기능인지 더 정확한 이해/지식 필요

### deep copy

```python
import copy

a = [1,2,3,[]]
b = copy.deepcopy(a)
print(id(a)==id(b)) # False
print(id(a[3])==id(b[3])) # False
```

## Context Manager

* 정확하게 원하는 타이밍에 리소스를 할당 반환하는 역할

```python
import time

class ExcuteTimer(ojbect):
	def __init__(self, msf):
		self._msg = msg
	def __enter__(self):
		self._start = time.monotonic()
		return self._start
	def __exit__(self, exc_type, value, tace_back):
		if exc_type:
			print('Logging exception {}'.format((exc_type, value, trace_back)))
		else:
			print('{} : {} s'.format(self._msg, time.monotonic() - self._start))
		return True

with ExcuteTimer('Start! job') as v:
	print('Received start monotonic1 : {}'.format(v))
	for i in range(10**9):
		pass
	raise Exception('Raise! Exception!!')
		
```

### Context Manager Annotation

```python
import contextlib

@contextlib.contextmanger
def my_file_writer(file_name, method):
	f = open(file_name, method)
	yield f # __enter__
	f.close() # __exit__

with my_file_writer('test.txt', 'w') as f:
	f.write('some string...')
```

```python
import contextlib
import time

@contextlib.contextmanger
def ExcuteTimeDc(msg):
	start = time.monotonic()
	try:
		yield start
	exception BaseException as e:
		print('Logging Exception, {} : {}'.format(msg, e))
	else:
		print('{} : {}s'.format(msg, time.monotonic() - self._start))

with ExcuteTimerDc('Start! job') as v:
	print('Received start monotonic1 : {}'.format(v))
	for i in range(10**9):
		pass
	raise Exception('Raise! Exception!!')
		
```

\- annotation 과 decorator 차이

## Property

* 파이써닉하다
* 변수에 제약을 설정한다
* Getter, Setter 와 동등하다 (코드 일관성 )
  * 캡슐화-유효성 검사 기능 추가 용이
  * 대체 표현(속성 노출, 내부의 표현 숨기기 가능)
  * 속성의 수명 및 메모리 관리 용이
  * 디버깅 용이

### underscore

* 인터프리터
* 값 무시
* 네이밍(국제화, 자릿수)

```python
x, *_, y = (1, 2, 3, 4)
print(x, y)

for _ in range(3):
	print('Hi')
```

### access modifier

* name : public
* \_name : protected
* \_\_name : private (Naming Mangling)
  * class 내부에서는 접근이 되는 이유 C 단에서

```python
class SampleA:
	def __init__(self):
		self.x = 0
		self._y = 0
		self.__z = 0
	def test(self):
		print(self.__z)

a = SampleA()
a.x = 1 # 1
a._y = 2 # 2
a.__z = 3 # Exception

```

### getter / setter

```python
class SampleA:
	def __init__(self):
		self.x = 0
		self.__y = 0

	@property
	def y(self):
		return self.__y
	@y.setter
	def y(self, value):
		if value < 0:
			raise ValueError('0 보다 큰 값을 입력하세요.')
		self.__y = value
	@y.deleter
	def y(self):
		del self.__y

a = SampleA()

a.y
a.y = 1
delf a.y

```

@property 와 일반 클래스 변수 이름이 같다면?

## Method Overriding

## Method Overloading

## Meta Class

## Descriptor

## 패키지 생성
