# Mixin

Mixin은 클래스를 상속하지 않으면서 메소드를 사용할 수 있는 클래스이다.

코드의 재사용성을 높이고 다중상속으로 발생할 수 있는 죽음의 다이아몬드 문제를 해결할 수 있다.





### 사용법

믹스 인은 일반 클래스 선언을 통해 암시적으로 정의

```dart
class Walker {
  void walk() {
    print("I'm walking");
  }
}
```



Mixin이 인스턴스화되거나 확장되는 것을 방지하려면 다음과 같이 정의 

```dart
abstract class Walker {
  //클래스가 믹스 인으로만 사용되고 확장되는 것을 방지
  factory Walker._() => null;

  void walk() {
    print("I'm walking");
  }
}
```



Mixin을 사용할 때는 `with` 키워드를 사용하여 하나 이상의 믹스인 이름을 사용

```dart
class Cat extends Mammal with Walker {}

class Dove extends Bird with Walker, Flyer {}
```



여기서 cat 클래스는 Walker mix인을 포함하기 때문에 `walk() ` 메소드를 사용할 수 있지만 `fly()` 메소드는 사용할 수 없다.

```dart
main(List<String> arguments) {
  Cat cat = Cat();
  Dove dove = Dove();

  // 고양이는 걸을 수 있고
  cat.walk();

  // 비둘기는 걷고 날수도 있다.
  dove.walk();
  dove.fly();
 
}
```



### 

만약 2개 이상의 Mix인을 포함할 때 동일한 메소드가 존재할 경우에는 어떻게 처리 될까?

아래의 경우를 실행해 볼 경우 BA 값이 출력되게 된다.

동일한 메소드가 존재할 경우 마지막에 선언된 Mixin의 메소드를 호출하게 된다.

```dart
class A {
  String getMessage() => 'A';
}

class B {
  String getMessage() => 'B';
}

class P {
  String getMessage() => 'P';
}

class AB extends P with A, B {}

class BA extends P with B, A {}

void main() {
  String result = '';

  AB ab = AB();
  result += ab.getMessage();

  BA ba = BA();
  result += ba.getMessage();

  // BA 출력
  print(result);
}
```



Dart에서 Mixin은 다중상속을 의미하는 것이 아니라 일련의 작업 및 상태를 추상화하고 재사용한다. 

A -> B 순으로 오버라이딩 된다고 생각하면 이해하기 쉬울 것 같다.

이러한 선형화 구조로 다수의 Mixin을 포함하여도 다중 상속에서 발생하는 오류가 발생하지 않는다.

![Screenshot_5](https://user-images.githubusercontent.com/43038052/123670604-5b0a2a00-d878-11eb-8800-28675bb5223a.png)



