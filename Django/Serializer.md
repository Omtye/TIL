Serializer



serializer.py

```python
from rest_framework import serializers
from .models import Job, UserInfo, YearOf

# 하위 모델 Serialize
class JobSerializer(serializers.ModelSerializer):
    class Meta:
        model = Job
        fields = ('id', 'job_name')

# 하위 모델 Serializer
class YearOfSerializer(serializers.ModelSerializer):
    class Meta:
        model = YearOf
        fields = ('id', 'year_of')


class UserInfoSerializer(serializers.ModelSerializer):
    job = JobSerializer(read_only=True) # N : 1 UserInfo가 1
    year_of = YearOfSerializer(many=True, read_only=True) # 1 : N


    class Meta:
        model = UserInfo
        fields =('user_id','eng_name','job','year_of','introduce_url','url','img_url')
```



views.py

```python
from rest_framework import serializers
from rest_framework.serializers import Serializer
from .serializers import UserInfoSerializer
from .models import UserInfo

@api_view(['GET'])
def userInfo(request, userid):
    
    if request.method == 'GET':
        userInfo = UserInfo.objects.filter(user_id=userid)
        serializer = UserInfoSerializer(userInfo, many=True)

        return Response(serializer.data)

```

