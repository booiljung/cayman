# 디버깅 가능한 코드 (Debuggable code)

## 조건문

다음 `while`루프가 있다. 언제 루프를 빠져 나갈까?

```c#
while (true)
{
    int a, b, c, d;
    if (a == b && c != d) break;
}
```

`break`문에 브레이크 포인터를 걸 수 없어, 루프를 빠져나가는 시기를 루프를 돌며 조건절 안의 모든 변수를 반복 확인해야 할 것이다. 루프를 빠져나가면 변수 `a, b, c, d`의 스코프도 벗어나니 빠져나간 시점의 값도 확인할 수 없다.

다음 예제를 보자.

```c#
while (true)
{
    int a, b, c, d;
	if (a == b && c != d)
		break;
}
```

`엔터`를 한번 눌러주면 'break'문에 브레이크 포인트를 걸 수 있고, 브레이크가 걸리면 변수 값들을 확인 할 수 있다. **빠르게** 디버깅이 가능하다.

## 리턴값을 인자로 전달 하기

다음 `Dog`, `Cat`, `Ox` 클래스와 이를 생성 및 처리하는 함수들 그리고 `Foo()`함수가 있다.

```c#
class Dog;
class Cat;
class Ox;

Dog FindDog() { ... }
Dog WashDog(Dog dog) { ... }
Cat FindCat() { ... }
Cat FeedCat(Cat cat) { ... }
Ox FindOx() { ... }
Ox TrainOx(Ox ox) { ... }

void foo(Dog dog, Cat cat, Ox ox) {	... }
```

개, 고양이, 소를 검색한 후 `foo()`를 호출 한다.

```c#
void Main()
{
	foo(WashDog(FindDog()), FeedCat(FindCat()), TrainOx(FindOx()));
}
```

실행했더니 `NullReferenceException`이 발생하였다.  어느 곳에서 `NullReferenceException`이 발생 하였을까? 브레이크 포인트를 걸어도 알 수 없다.

```C#
void Main()
{
	Dog dog = FindDog();
	dog = WashDog(dog);
	Cat cat = FindCat();
	cat = FeedCat(cat);
	Ox ox = FindOx();
	ox = TrainOx(ox);
    Debug.Assert(dog);
    Debug.Assert(cat);
    Debug.Assert(ox);
	foo(dog, cat, ox);
}
```

브레이크 포인트를 걸고 로컬 변수를 확인하며 **빠르게** 디버깅이 가능하다.

길게 쓰면 컴파일러가 긴 코들 생성하여 느려지지 않느냐고? 그렇지 않다. 정상적인 컴파일러라면 컴파일러는 동일한 코드를 생성 한다.

모 반도체 회사의 레거시 소스 코드를 읽다가 생각나서 적어 본다. 디버그를 위해 코드를 변경하는 상황은 좋지 않다. 디버깅 가능하게 코딩 하자.

