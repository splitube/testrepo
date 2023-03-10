## Item 71 : 필요 없는 검사 예외 사용은 피하라
### Conclusion
의미없는 checked exception은 자제하라.  
체크 예외를 사용하는 메서드는 스트림에서 사용할수 없다.  
예외시 클라이언트 프로그래머가 조치 가능하다면 체크 예외를 사용하라.  

### In case
클라이언트가 반드시 잡아서 처리해야하는 예외를 대응하기 위해서는 체크 예외를 사용해야한다.  
하지만 체크 예외를 남발하면 사용성이 떨어지는 코드를 작성하게 된다.

체크 예외는 반드시 처리를 하는 코드를 작성해야하기 때문에 스트림 안에서 '직접' 사용될수 없다.

API를 제대로 사용해도 발생할수 있는 예외이거나 클라이언트가 의미있는 조치를 할수 있는 상황이 아니라면 언체크 예외를 사용하라.
- API를 제대로 사용해도 발생할수 있는 예외
    - 해당 리소스 점유 불가, 외부 시스템 연동 실패 따위
- 클라이언트가 의미있는 조치를 할수 있는 상황
    - 외부 시스템 연동 실패시 n초후 재시도, 혹은 fallback 실행

위와 같은 상황이 아니면 그냥 언체크 예외로 대체할것

체크 예외를 언체크 예외로 바꾸어 사용하는 예외 번역이나, Optional을 리턴하도록 바꾸자.

혹은, 체크 예외를 발생 시키기 전에 검사하는 방식으로 우회할수 있도록 디자인하라.
- 이 경우 검사 메소드는 거의 같은 행동을 하기 때문에 시간 퍼포먼스는 나빠질수 있다.
- 멀티 스레드 환경에서는 무의미 할수 있다.

## in my opinion

체크 예외는 OCP와 LSP를 위반하기 쉽다.
- 예외를 던지는 메소드가 수정되면 모든 클라이언트가 수정되어야함
- 새로운 예외를 추가하면 상위에서는 새로운 catch문을 추가해야함
- 모듈끼리의 결합도가 상승함 
- 새로운 기능을 구현시 기존코드가 깨지지 않게 한다는 OCP에 반함
- 상속된 자식의 메소드는 예외 타입도 하위 유형이거나 같은 유형이여야 하기 때문에 LSP를 지키기 어려움


