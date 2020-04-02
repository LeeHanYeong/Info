

### Serializer의 create, update

객체를 다룰 경우, create와 update를 작성해서 save()를 이용

```python
serializer = SampleSerializer(data=data)
serializer.save() # create

serializer = SampleSerializer(instance=instance, data=data)
serializer.save() # update
```

객체를 다루지 않아도, 값을 validate하는데 사용

```python
class LoginSerializer(serializers.Serializer):
    username = serializers.CharField(required=False, allow_blank=True)
    email = serializers.EmailField(required=False, allow_blank=True)
    password = serializers.CharField(style={'input_type': 'password'})
    
    def validate(self, attrs):
        username = attrs.get('username')
        email = attrs.get('email')
        password = attrs.get('password')
```



### Serializer가 다루어야 하는 로직

- validation
- 해당하는 객체의 create/update

```python
def signup(request):
    serializer = SignupSerializer(data=request.data)
    username = serializer.validated_data['username']
    password = serializer.validated_data['password']
    User.objects.create(username=username, ...)
    
    
def signup(request):
    serializer = SignupSerializer(request.data)
    if serializer.is_valid():
        user = serializer.save()
        return Response(serializer.data)
		return Response(serializer.errors, status=400)
```



### blank=True, null=True

blank는 빈 문자열

- CharField기반에서만 가능

null은 DB의 null(없음)

- JavaScript의 null
- Python의 None
- DB의 null

