# OOP(객체지향 프로그래밍)

## C#의 객체 지향 프로그래밍 네 가지 주요 방법
1. 추상화: 형식 소비자의 불필요한 세부 정보를 숨기는 것
2. 캡슐화: 서로 관련된 속성, 메서드 및 기타 멤버의 그룹을 하나의 단위나 개체로 취급하는 것
3. 상속: 기존 클래스를 기반으로 새로운 클래스를 만들 수 있는 능력
4. 다형성: 동일한 속성 또는 메서드를 각각 다른 방식으로 구현하는 여러 클래스를 서로 교체하여 사용할 수 있음

### 추상화

    using System;

    namespace classes
    {
        public class BankAccount
        {
            public string Number { get; }
            public string Owner { get; set; }
            public decimal Balance { get; }

            public void MakeDeposit(decimal amount, DateTime date, string note)
            {
            }

            public void MakeWithdrawal(decimal amount, DateTime date, string note)
            {
            }
        }
    }

### 캡슐화

객체의 속성(data fields)과 행위(methods)를 하나의 클래스라는 캡슐에 묶음

**접근 지정자**
1. private
2. protected
3. public

### 상속

    public class InterestEarningAccount : BankAccount
    {
    }

    public class LineOfCreditAccount : BankAccount
    {
    }

    public class GiftCardAccount : BankAccount
    {
    }

### 다형성

오버라이딩(Overriding)
자식 객체에서 재정의하는 것

오버로딩(Overloading)
하나의 클래스에서 같은 이름의 메소드들을 여러 개 가지고 있는 것

### 객체지향 5원칙

**SOLID**

1. SRP (Single responsibility principle) 단일 책임 원칙
2. OCP (Open-closed principle) 개방 폐쇄 원칙
3. LSP (Liskov substitution principle) 리스코브 치환 원칙
4. ISP (Interface segregation principle) 인터페이스 분리 원칙
5. DIP (Dependency inversion principle) 의존 역전 원칙

### 출처

[MSDN](https://docs.microsoft.com/ko-kr/dotnet/csharp/tutorials/intro-to-csharp/object-oriented-programming)  
[객체지향 5원칙](https://okayoon.tistory.com/entry/객체지향프로그래밍이란-3요소-5원칙-그리고-추상화)
