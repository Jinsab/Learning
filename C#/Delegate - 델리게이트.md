# Delegate(델리게이트)
### 대리자, 특정 매개 변수 목록 및 반환 형식이 있는 메서드에 대한 참조를 나타내는 형식
  
**대리자 선언**

    public delegate int Calculation(int x, int y);

**대리자 개요**

1. C++ 함수 포인터와 유사하지만, C++ 함수 포인터와 달리 멤버 함수에 대해 완전히 객체 지향
  -> 대리자는 개체 인스턴스 및 메소드를 모두 캡슐화함
2. 대리자를 통해 메서드를 매개 변수로 전달할 수 있음
3. 대리자를 사용하여 콜백 메서드를 정의할 수 있음
4. 여러 대리자를 연결할 수 있음
  -> 단일 이벤트에 대해 여러 메서드를 호출할 수 있음
5. [메서드와 대리자 형식이 정확히 일치할 필요는 없음](https://docs.microsoft.com/ko-kr/dotnet/csharp/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates)

**[사용 예시](https://mrw0119.tistory.com/19)**

    namespace Learn_Cshap
    {
        delegate int DelegateA(int x, int y);
        
        class Learn_Delegate
        {
            public static void Calculator(int x, int y, DelegateA dele)
            {
                Console.WriteLine(dele(x, y));
            }
            
            public static int plus(int x, int y) { return x + y };
            public static int minus(int x, int y) { return x - y };
            public static int multiply(int x, int y) { return x * y };
            
            static void Main(string[] args)
            {
                Dele Plus = new Dele(plus);
                Dele Minus = new Dele(minus);
                Dele Multiply = new Dele(multiply);
                
                Calculator(12, 23, Plus);
                Calculator(34, 23, Minus);
                Calculator(12, 23, Multiply);
            }
        }
    }
    
**일반화도 가능**

    delegate T DelegateB<T> (T x, T y);
    
    public static void Calculator<T>(T x, T y, DelegateB<T> dele)
    {
        Console.WriteLine(dele(x, y));
    }
    
**대리자 연결**

대리자 선언 이후 +=으로 추가해주면 됨

    delegate exam;
    exam = new delegate(function1);
    exam += function2;
    exam += function3;
    
반대로 -= 키워드로 델리게이트에 연결된 메소드를 끊어줄 수 있음
