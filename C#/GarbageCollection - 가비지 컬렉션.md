# Garbage Collection - 가비지 컬렉션

**Garbage Collection**    
Heap 영역에서 사용하지 않는 객체를 삭제하는 프로세스   
Heap 영역 (Object 타입의 데이터들 - String, List 등)

GC의 수거 대상: Reachability   
어떤 객체에 유효한 참조가 존재한다면 Reachable, 그렇지 않다면 Unreachable이라고 한다.

GC Roots -> Reachable Objects
        -/> Unreachable Objects

