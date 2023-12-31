> <h1>GPT의 기술면접</h1>

---

## <h2>Backend</h2>

---

### <h2>DB</h2>

> 동시에 같은 `DB Table row`를 업데이트하는 상황을 방어하기 위해 어떻게 개발하실건가요?

>> GPT의 답변

1. **트랜잭션**: 데이터베이스 트랜잭션을 사용하여 동시에 실행되는 업데이트 작업을 제어할 수 있습니다. 트랜잭션은 일련의 작업을 원자적으로 실행하여 데이터 일관성을 보장합니다. 동시에 같은 행을 업데이트하려는 경우, 첫 번째 트랜잭션이 해당 행을 잠금(lock)하여 다른 트랜잭션이 해당 행에 접근하는 것을 방지합니다.

2. **낙관적 락**: 낙관적 잠금은 업데이트 작업이 충돌할 가능성이 낮은 상황에서 사용됩니다. 이 방법은 각 행에 버전 번호나 타임스탬프를 추가하여 업데이트 시도 시 행이 변경되었는지 확인합니다. 업데이트 작업이 수행되기 전에 해당 행의 버전을 확인하고, 변경 사항이 없다면 업데이트를 진행합니다. 변경 사항이 있을 경우 충돌이 발생한 것으로 간주하고 예외를 발생시키거나 복구 작업을 수행합니다.

3. **비관적 락**: 비관적 잠금은 업데이트 작업 충돌 가능성이 높은 상황에서 사용됩니다. 이 방법은 트랜잭션 시작 시 해당 행을 명시적으로 잠그는 방식입니다. 다른 트랜잭션이 해당 행을 읽거나 업데이트할 때까지 잠금을 유지합니다. 이를 위해 데이터베이스에서 제공하는 락(lock) 메커니즘을 사용할 수 있습니다. 이러한 잠금은 다른 트랜잭션이 해당 행에 접근하지 못하도록 막으며, 순차적으로 업데이트 작업을 수행할 수 있게 해줍니다.

4. **버전 관리**: 업데이트할 행에 버전 번호나 타임스탬프와 같은 추가 필드를 사용하여 버전 관리를 구현할 수 있습니다. 동시에 업데이트를 시도하는 경우, 이러한 버전 정보를 비교하여 충돌 여부를 확인하고 적절한 조치를 취할 수 있습니다.

<br>

---

### <h2>Network</h2>

> `TCP`와 `UDP`의 차이를 설명해주세요.

>> GPT의 답변

1. **연결 지향성 vs. 비연결성**

  TCP: TCP는 연결 지향적인 프로토콜입니다. 통신 시작 전에 클라이언트와 서버 간에 연결을 설정하고, 데이터를 전송한 후에는 연결을 정리합니다. 데이터의 순서와 신뢰성을 보장하기 위해 시퀀스 번호와 확인 응답을 사용합니다.

  UDP: UDP는 비연결성 프로토콜입니다. 데이터를 전송하기 전에 연결 설정 단계가 없으며, 데이터 패킷을 독립적으로 전송합니다. 따라서 데이터의 순서가 보장되지 않고, 신뢰성 있는 전송도 보장하지 않습니다.

2. **신뢰성**

TCP: TCP는 신뢰성을 중요시하는 프로토콜입니다. 데이터 전송 시 확인 응답 및 재전송 메커니즘을 사용하여 데이터 손실, 손상, 중복을 방지합니다. 데이터의 순서를 보장하고, 수신자가 정상적으로 받았는지 확인할 수 있습니다.

UDP: UDP는 데이터 전송의 신뢰성을 보장하지 않습니다. 패킷이 손실되거나 손상되더라도 재전송을 수행하지 않습니다. 데이터의 순서가 보장되지 않을 수 있으며, 수신자가 데이터를 올바르게 받았는지 확인할 수 있는 기능이 없습니다.

3. **통신 방식**

TCP: TCP는 가상 회선 방식을 사용하여 데이터를 전송합니다. 이는 데이터가 안정적으로 전송되는 것을 보장하기 위해 연결 설정, 데이터 전송 및 연결 해제의 단계를 거치는 것을 의미합니다. 신뢰성과 순서를 중요시하며, 대부분의 웹 브라우저, 이메일, 파일 전송 등에 사용됩니다.

UDP: UDP는 데이터그램 방식을 사용하여 데이터를 전송합니다. 데이터 패킷을 독립적으로 전송하며, 추가적인 연결 설정 과정이 없습니다. TCP보다 간단하고 빠르지만, 신뢰성과 순서가 보장되지 않는 대신 지연 시간이 더 낮습니다. 동영상 스트리밍, DNS, 실시간 통신 등에 주로 사용됩니다.

4. **헤더 크기**

TCP: TCP 헤더는 크기가 크고, 연결 설정, 순서 관리, 오류 복구 등에 필요한 많은 정보를 포함합니다. 이로 인해 더 많은 오버헤드가 발생할 수 있습니다.

UDP: UDP 헤더는 TCP보다 훨씬 작으며, 기본적인 소스 및 목적지 포트 번호, 길이 등의 정보만 포함합니다. 따라서 더 적은 오버헤드가 발생합니다.

> <h2>결론</h2><br>신뢰성과 순서가 필요한 경우에는 TCP를, 간단하고 빠른 전송이 중요한 경우에는 UDP를 선택할 수 있습니다.

<br><br>

---

<br>

> `Chrome` 브라우저에 `Google`을 검색하고 화면에 Google 화면이 출력이 될 때 가지 내부적으로 어떤 동작들이 수행되는지 설명해주세요.

~~이건 진짜 지독하네~~

>> GPT의 답변

1. **사용자 입력 및 URL 해석**

- 사용자가 주소 표시줄에 "google.com" 또는 검색어를 입력합니다.
- 브라우저는 입력된 내용을 해석하여 검색어인 경우에는 검색 엔진을 통해 검색 결과를 가져올 준비를 합니다.

2. **DNS(Domain Name System) 조회**

- 브라우저는 입력된 도메인 이름("google.com")을 IP 주소로 변환하기 위해 DNS 조회를 수행합니다.
- DNS 조회는 도메인 이름을 해당하는 IP 주소로 해석하여 서버에 연결할 수 있도록 돕습니다.

3. **서버 연결**

- 브라우저는 Google 서버의 IP 주소를 얻었으므로 해당 서버와 TCP/IP 연결을 설정합니다.
- TCP/IP 연결은 클라이언트(브라우저)와 서버 간의 네트워크 연결을 말합니다.

4. **HTTP 요청**

- 브라우저는 서버에 HTTP 요청 메시지를 보냅니다. 이 요청은 사용자의 검색어와 관련된 정보를 서버에 전달합니다.
- 요청 메시지에는 HTTP 메서드(GET)와 요청 URI(/search?q=검색어)가 포함됩니다.

5. **서버 응답**

- Google 서버는 브라우저의 요청을 받고, 검색 결과를 포함한 HTTP 응답 메시지를 생성합니다.
- 응답 메시지는 HTML, CSS, `JavaScript` 및 기타 리소스로 구성될 수 있습니다.

6. **응답 처리**

- 브라우저는 서버로부터 받은 응답 메시지를 받아서 처리합니다.
- HTML 문서를 파싱하여 웹 페이지 구조를 이해하고, CSS 스타일을 적용하여 화면에 표시될 내용을 결정합니다.

7. **화면 표시**

- 브라우저는 파싱된 HTML과 CSS를 기반으로 화면에 웹 페이지를 렌더링합니다.
- 렌더링된 내용은 사용자가 볼 수 있는 형태로 표시되며, 검색 결과 및 관련된 링크, 이미지 등이 표시됩니다.

8. **추가 리소스 로딩**

- 웹 페이지에는 추가적인 리소스(이미지, 스크립트, 스타일시트 등)가 포함될 수 있습니다.
- 브라우저는 이러한 리소스를 필요에 따라 다운로드하고 화면에 표시합니다.

위의 단계는 Chrome 브라우저에서 Google을 검색하는 일반적인 시나리오를 설명한 것입니다. 실제로는 더 복잡한 내부 동작들이 수행되며, Chrome의 렌더링 엔진인 Blink와 다양한 네트워크 및 데이터 처리 기술이 사용됩니다.

<br><br>

---

> 주력으로 사용하는 언어의 설계적 결함을 알려주세요.

`JavaScript`, `Node.js` 기준으로 작성

>> '그'의 답변

- **`JavaScript`의 설계적 결함**


  - 동적 타입: `JavaScript`는 변수의 타입을 선언하지 않고 값을 할당할 수 있습니다. 이는 개발자가 실수로 잘못된 타입의 값을 사용할 수 있게 합니다.

  -> TS로 보완 가능

  - 호이스팅: `JavaScript`는 변수와 함수 선언을 스코프의 맨 위로 끌어올리는 호이스팅을 수행합니다. 이는 코드의 가독성과 예측성을 낮출 수 있습니다.

  -> JS 개발자의 깊은 이해도 필요

  - 유연한 문법: `JavaScript`의 문법은 유연하여 여러 가지 방식으로 코드를 작성할 수 있습니다. 이는 일관성이 떨어지고 예기치 않은 동작이 발생할 수 있는 여지를 남깁니다.

  -> 이맛에하는디;

- **Node.js의 설계적 결함**

  - 콜백 지옥: Node.js는 비동기 이벤트 기반 아키텍처를 채택하여 입출력 작업과 네트워크 요청을 효율적으로 다룰 수 있습니다. 그러나 비동기 콜백의 중첩으로 인해 코드가 길어지고 가독성이 떨어질 수 있는 "콜백 지옥" 상황이 발생할 수 있습니다.<br><br>-> async로 처리 가능한거 아닌지..??
  
  - 단일 스레드 모델: Node.js는 이벤트 루프를 사용하여 단일 스레드에서 비동기 작업을 처리합니다. 이는 확장성과 성능 면에서 이점을 제공하지만, CPU 집약적인 작업이나 장기간 실행되는 작업이 있는 경우 이벤트 루프가 차단될 수 있으며 전체 시스템 성능에 영향을 줄 수 있습니다.<br><br>-> 하나 죽으면 서버 자체가 뻗어버려서 개발자의 역량 중요

  - 모듈 시스템의 부족: `JavaScript`는 초기에는 모듈 시스템이 없었으며, 여러 개발자들이 자체적으로 모듈 패턴을 만들어 사용했습니다. 이로 인해 일관성과 재사용성이 낮아질 수 있었으며, 최근에는 ES6(ECMAScript 2015)부터 공식적인 모듈 시스템이 추가되었습니다. 하지만 모든 환경에서 완벽히 지원되지 않을 수 있습니다.<br><br>-> 이건 이제 아닌거같은디

<br>

---

> `JavaScript`에서 자료구조와 관련된 클래스가 내부적으로 어떻게 동작하는지 한 가지 사례를 정하여 작성해주세요. ex) ArrayList, HashMap

js는,,, 만능해결사 the 메타몽 배열GPT이 스택 큐 리스트 다해먹을수있어서 별로,, 나중에 c를 하든가 py를 하든가 해야지 ,,,

>> GPT의 답변

한 가지 예시로 `JavaScript`에서 배열(Array) 자료구조의 내부 동작을 살펴보겠습니다.

배열은 일련의 값들을 순서대로 저장하는 자료구조입니다. `JavaScript`의 배열은 동적으로 크기가 조정되며, 서로 다른 타입의 값들을 포함할 수 있습니다. 다음은 배열의 내부 동작 예시입니다

1. **배열 생성**

`JavaScript`에서 배열은 다음과 같이 생성할 수 있습니다: `let array = [];` 또는 `let array = new Array();`
배열은 일련의 메모리 공간을 할당받아 값을 저장하는 구조로 생성됩니다.

2. **배열 요소 접근**

배열은 0부터 시작하는 인덱스를 사용하여 각 요소에 접근할 수 있습니다. 예를 들어, `array[0]`은 첫 번째 요소를 나타냅니다.
`JavaScript`의 배열은 인덱스에 상응하는 메모리 주소로 직접 접근할 수 있는 특성을 가지고 있습니다.

3. **배열 값 추가/수정**

배열에 값이 추가되면, 배열의 끝에 새로운 요소가 추가됩니다. 예를 들어, `array.push(value)` 메서드를 사용하여 배열의 끝에 값을 추가할 수 있습니다.
배열의 특정 인덱스에 값을 수정하고자 할 경우, `array[index] = newValue`와 같은 형태로 값을 할당하면 됩니다.

4. **배열 값 삭제**

배열에서 요소를 삭제하려면, `array.splice(index, count)` 메서드를 사용합니다. 이는 특정 인덱스부터 지정된 개수의 요소를 삭제합니다.
또는 `delete array[index]`와 같이 요소를 삭제할 수도 있지만, 해당 인덱스는 여전히 존재하며 `undefined로` 값이 설정됩니다.

5. **배열 크기 조정**

`JavaScript`의 배열은 동적으로 크기를 조정할 수 있습니다. `array.length` 속성을 사용하여 배열의 길이를 확인하거나 설정할 수 있습니다.
배열의 크기를 늘리려면 `array.length` 값을 증가시키고, 줄이려면 값을 감소시키면 됩니다.
`JavaScript`의 배열은 내부적으로 동적으로 크기가 조정되며, 값들은 메모리에 연속적으로 저장됩니다. 이를 통해 인덱스를 기반으로 빠르게 접근하고 요소를 추가/수정/삭제할 수 있습니다. 자바스크립트의 배열은 ArrayList와 유사한 동작을 하지만, 내부 동작 및 구현 방식은 약간 다를 수 있습니다.

## <h2>Frontend</h2>

---
