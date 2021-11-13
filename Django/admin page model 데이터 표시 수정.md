admin 에서 데이터를 추가할 경우 별도로 객체 명을 지정하지 않으면 class object(숫자) 로 표시됨

<br>

![image](https://user-images.githubusercontent.com/43038052/141606804-f944ae52-43fb-41bb-a6d5-5095eb548b36.png)

<br>
<br>
<br>

```python
#대륙 테이블
class Continent(models.Model):
    
    continent_name = models.CharField(max_length=100, null=False, verbose_name="대륙")
    reg_date = models.DateTimeField(auto_now_add=True)

    #데이터가 문자열로 변환될 경우 표시할 값 설정
    def __str__(self):
        return self.continent_name
    class Meta:
        db_table="continent"
```

<br>

![image](https://user-images.githubusercontent.com/43038052/141606922-e09acb00-26f2-4d15-a6f8-abcea9f76259.png)
