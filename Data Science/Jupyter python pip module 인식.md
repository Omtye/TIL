## Jupyter python pip module 인식



### Jupyter와 local의 Python 인식 경로 확인



Jupyter 실행

```python
import sys
print(sys.executable)
```



cmd 실행

```bash
>>python
>>import sys
>>print(sys.executable)
```



두 명령어를 실행할 경우 Jupyter 와 local에서 python을 인식하는 경로가 다른 것을 확인할 수 있음



### jupyter의 python3 경로 local의 python3로 변경

```bash
python3 -m ipykernel install --user
```

명령어 실행 후 jupyter를 재실행하면 위의 두 명령어의 경로가 일치된 것을 확인할 수 있음



이후 정상적으로 pip module 설치하여 jupyter에서 import가 가능하다.