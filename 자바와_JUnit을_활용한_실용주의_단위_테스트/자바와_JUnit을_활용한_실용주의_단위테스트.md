## 자바와 JUnit을 활용한 실용주의 단위 테스트

<img src="https://github.com/jayyhkwon/review/blob/master/%E1%84%8C%E1%85%A1%E1%84%87%E1%85%A1%E1%84%8B%E1%85%AA_JUnit%E1%84%8B%E1%85%B3%E1%86%AF_%E1%84%92%E1%85%AA%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB_%E1%84%89%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%8C%E1%85%AE%E1%84%8B%E1%85%B4_%E1%84%83%E1%85%A1%E1%86%AB%E1%84%8B%E1%85%B1_%E1%84%90%E1%85%A6%E1%84%89%E1%85%B3%E1%84%90%E1%85%B3/%ED%91%9C%EC%A7%80.jpeg">

### 5장 - 좋은 테스트의 FIRST 속성

-  Fast

  - 영속성과 관련있는 코드 중에 작은 단위의 코드는 하드코딩하여 테스트함
    - 속도측면에서 이점이 있음

-  Isolated, Repeatable

  - 각 테스트는 독립적이여야 한다.
  - 각 테스트는 단일한 목적을 가져야 한다 (SRP)
  - 반복가능한 테스트는 실행할 때마다 결과가 같아야한다.
    - mock 객체 사용
  - 시간과 관련된 테스트는?
    - java.time.Clock 객체를 사용하여 고정된 시간을 반환할 수 있다.

-  Self-validating, Timely

  - main() 메서드로 테스트하면 복잡하여 가독성이 떨어져 시간 소모적이다.

  - junit 사용하면 기대하는 것이 무엇인지 단언하여 결과를 성공 or 실패로 확인 가능하다.

    - CI,CD를 위한 자동화

    - 단위 테스트로 코드 검증을 미룰수록 결함이 늘어날 수 있다. 
      또한, 소스 저장소에 푸시한 뒤라면 그 것을 되돌려 테스트를 작성하기는 더욱 힘들어진다.

  - 옛날 코드에 큰 결함이 없고 당장 변경할 예정이 없다면 테스트 케이스 작성은 시간낭비가 될 수 있다.

    

### 6장 - Right-BICEP

- **Right - 결과가 올바른가?**

  - **B - 경계 조건(boundary conditions)은 맞는가?**

    - 모서리 사례를 테스트로 처리해야 한다.

      - 1. 모호하고 일관성 없는 입력값.
           예를 들어 특수문자가 포함된 파일 이름
        2. 잘못된 양식의 데이터
           예를 들어 최상위 도메인이 빠진 이메일 주소(fred@google)
        3. 수치적으로 오버플로를 일으키는 계산
        4. 비거나 빠진 값.
           예를 들어 0,"",null
        5. 이성적인 기댓값을 훨씬 벗어나는 값.
           에를 들어 150세의 나이
        6. 교실의 당번표처럼 중복을 허용해서는 안 되는 목록에 중복 값이 있는 경우
        7. 정렬이 안 된 정렬 리스트 혹은 그 반대. 정렬 알고리즘에 이미 정렬된 입력 값을 넣는 경우
           또는 정렬 알고리즘에 역순 데이터를 넣는 경우
        8. 시간 순이 맞지 않는 경우. 예를 들어 HTTP 서버가 OPTIONS 메서드의 결과를
           POST 메서드보다 먼저 반환해야 하지만 그 후에 반환하는 경우

    - 클라이언트가 팀원이 아닌 이상 보호절을 통하여 처리하는 것이 맞다.

    - CORRECT를 기억하라

      - Conformance(준수) - 값이 기대한 양식을 준수하고 있는가?
      - Ordering(순서) - 값의 집합이 적절하게 정렬되거나 정렬되지 않았나?
      - Range(범위) - 이성적인 최솟값과 최댓값 안에 있는가?
      - Reference(참조) - 코드 자체에서 통제할 수 없는 어떤 외부 참조를 포함하고 있는가?
      - Existence(존재) - 값이 존재하는가(non-null, non-zero ...)
      - Cardinality(기수) - 정확히 충분한 값들이 있는가?

      - Time(절대적 혹은 상대적 시간) - 모든 것이 순서대로 일어나는가? 정확한 시간에?

    

- **I - 역 관계(inverse relationship)를 검사할 수 있는가?**

  - 예를 들어 Profile.find() 메서드를 검증하려고 할때

    Predicate return 값이 true 인것과 false인 것 둘다 합쳐서 검증(교차검사)

  - 교차검사는 모든 요소를 더하고 균형이 맞는지 확인하는 방법으로 

    복식 부기에서 총 계정 원장을 맞추는 것과 같다.

    

- **C - 다른 수단을 활용하여 교차 검사(cross-check)할 수 있는가?**

  - 성능 또는 냄새가 좋지 않더라도 믿을 수 있고 참 값을 보장한다면 1등 해법을 교차 검사할 때 활용 할 수 있다.

    

- **E - 오류 조건(error conditions)을 강제로 일어나게 할 수 있는가?**

  - 오류의 종류 혹은 다른 환경적인 제약 사항 종류

    - 1. 메모리가 가득 찰 때
      2. 디스크 공간이 가득 찰 때
      3. 벽시계 시간에 대한 문제들
         - 서버와 클라이언트 간 시간이 달라서 발생하는 문제들
      4. 네트워크 가용성 및 오류들

      5. 시스템 로드
      6. 제한된 색상 팔레트
      7. 매우 높거나 낮은 비디오 해상도

- **P - 성능 조건(performance characteristics)은 기준에 부합하는가?**

  - 실제 병목이 어디인지 증명되기 전까지는 미리 짐작하여 코드를 난도질하지 마라 - 롭 파이크
  - 성능 측정시 충분한 갯수를 반복해야 결과 수치가 튀지 않는다
  - 반복하는 코드 부분을 자바(JVM)가 최적화하지 못하는지 확인해야 한다.

    - 예를 들어 람다 기반의 코드가 최적화 되지 않았다고 가정하면 성능이 향상되는 지 
      확인하기 위해 전통적인 코드로 교체하고 싶을 수 있다.
      최적화를 하기 전에 먼저 기존 코드를 기준으로 경과시간을 측정하는 테스트 후에
      코드 변경 후 결과를 비교해봐라.(**몇 번 실행해 보고 평균을 계산하라**)
    - 최적화되지 않은 테스트는 한 번에 수 밀리초가 걸리는 일반적인 테스트코드들보다 매우 느리다.
      느린 테스트들은 빠른 것과 분리해라.
  - 성능 측정 도구 : JMeter, JUnitPerf



###  8장. 깔끔한 코드로 리팩토링 하기

- 메서드 추출(169p)
  - **코드를 안전하게 옮길 수 있는 능력은 단위 테스트의 가장 중요한 이점**
  - 새로운 기능을 안전하게 추가할 수 있고 좋은 설계를 유지하면서 변경할 수 있다.
- 메서드를 위한 더 좋은 집 찾기(170p)
  - 리팩토링한 Profile.matches(criterion,answer) 메서드는
    Profile 객체와 연관이 없다.
    Criterion 객체는 Answer 객체를 알고 있으므로 Criterion 객체로 해당 메서드를 이동한다.
  - Answer answer = answers.get(criterion.getAnswer().getQuestionText()); 구문은
    디메테르의 법칙(다른 객체로 전파되는 연쇄적인 메서드 호출을 피해야함)을 위반하고 깔끔하지도 않으므로 별도 메서드로 추출한다.
- 과한 리팩토링? (176p)
  - matches() 메서드를 3단계로 나눠서 가독성은 증가하였으나 각 부분마다 for문이 존재함
    - **성능이 당장 문제되지 않는다면 코드를 깔끔하게 유지해라**
      깔끔한 설계는 코드를 이동시킬 수 있는 유연성을 제공하고 
      다른 알고리즘을 적용하는 데도 수월하다.



### 9장. 더 큰 설계 문제

- Profile 객체에서 MatchSet 객체를 분리해내는 과정을 예시로 설명하고 있음

- 클래스 쪼개기(SRP) - **186p**

  - 기존 Profile 클래스는 크게 2가지 책임이 있다.

    1)  Profile에 관한 정보 추적하기

    2) 조건 집합이 Profile에 매칭되는지 혹은 그 정도를 판단하기

    이는 SRP에 어긋나므로 2개의 클래스로 쪼갠다

- 명령-질의 분리 원칙 - **191p**

  - 특정 메서드는 명령을 실행하거나 질의에 응답할 수 있으며, 두 작업 모두를 해서는 안된다

  - Profile.matches(criteria); 메서드는 조건 집합과 매칭하는 메서드인데

    score 변수의 상태값을 변경하려 한다.

    점수(score)를 원한다면 matches() 메서드를 호출해야 한다는 것을 사전에 알아야하는 것은 직관에 어긋난다. (메서드명에서 유추 불가능)

- 단위테스트의 유지보수 비용 - **194p**

  - 작은 코드 조각들을 단일 메서드로 추출하면 그 코드 조각들을 변경해야 할 때 
    미치는 영향을 최소화할 수 있다.
  - private 메서드를 테스트하려는 충동은 클래스가 필요 이상으로 커졌다는 또 다른 힌트.
  - 단위테스트가 어려워 보인다면 그것도 좋은 힌트.

- 다른 설계에 관한 생각들 - **198p**

  - 불필요한 필드 값 제거
    - MatchSet 객체의 score 필드는 getScore() 메서드 호출시 지역변수로 선언하여 
      필요할 때만 사용하도록 한다.
  - 하나의 answers 맵 객체가 Profile,MatchSet 클래스에 동시에 존재한다.
    - 기능의 산재 (shotgun surgery)
      - 서로에 대한 정보를 너무 많이 가지고 있다
    - 데이터 상태에 혼란이 올 수 있음
      - **Profile 객체 내에서 Answer와 관련된 메서드들을 위해**
        **AnswerCollection으로 클래스를 분리하는 것은 이해되나** 
        **AnswerCollection으로 분리한다고 해서 데이터 상태와 상관이 있나..?**
        **어차피 Profile 객체와 MatchSet은 같은 AnswerCollection을 공유하는데..**

- #### 10장. 목 객체 사용

  - 의존성이 강한 코드들을 테스트하기 위한 도구
  
  - 스텁 -> 목 객체 흐름으로 설명

  - **테스트 대상 : AddressRetriever.retrieve() 메서드의 로직을 검증.**

  -  AddressRetriever 클래스

  ```java
  package iloveyouboss;
  
  import java.io.*;
  import org.json.simple.*;
  import org.json.simple.parser.*;
  import util.*;
  
  public class AddressRetriever {
     public Address retrieve(double latitude, double longitude)
           throws IOException, ParseException {
        String parms = String.format("lat=%.6flon=%.6f", latitude, longitude);
       
        // retrieve() 메서드는 HttpImpl 클래스에 의존하고 있다.
        String response = new HttpImpl().get(
          "http://open.mapquestapi.com/nominatim/v1/reverse?format=json&"
          + parms);
  
        JSONObject obj = (JSONObject)new JSONParser().parse(response);
  
        JSONObject address = (JSONObject)obj.get("address");
        String country = (String)address.get("country_code");
        if (!country.equals("us"))
           throw new UnsupportedOperationException(
              "cannot support non-US addresses at this time");
  
        String houseNumber = (String)address.get("house_number");
        String road = (String)address.get("road");
        String city = (String)address.get("city");
        String state = (String)address.get("state");
        String zip = (String)address.get("postcode");
        return new Address(houseNumber, road, city, state, zip);
     }
  }
  
  ```

  

  - HttpImpl 클래스는 HTTP 요청을 처리하는 클래스이므로

    - 1. 일반 로직 테스트에 비해 느리다.
    - 2. API 서버에 의존적이므로 IOException이 발생할 수 있다.

    -  이러한 이유로 프로덕트 코드를 사용하기 보다는 **스텁(stub)**을 사용 한다.

      

  - 스텁이란?

    테스트 용도로 하드 코딩한 값을 반환하는 구현체를 말한다.

    

  - **테스트 코드**

- ```java
     @Test
     public void answersAppropriateAddressForValidCoordinates() 
           throws IOException, ParseException {
        
        // 스텁
        Http http = (String url) ->
           "{\"address\":{"
           + "\"house_number\":\"324\","
           + "\"road\":\"North Tejon Street\","
           + "\"city\":\"Colorado Springs\","
           + "\"state\":\"Colorado\","
           + "\"postcode\":\"80903\","
           + "\"country_code\":\"us\"}"
           + "}";
       
        // 파라미터 검증을 로직을 추가한 스텁
        Http httpWithParameterVerification = (String url) -> 
        {
          if (!url.contains("lat=38.000000&lon=-104.000000"))
            fail("url " + url + " does not contain correct parms");
          return "{\"address\":{"
             + "\"house_number\":\"324\","
             + "\"road\":\"North Tejon Street\","
             + "\"city\":\"Colorado Springs\","
             + "\"state\":\"Colorado\","
             + "\"postcode\":\"80903\","
             + "\"country_code\":\"us\"}"
             + "}";
        };
  
        // 생성자를 이용한 의존성 주입(DI)
        // 세터 메서드, 팩토리 메서드, 추상 팩토리도 사용가능 하다.
        AddressRetriever retriever = new AddressRetriever(http);
  
        Address address = retriever.retrieve(38.0,-104.0);
        
        assertThat(address.houseNumber, equalTo("324"));
        assertThat(address.road, equalTo("North Tejon Street"));
        assertThat(address.city, equalTo("Colorado Springs"));
        assertThat(address.state, equalTo("Colorado"));
        assertThat(address.zip, equalTo("80903"));
     }
  
  ```



- **AddressRetriever 클래스**

```java
package iloveyouboss;

import java.io.*;
import org.json.simple.*;
import org.json.simple.parser.*;
import util.*;

public class AddressRetriever {
  
  // 필드 선언
  private Http http;

  // 생성자 추가
   public AddressRetriever(Http http) {
      this.http = http;
   }

   public Address retrieve(double latitude, double longitude)
         throws IOException, ParseException {
      String parms = String.format("lat=%.6flon=%.6f", latitude, longitude);
      String response = http.get(
         "http://open.mapquestapi.com/nominatim/v1/reverse?format=json&"
         + parms);

      JSONObject obj = (JSONObject)new JSONParser().parse(response);
      // ...

      JSONObject address = (JSONObject)obj.get("address");
      String country = (String)address.get("country_code");
      if (!country.equals("us"))
         throw new UnsupportedOperationException(
               "cannot support non-US addresses at this time");

      String houseNumber = (String)address.get("house_number");
      String road = (String)address.get("road");
      String city = (String)address.get("city");
      String state = (String)address.get("state");
      String zip = (String)address.get("postcode");
      return new Address(houseNumber, road, city, state, zip);
   }
}
```



- 스텁을 이용하여 테스트를 작성하는 것도 방법이지만 
  좀 더 편하고 빠르게 테스트를 작성할 수 있도록 mockito 프레임워크의 mock 을 사용해보자.

  ```java
  @Test
  public void answersAppropriateAddressForValidCoordinates() 
        throws IOException, ParseException {
     
     // Http 타입을 구현하는 mock 을 생성한다.
     Http http = mock(Http.class);
     // stubbing
     when(http.get(contains("lat=38.000000&lon=-104.000000"))).thenReturn(
           "{\"address\":{"
           + "\"house_number\":\"324\","
          // ...
           + "\"road\":\"North Tejon Street\","
           + "\"city\":\"Colorado Springs\","
           + "\"state\":\"Colorado\","
           + "\"postcode\":\"80903\","
           + "\"country_code\":\"us\"}"
           + "}");
  
     AddressRetriever retriever = new AddressRetriever(http);
  
     Address address = retriever.retrieve(38.0,-104.0);
     
     assertThat(address.houseNumber, equalTo("324"));
     // ...
     assertThat(address.road, equalTo("North Tejon Street"));
     assertThat(address.city, equalTo("Colorado Springs"));
     assertThat(address.state, equalTo("Colorado"));
     assertThat(address.zip, equalTo("80903"));
  }
  ```



- mockito프레임워크를 이용하여 stubbing한 코드와 
  프레임워크를 사용하지 않은 stub의 코드를 비교해보자.

  

  ```java
        // stubbing(mockito 사용)
        when(http.get(contains("lat=38.000000&lon=-104.000000"))).thenReturn(
           "{\"address\":{"
           + "\"house_number\":\"324\","
          // ...
           + "\"road\":\"North Tejon Street\","
           + "\"city\":\"Colorado Springs\","
           + "\"state\":\"Colorado\","
           + "\"postcode\":\"80903\","
           + "\"country_code\":\"us\"}"
           + "}");
  
       // 파라미터 검증을 로직을 추가한 스텁(mockito 사용x)
        Http httpWithParameterVerification = (String url) -> 
        {
          if (!url.contains("lat=38.000000&lon=-104.000000"))
            fail("url " + url + " does not contain correct parms");
          return "{\"address\":{"
             + "\"house_number\":\"324\","
             + "\"road\":\"North Tejon Street\","
             + "\"city\":\"Colorado Springs\","
             + "\"state\":\"Colorado\","
             + "\"postcode\":\"80903\","
             + "\"country_code\":\"us\"}"
             + "}";
        };
  
  ```



- 나의 경우 mockito 프레임워크로 코드를 작성한 것이 더 나아보이는 점을 딱히 찾지 못했다

  어노테이션을 사용하기 전까지는.



```java
public class AddressRetrieverTest {

   @Mock
   private Http http; // Http http = mock(Http.class);와 동일한 코드

   // 목 객체를 주입하는 과정
   // 1. 생성자를 탐색
   // 2. 세터 메서드를 탐색
   // 3. 적절한 필드를 탐색(필드 타입과 매칭되는 것)
   // 기존 AddressRetriever 클래스의 생성자로 Http 객체를 넘겨주는 코드를 삭제하더라도
   // AddressRetriever 클래스에 Http 타입을 필드로 선언하기만 한다면
   // 3. 적절한 필드를 탐색하는 과정으로 mock 객체를 주입 할 수 있고
   // 프로덕션 코드와 mock을 이용하는 테스트 코드를 분리할 수 있다.
   @InjectMocks
   private AddressRetriever retriever; // mock을 주입할 대상
   
   @Before
   public void createRetriever() {
      retriever = new AddressRetriever();
      MockitoAnnotations.initMocks(this);
   }

   @Test
   public void answersAppropriateAddressForValidCoordinates() 
         throws IOException, ParseException {
      when(http.get(contains("lat=38.000000&lon=-104.000000")))
         .thenReturn("{\"address\":{"
                        + "\"house_number\":\"324\","
         // ...
                        + "\"road\":\"North Tejon Street\","
                        + "\"city\":\"Colorado Springs\","
                        + "\"state\":\"Colorado\","
                        + "\"postcode\":\"80903\","
                        + "\"country_code\":\"us\"}"
                        + "}");

      Address address = retriever.retrieve(38.0,-104.0);
      
      assertThat(address.houseNumber, equalTo("324"));
      assertThat(address.road, equalTo("North Tejon Street"));
      assertThat(address.city, equalTo("Colorado Springs"));
      assertThat(address.state, equalTo("Colorado"));
      assertThat(address.zip, equalTo("80903"));
   }
}
```



```java
package iloveyouboss;

import java.io.*;
import org.json.simple.*;
import org.json.simple.parser.*;
import util.*;

public class AddressRetriever {
  
   // 생성자 제거하고 필드로 선언한다.
   private Http http = new HttpImpl();

   public Address retrieve(double latitude, double longitude)
         throws IOException, ParseException {
      String parms = String.format("lat=%.6f&lon=%.6f", latitude, longitude);
      String response = http.get(
         "http://open.mapquestapi.com/nominatim/v1/reverse?format=json&"
         + parms);

      JSONObject obj = (JSONObject)new JSONParser().parse(response);
      // ...

      JSONObject address = (JSONObject)obj.get("address");
      String country = (String)address.get("country_code");
      if (!country.equals("us"))
         throw new UnsupportedOperationException(
               "cannot support non-US addresses at this time");

      String houseNumber = (String)address.get("house_number");
      String road = (String)address.get("road");
      String city = (String)address.get("city");
      String state = (String)address.get("state");
      String zip = (String)address.get("postcode");
      return new Address(houseNumber, road, city, state, zip);
   }
}
```



- 목이 프로덕션 코드의 동작을 실행하는 지 확인 하는 방법

  - AddressRetriever 클래스의 프로덕션 코드는 HttpImpl.get() 메서드를 호출한다.

    1. 실제 통신이 이루어질 것이므로 실행시간을 측정하여 비교해보거나

    2.  HttpImpl.get() 메서드에 예외를 발생시켰을 때 테스트 코드에서 예외가 보인다면 
        프로덕션 코드가 동작하고 있는 것임을 알 수 있다.



- ####12장. 테스트 주도 개발 (TDD)

  - TDD vs 단위 테스트

    - 단위 테스트는 구현한 코드를 테스트 하는 것

    - TDD는 단위 테스트를 먼저 작성하며 테스트가 개발을 이끌어 가는 방식 

      

  - 테스트의 장점

    - 코드가 예상한 대로 동작한다는 자신감을 얻는 것

    

  - TDD 사이클

    - 1. 실패하는 코드 작성 (Red)
      2. 테스트 통과시키기 (Green)
      3. 코드 개선하기 (Refactoring)