## addPostFrameCallBack

build가 끝나지 않은 시점에서 데이터의 변경이 발생하고 UI가 변경될 경우 에러 메시지 발생

<br>


════════ Exception caught by foundation library ════════════════════════════════════════════════════
The following assertion was thrown while dispatching notifications for TestInt:
setState() or markNeedsBuild() called during build.

<br>

이 경우 `addPostFrameCallBack` 을 사용하여 build가 끝나는 시점에 callback 함수를 호출 하도록 함

```dart
WidgetsBinding.instance!.addPostFrameCallback((_) {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => EditAlarm(
                      alarm: newAlarm,
                      manager: _manager,
                    ),
                  ),
                );
              });
```

