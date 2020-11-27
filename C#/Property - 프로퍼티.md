# 프로퍼티(Property)

## get, set를 조금 더 간결하게 작성 가능

**getter, setter 코드**

    private float exp;
    
    public float GetExp()
    {
        return exp;
    }
    
    public void SetExp(float value)
    {
        exp = value;
    }

**property 코드**

    private float exp;
    public float _exp { get => exp; set => exp = value; }
    
같은 역할을 하지만, 클래스의 변수가 많아질수록 Get, Set이 많아져 가독성이 떨어지고, 불편함이 있었다.   
프로퍼티의 경우에는 조금 더 간단하고 가독성이 보다 높다.    
또한 프로퍼티의 경우 set을 빼면 멤버변수를 읽기전용 형태로 변환도 가능하다.
