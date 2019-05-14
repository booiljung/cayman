[Up](./index.md)

## 시작 (Getting start)

정수 42를 콘솔에 인쇄하는 짧은 코드를 작성해 보겠습니다.

```dart
printInteger(int aNumber) {
	print('The number is $aNumber.');
}

main() {
	var number = 42;
    printInteger(number);
}
```

### 주요 컨셉

- 변수에 넣을 수있는 모든 것은 객체이며 모든 객체는 클래스의 인스턴스입니다. 숫자, 함수 및 `null`은 객체입니다. 모든 객체는 `Object` 클래스에서 상속 받습니다.

-  다트가 강력하게 타입화 되어 있지만 다트는 타입을 유추 할 수 있으므로 타입 주석은 선택 사항입니다. 위의 코드에서 `number`는 `int` 유형으로 유추됩니다. 만일 타입이 없다고 명시적으로 말하면 특수한 타입인 `dynamic`을 사용합니다.
- 다트는 `List <int>` (정수 목록) 또는 `List <dynamic>` (동적 타입의 개체 목록)와 같은 제네릭 타입을 지원합니다.
- 다트는 최상위 함수 (예 : `main()`)와 클래스 또는 객체 (정적 메소드 및 인스턴스 메소드)에 연결된 함수를 지원합니다. 함수 (중첩 또는 로컬 함수) 내에서 함수를 만들 수도 있습니다.
- 마찬가지로 다트는 클래스 또는 객체 (정적 변수 및 인스턴스 변수)에 연결된 변수뿐만 아니라 최상위 변수도 지원합니다. 인스턴스 변수는 필드 또는 속성이라고도합니다.
-  Java와 달리 다트에는 public, protected 및 private 키워드가 없습니다. 식별자가 밑줄 (`_`)로 시작하면 해당 라이브러리에 대해 비공개입니다. 자세한 내용은 라이브러리 및 가시성을 참조합니다.
- 식별자는 문자 또는 밑줄 (_)로 시작하고 그 뒤에 문자와 숫자의 조합이 올 수 있습니다.
-  다트에는 실행 값이 있는 표현식과 실행되지 않는 명령문이 있습니다. 예를 들어, 조건식 `condition ? expr1 : expr2`는 `expr1` 또는 `expr2` 값을 가집니다. 이를 값이 없는 if-else 문과 비교하면 명령문에는 하나 이상의 표현식이 포함되는 경우가 많지만 표현식에 명령문을 직접 포함 할 수는 없습니다.
-  다트 도구는 경고 및 오류의 두 가지 종류의 문제를 보고 할 수 있습니다. 경고는 코드가 작동하지 않을 수도 있지만 프로그램 실행을 방해하지는 않습니다. 오류는 컴파일 타임 또는 런타임 중 하나 일 수 있습니다. 컴파일 타임 오류로 인해 코드가 전혀 실행되지 않습니다. 런타임 오류로 인해 코드가 실행되는 동안 예외가 발생합니다.

### 키워드 (Keywords)

예약된 키워드는 다음과 같습니다.

```dart
abstract 		dynamic	 		implements  	show
as				else		 	import			static
assert 			enum 			in 				super
async 	 		export  		interface  		switch
await  			extends 		is 				sync
break 			external  		library  		this
case 			factory  		mixin  			throw
catch 			false 			new 			true
class 			final 			null 			try
const 			finally 		on  			typedef 
continue 		for 			operator  		var
covariant 	 	Function 	 	part 		 	void
default 		get 		 	rethrow 		while
deferred 	 	hide 		 	return 			with
do 				if 				set 		 	yield
```


