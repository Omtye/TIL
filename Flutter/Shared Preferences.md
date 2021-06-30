## 개요

Flutter에서 상대적으로 적은 양의 key-value 값의 데이터를 저장하려고 할 때 shared_preferences 를 사용할 수 있다. 또한 shared preferences 플러그인의 경우 iOS의 NSUserDefaults와 Android의 Shared Preferences를 포함하고 있어 iOS, Android 동시 사용이 가능하다.



### Shared preferences 패키지 설치

```dart
flutter pub add shared_preferences
```



### 데이터 저장하기

SharedPreferences 클래스의 `setter` 메소드를 사용하여 데이터를 저장합니다. 

다만, 대용량 데이터 저장이 불가하고 `int`, `double`, `bool`, `string`, `stringList` 타입만 사용이 가능합니다.

```dart
// shared preferences 얻기
final prefs = await SharedPreferences.getInstance();

// 값 저장하기
prefs.setInt('counter', counter);
```



### 데이터 읽기

SharedPreferences 클래스의 `getter` 메소드를 사용하여 데이터를 읽을 수 있습니다.

```dart
final prefs = await SharedPreferences.getInstance();

// counter 키에 해당하는 데이터 읽기를 시도합니다. 만약 존재하지 않는 다면 0을 반환합니다.
final counter = prefs.getInt('counter') ?? 0;
```



### 데이터 삭제하기

```
final prefs = await SharedPreferences.getInstance();

prefs.remove('counter');
```



### 정리

Flutter 에서 간단한 사용자 데이터를 저장할 때는 Shared preferences는 정말 편리하나 대용량 데이터를 저장하기에는 좋지 않음. 사용자 설정 옵션 등을 저장할 때 정말 간편할 것이라 생각됨