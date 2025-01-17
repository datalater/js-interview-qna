# 💁 면접 질문 정리

## 기술 면접

### 프론트엔드 전반

<details>
<summary><strong>CSR과 SSR 차이</strong></summary>

> [velog - 기술 스터디: SSR과 CSR의 차이](https://velog.io/@vagabondms/%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%84%B0%EB%94%94-SSR%EA%B3%BC-CSR%EC%9D%98-%EC%B0%A8%EC%9D%B4)

CSR과 SSR의 차이는 렌더링을 서버에서 하느냐 클라이언트에서 하느냐의 차이입니다. SSR의 경우 서버에서 렌더 준비가 모두 끝낸 HTML을 브라우저에 내려줍니다.

먼저 SPA와 MPA의 개념에 대해 알고 있어야 한다.

- SPA: 하나의 HTML 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션 (CSR이라고 보면 된다.)
- MPA(Multi Page Application): 사용자가 페이지를 요청할 때 마다 서버가 (요청 받은) UI와 필요한 데이터를 기반으로 HTML로 파싱하여 보여주는 방식의 웹 어플리케이션이다. (SSR 이라고 보면 된다.)

- CSR: 브라우저가 서버에 HTML과 JS파일을 요청한 후 로드되면 사용자의 상호작용에 따라 JS를 이용하여 동적으로 렌더링 한다.
  - 장점
    - 첫 로딩(JS 다운로드 후 실행까지)만 기다린다면 동적으로 화면이 빠르게 렌더링 되기 때문에 사용자 경험이 좋다.
    - 서버에 요청하는 횟수가 훨씬 적기 때문에 서버의 부담이 덜하다
  - 단점
    - 모든 스크립트 파일이 로드될 때까지 기다려야 한다. (리소스를 chunk 단위로 묶어서 요청할 때만 다운받게 하는 방식으로 완화시킬 수 있지만 완벽히 해결할 수는 없다.)
    - 맨 처음 index.html의 구조가 빈 껍데기이기 때문에 검색엔진의 검색 봇이 크롤링하는데 어려움을 겪어 검색엔진 최적화의 문제가 있다. 하지만 구글 봇의 경우는 JS를 지원하지만 다른 검색엔진의 경우 그렇지 않다
- SSR: 브라우저가 페이지를 요청할 때마다 해당 페이지와 관련된 HTML, CSS, JS 파일 및 데이터를 받아와서 렌더링 한다.
  - 장점
    - 초기 로딩 속도가 빠르기 때문에 사용자가 컨텐츠를 빨리 볼 수 있다.
    - JS를 이용한 렌더링이 아니라 서버에서 렌더링을 하기 때문에 검색엔진 최적화가 가능하다.
  - 단점
    - 매번 페이지를 요청할 때마다 새로고침 되기 때문에 사용자 경험이 SPA에 비해 좋지 않다.
    - 서버에 매번 요청을 하기 때문에 서버의 부하가 커진다.
- SSG(Static Site Generation) - [https://velog.io/@longroadhome/FE-SSRServer-Side-Rendering-그리고-SSGStatic-Site-Generation-feat.-NEXT를-중심으로](https://velog.io/@longroadhome/FE-SSRServer-Side-Rendering-%EA%B7%B8%EB%A6%AC%EA%B3%A0-SSGStatic-Site-Generation-feat.-NEXT%EB%A5%BC-%EC%A4%91%EC%8B%AC%EC%9C%BC%EB%A1%9C)

</details>

<details>
<summary><strong>브라우저 렌더링 원리</strong></summary>

브라우저가 화면에 나타나는 요소를 렌더링할 때 `웹킷`, `게코` 같은 렌더링 엔진을 사용한다. 렌더링 엔진이 HTML, CSS, Javascript로 렌더링 할 때 CRP(Critical Rendering Path)라는 프로세스를 사용하며 다음 단계들로 이루어진다.

1. HTML 파싱 후, DOM 트리 생성
2. CSS 파싱 후, CSSOM 트리 생성 (CSS는 렌더링 차단 리소스로 취급된다. CSSOM이 생성될 때까지 브라우저는 처리되는 모든 컨텐츠를 렌더링하지 않는다. 그렇기 때문에 CSS를 간단하게 유지하고 가능한 빨리 제공해야 한다. 그 이유는 CSS가 있어야 화면 스타일이 제대로 적용될 테니까. 그래서 최대한 빨리 다운로드 받게 해야 함)
3. 렌더 트리 생성 (`display: none`과 같이 화면에 보이지 않고, 공간을 차지하지 않는 것은 렌더트리로 만들어지지 않음) - 렌더트리는 DOM + CSSOM 합한 후 최종적으로 브라우저에 표기될 것들을 표현한 트리
4. 레이아웃 단계: 뷰포트 내의 각 요소들의 크기와 위치를 계산
5. 페인팅: 레이아웃에서 계산한 값들을 화면의 실제 픽셀로 그리는 단계
6. 컴포지션: 레이어를 합치는 과정

</details>

<details>
<summary><strong>[C]자바스크립트 엔진이 코드를 실행하는 과정</strong></summary>

자바스크립트를 실행하기 위해서는 자바스크립트 엔진이 필요하고, 웹 브라우저는 자바스크립트 엔진을 내장하고 있다. 브라우저마다 엔진의 종류가 다르지만 코드를 실행하는 방식은 비슷하다.

1. 코드를 만나면 파싱하여 `추상 구문 트리 (AST) - 소스 코드의 추상 구문 구조의 트리` 로 변환된다.
2. 인터프리터는 AST를 기반으로 바이트 코드를 생성한다.
3. 인터프리터가 바이트 코드를 실행하고, 최적화가 가능하다면 최적화 컴파일러에게 보내서 최적화된 코드를 생성합니다.

</details>

<details>
<summary><strong>[C]HTTP 1.1 / 2.0 차이</strong></summary>

> https://developer.mozilla.org/ko/docs/Web/HTTP/Connection_management_in_HTTP_1.x

- HTTP 1.1 : HTTP 1.0은 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 즉, 여러 개의 요청을 한 번에 전송할 수 없고 응답할 수도 없다. 그래서 HTML에 포함된 여러 개의 리소스 요청, 즉 CSS 파일을 로드하는 link 태그나 이미지 파일을 로드하는 img 태그, script 태그 등에 의한 리소스 요청이 개별적으로 전송되고 응답 또한 개별적으로 전송된다. 그래서 요청할 리소스의 개수에 비례하여 응답시간이 증가하는 단점이 있다. HTTP1.1에서 `파이프라이닝이 추가되었고 응답을 기다리지 않고 연속적인 요청을 보내서 네트워크 지연을 줄일 수 있긴 하다.` **하지만 완전한 멀티플렉싱이 아닌 응답처리를 미루는 방식이므로 결국 응답의 처리는 순차적으로 된다(Head Of Line).**
  - 이미지 스프라이팅, 미니파이 CSS javascript
- HTTP 2.0 : 커넥션당 여러개의 요청과 응답이 가능하기 때문에 1.1에 비해 약 50%가 빠르다. 이전 헤더의 내용과 중복되는 필드를 재전송 하지 않아 데이터 절약함

<!-- prettier-ignore-start -->
| 비교      | HTTP 1.0 | HTTP 1.1 | HTTP 2.0 |
| :-------- | :------- | :------- | :------- |
| 다중 요청 | 불가능   | 가능     | 가능     |
| 응답 지연 | 있음     | 있음     | 없음     |
| 네트워크 지연 개선점 | | 응답을 기다리지 않고 연속적으로 여러 개 요청을 보낼 수 있다 (파이프라이닝)  | 요청과 응답이 병렬 처리 된다 (멀티플렉싱)  |
| 네트워크 지연 문제점 | 이전 요청이 처리되어야 다음 요청을 보낼 수 있다 (요청 블로킹) | 이전 응답이 처리되어야 다음 응답이 처리된다(HOC) (응답 블로킹) | |
| 커넥션 | 단기 커넥션(요청을 보낼 때마다 새로운 TCP 커넥션 생성) | 영속적인 커넥션(연속적인 요청 사이에 TCP 커넥션 유지, keep-alive 커넥션) | |
<!-- prettier-ignore-end -->

</details>

<details>
<summary><strong>번들러와 트랜스파일러</strong></summary>

- 번들러
  - 각각 모듈 의존성을 확인하여 하나의 자바스크립트 파일로 만드는 도구이다. webpack, parcel, rollup등이 있다. (최소화, 이미지 압축 등의 여러 가지 기능도 있다)
- 트랜스파일러
  - 특정 언어로 작성된 코드를 비슷한 다른 언어로 변환시키는 행위를 말한다. 트랜스파일러가 필요한 이유는 모든 브라우저가 es6+ 기능을 제공하지 않기 때문이다. 이뿐만 아니라 리액트의 JSX 코드를 자바스크립트 코드로 변환시킨다거나 타입스크립트를 자바스크립트로 변환시키는 등의 역할도 한다. babel이 있다.
  - 브라우저가 이해할 수 없는 언어(ES6+, React, TypeScript)를 이해할 수 있는 언어로 변환시키기 위해 사용한다.

</details>

<details>
<summary><strong>BOM과 DOM</strong></summary>

- BOM (Browser Object Model)
  - 브라우저의 창이나 프레임을 프로그래밍적으로 제어할 수 있게 해주는 객체 모델이다.
  - 전역객체로 `window`가 있으며 하위 객체들로 `location, navigator, document, screen, history`가 포함되어 있다.
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dd23f5be-be63-424a-9fc2-9ac617a91b53/Untitled.png)
- DOM (Document Object Model)
  - 웹 페이지를 프로그래밍적으로 제어할 수 있게 해주는 객체 모델이다. 최상위 인터페이스로 Node가 있다.
  - DOM은 프로그래밍 언어와 독립적으로 디자인되었다.
  - 이런 DOM을 다루기 위해 브라우저가 제공하는 DOM API를 사용하면 된다.
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5a4cf5a-c46c-478f-a558-f1f777fde038/Untitled.png)

</details>

<details>
<summary><strong>CI / CD</strong></summary>

- CI - 지속적 통합
  - 빌드와 테스트를 자동화 하여 공유 저장소 (깃허브)등에 병합시키는 프로세스를 말한다. 코드의 일관성(Consistency)을 제공하기 때문에 지속적으로 통합한다는 용어를 사용한다.
  - 깃허브 액션
- CD - 지속적 전달/배포
  - 지속적 전달: 프로덕션 배포를 바로 할 수 있는 상태로 만들고 배포 자체는 수동으로 실행
  - 지속적 배포: 프로덕션까지 자동으로 배포한다. 어플리케이션의 제공 속도를 증가시킨다.
  - 대표적인 서비스로 젠킨스, 트래비스 등이 있다.

</details>

<details>
<summary><strong>[C]CSS 애니메이션 vs JS 애니메이션</strong></summary>

> - https://d2.naver.com/helloworld/5237120
> - https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/frontend/css-js-animation.md

웹사이트에서 애니메이션을 구현할 때 CSS의 `transition`이나 animation 속성을 사용할 수 있고, 자바스크립트의 `setInterval`, `requestAnimationFrame`을 사용할 수 있다.

- CSS 애니메이션
  - 일반적으로 마우스 hover, 메뉴 전환과 같은 간단하게 처리하는 애니메이션의 경우 CSS로 처리한다.
  - 예를 들어 200px 크기의 정사각형을 왼쪽 위에서 오른쪽 아래로 350px 움직이게 하는 애니메이션을 구현한다고 하면 `transform`의 `translate`를 사용하여 구현할 수 있다.
  - 하지만 이를 자바스크립트로 구현하려면 `setInterval`을 통해 계속해서 `style.top`, `style.left` 속성을 변화시켜야 한다. 이는 브라우저 렌더링 과정에서 reflow (레이아웃) 단계를 발생시키므로 성능에도 좋지 않다.
  - 장점
    - 반응형으로 애니메이션을 구현하기에 유용
    - 외부 라이브러리 필요없음
    - CSS가 선언형이기 때문에 직관적
    - 메인 쓰레드가 아닌 별도의 Compositor Thread에서 그려지기 때문에 메인 쓰레드에서 동작하는 JS보다 효율이 좋다.
- JS 애니메이션
  - CSS로 처리하기에 복잡하고 무거운 애니메이션을 다루기 위해 사용한다.
  - 바닐라 자바스크립트로 구현할 경우 reflow (레이아웃) 현상이 발생하기 때문에 외부 라이브러리를 사용하기도 한다.
  - 아니면 Web Animation API가 나오기도 했다.
  - 장점
    - 애니메이션의 세밀한 구성 가능
    - GPU를 통한 하드웨어 가속을 제어할 수 있다.
    - 브라우저 호환성 측면에서 transition / animation 속성보다 뛰어나다. (브라우저마다 CSS 스펙이 조금씩 다를 수 있기 때문에, 예를 들어 벤더 프리픽스 붙이듯이. 자바스크립트는 자바스크립트 표준 스펙이 있고 바벨도 있으니)

</details>

<details markdown="1">
<summary><strong>라이브러리와 프레임워크 차이</strong></summary>

- 큰 차이는 제어 흐름의 주도권이 어디, 누구에 있느냐이다.
- 라이브러리는 라이브러리를 가져다가 사용하고, 호출하는 측에 주도성이 있다.
- 프레임워크는 어떤 큰 틀 안에서 제어의 흐름에 대한 주도성을 가지고 있다. (뼈대나 기반구조를 뜻하고 제어의 역전 개념이 적용됨)

</details>

<details markdown="1">
<summary><strong>1급 객체란?</strong></summary>

- 변수에 담을 수 있고, 함수의 인자와 결과로 사용할 수 있는 값을 일급이라고 한다.
- 값으로 다룰 수 있는 것을 일급이라고 한다. 값으로 다룰 수 있다는 것은 변수에 담을 수 있고, 함수의 인자와 리턴(결과)으로 사용할 수 있다는 것을 뜻한다.

자바스크립트에 적용

- 변수에 할당할 수 있고, 어떤 함수의 매개변수로 값을 넘길 수 있고, 어떤 함수의 결과값으로 리턴될 수 있는 객체를 말합니다. 고로, 자바스크립트에서 함수는 1급 객체 입니다.
- 함수가 1급객체이기 때문에 콜백패턴이나 고차함수를 만들 수 있습니다.

</details>

<details markdown="1">
<summary><strong>가비지컬렉션 원리</strong></summary>

참조할 수 있는 값은 삭제되지 않고 그렇지 않은 값은 삭제된다.

- 자바스크립트는 reachablilty (도달 가능성)이라는 개념을 사용해 메모리를 관리합니다. 쉽게 말해서 어디서든 접근하거나 사용할 수 있는 값을 말합니다. 도달 가능한 값은 메모리에서 삭제되지 않습니다. 코드에서 참조하는 값이나 체이닝으로 참조할 수 있는 값은 도달 가능하기 때문에 삭제되지 않습니다.
- 자바스크립트 엔진 내에서 가비지 컬렉션은 끊임없이 동작하고 있습니다.
- 내부 알고리즘은 mark and sweep 으로 되어있다. 가비지 컬렉션은 루트 정보를 수집하고 이것을 `마크` 라고 합니다 한번 방문한 객체는 전부 마크하기 때문에 또 방문할 일은 없습니다. 루트에서 도달 가능한 모든 객체를 방문할 때 까지 위 과정을 반복합니다. 그리고 마크되지 않은 객체들을 삭제합니다.

</details>

### 최적화

<details markdown="1">
<summary><strong>코드 스플리팅</strong></summary>

(문제점) SPA는 하나의 파일에 전체 애플리케이션에 대한 코드가 모두 포함된다. 프로젝트 규모가 커질수록 한번에 불러와야 하는 코드의 양이 많아진다.

(해결책) 지금 당장 필요한 코드가 아니라면 따로 분리(split)시켜서 나중에 필요할 때 불러와서 사용하는 기법이 코드 스플리팅이다. 즉, 하나로 번들링한 파일을 여러 개의 번들 파일로 분리한다.

(예시) 가장 쉬운 방법 중 하나는 리액트 라우터를 사용하여 라우트 기준으로 코드를 스플리팅하는 것이다.

(코드) [리액트 앱 - 바벨에서 제공하는 dynamic import 예제](https://zereight.tistory.com/969) [리액트 앱 - 리액트에서 제공(React.lazy)하는 dynamic import 예제](https://jforj.tistory.com/162?category=877028#:~:text=%EC%BD%94%EB%93%9C%20%EC%8A%A4%ED%94%8C%EB%A6%BF%ED%8C%85%EC%9D%B4%EB%9E%80%3F,%EB%A1%9C%20%EB%82%98%EB%88%84%EB%8A%94%20%EA%B2%83%EC%9D%84%20%EC%9D%98%EB%AF%B8%ED%95%A9%EB%8B%88%EB%8B%A4.)

- 코드를 분할하여 해당 컴포넌트가 로딩되야 할 시점에만 로드 하는 방법이다.
  - 페이지별, 모듈별, 두 가지를 섞어서 할 수 있다.
  - 가장 핵심은 적절한 사이즈의 코드가 적절한 타이밍에 로드되도록 하는 것이다.
  - 가장 쉬운 방법은 React Router를 사용하여 라우트 기반의 코드 스플리팅을 하는 것이다.
  - 텍스트 압축
    - 응답 헤더를 보면 gzip으로 압축되어 있는 경우를 볼 수 있다.
    - 하지만 빌드된 main.js를 보면 응답헤더에는 Content-Encoding을 찾아볼 수 없다. 이 의미는 번들링된 파일은 압축되어 있지 않다는 의미이다.
    - 즉 번들 파일을 서비스 해주는 서버에서 해줘야 한다.
    - 하지만 무분별 하게 압축을 하면 안된다. 2kb 기준으로 압축 유무를 결정하는데 무분별하게 압축하면 압축 해제하는데 시간이 더 걸릴 수 있기 때문

</details>

<details markdown="1">
<summary><strong>애니메이션 최적화</strong></summary>

- 핵심은 비용이 많이 드는 리플로우(레이아웃)과 리페인트를 스킵하는 것이다.
  - 리플로우가 일어나야 리페인트가 일어나는 것은 아니다.
  - 하지만 리플로우가 일어나면 리페인트는 반드시 일어난다.
  - 리플로우 (레이아웃)
    - position
    - width, height
    - margin, padding
    - border
    - font-size, font-weight
    - overflow
  - 리페인트
    - background, color
    - text-decoration
    - border-radius, border-style
    - visibility
    - outline
  - 레이아웃과 리페인트 단계 모두 생략하기 위해선 GPU의 도움을 받으면 되는데 transform, opacity, cursor 같은 GPU가 관여할 수 있는 속성을 변경하는 것이다.

</details>

<details markdown="1">
<summary><strong>프리로딩</strong></summary>

> https://ui.toast.com/weekly-pick/ko_2021117

- 사용자가 해당 페이지에 접근하기 전에 로딩 시키는 것이다.
- 버튼에 마우스를 올렸을 때 로딩
- 컴포넌트가 마운트 되었을 때 로딩

</details>

<details markdown="1">
<summary><strong>이미지 사이즈 최적화</strong></summary>

- 포맷
  - PNG : 용량이 상대적으로 큼
  - JPG : 압축을 하기 떄문에 PNG에 비해 용량이 작으나 화질저하의 우려가 있다. 하지만 투명도를 지정할 수 없음
  - WEBP : 구글에서 개발한 차세대 이미지 포맷이다. 투명도도 지원하고 JPG, PNG에 비해 더 작은 크기를 가진다. 하지만 지원하지 않는 브라우저가 있을 수 있다.
  - 이럴 경우 picture 태그를 사용하여 해결
- 압축
  - squoosh를 사용하여 이미지 압축
- 폰트 사이즈 최적화
- FOUT - Flash of Unstyled Text
  - 폰트가 다운로드 되기 전엔 기본 폰트로 보임
  - 폰트가 다운로드 완료 되어야 적용
  - IE, Edge
- FOIT - Flash of Invisible Text
  - 폰트가 다운로드 되기 전에는 화면에 안보임
  - 다운로드 되어야 보임
  - 크롬 사파리
- 최적화 방법
  - 궁극적 목표는 FOIT 와 FOUT 최소화
  - 폰트 사이즈 줄이기
  - `font-display` 속성으로 상황에 따라 FOUT, FOIT 을 결정할 수 있다.
  - 트릭 폰트: 다운로드가 완료되는 시점에 fade-in 효과를 준다. 이 때 Font Face Observer 라이브러리의 도움!
  - ttf가 아니라 woff, woff2 를 적용한다. 마찬가지로 브라우저 적용여부를 확인해야 함
  - 사용자 PC에 폰트가 설치되어있을 경우 local 폰트 사용
  - Subset 사용 - 불필요한 글자를 제거하고 사용할 글자들만 폰트로써 적용 한다.
  - 웹에서 CSS가 로드 되어야 그 시점에 CSS에 명시한 폰트가 필요하다는 것을 알게 되는데 그 시점보다 더 먼저 로드를 시킨다. 그러기 위해선 HEAD 안에 명시

</details>

- 캐시 최적화
  - Cache-Control을 사용하여 서버로 특정 리소스를 요청할 때 서버에 해당 `리소스의 캐시를 적용해 달라`고 요청해야 한다. 이것은 Http Header에 있다. (no-store, no-cache, 등등의 옵션)
  - 하지만 클라이언트에서 요청한다고 해서 바로 적용할 수 있는 것은 아니고 서버에서 설정을 해줘야 한다.
  - 그럼 서버는 어떻게 캐시데이터가 서버에 있는 최신의 데이터랑 같은지 판단할 수 있을까? 바로 응답 헤더에 있는 etag로 확인할 수 있다. etag는 해시값이라고 생각하면 된다.

### HTML

- DOCTYPE
  Document Type의 약자로 HTML이 어떤 버전으로 작성되었는지 미리 선언하여 브라우저가 내용을 올바르게 표시할 수 있도록 하는 것이다. 만약 이를 선언하지 않으면 호환 모드로 동작한다.
  호환 모드는 브라우저마다 문서를 나타내는 방식이 다르기 때문에 크로스 브라우징 이슈가 훨씬 심해지게 된다.
  - DTD (Document Type Definition)
    - 문서 형식을 정의해놓은 것으로 DOCTYPE을 명시할 때 사용한다.
    - 즉, HTML 문서가 어떤 문서 형식을 따르는지 DOCTYPE에서 DTD를 지정한다.
    - 현재시점에선 `<!DOCTYPE html>` 로 HTML5를 명시하는 것이 젤 바람직하다.
- Local Storage, Session Storage, Cookie
  - 이 3개의 공통점은 동일 출처 정책을 따르기 때문에 다른 도메인에서 접근할 수 없다.
  - Web Storage = Local Storage (영구 저장소) + Session Storage (임시 저장소)
    | | Local Storage | Session Storage | Cookie |
    | ------------- | ----------------------- | -------------------------- | ----------------- |
    | 생성자 | 클라이언트 | 클라이언트 | 클라이언트 / 서버 |
    | 지속시간 | 명시적으로 지울 때 까지 | 탭 / 윈도우를 닫을 때 까지 | 설정 여부에 따름 |
    | 용량 | 5MB / 10MB | 5MB | 5KB |
    | 서버와의 통신 | X | X | O |
    | 취약점 | XSS 공격 | XSS 공격 | XSS / CSRF 공격 |
  - 쿠키와 Web Storage의 차이점
    - 쿠키는 매번 서버로 전송된다. 웹 스토리지는 클라이언트에 존재만 할 뿐 서버로 전송은 하지 않는다.
- script, script async, script defer 차이
  - script : HTML 파싱이 중단되고 즉시 스크립트 로드되고 로드된 스크립트가 실행되고 파싱이 재개 된다.
  - script async : HTML 파싱과 병렬적으로 로드되고 스크립트를 실행할 때는 파싱이 중단된다. (다른 스크립트가 의존하지 않는 독자적인 스크립트를 로드할 때 적합)
  - script defer : HTML 파싱과 병렬적으로 로드가 되는데, 파싱이 끝나고 스크립트를 로드한다. body 태그 직전에 script 태그를 삽입하는 것과 동작은 같지만 브라우저 호환성에 따라 다를 수 있다. → `**파싱이 중단되지 않음**`
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/feae7c36-b9d5-4e1a-ab14-6e1bbb8f3fb9/Untitled.png)
- 시멘틱 마크업
  - 의미를 잘 전달하도록 문서를 작성하는 것을 말한다.
  - header, footer
  - main, section
  - article - 독립적 컨텐츠
  - h1 - 최상위 제목
  - ul, li - 순서가 없는 목록
  - nav - 내비게이션
  - 등등..
  - 이런식으로 태그가 가지고 있는 의미에 맞게 사용하는 것이다. 이 외에 CSS 스타일을 명시하는 태그를 사용하지 않는 것 또한 시맨틱 마크업의 한 종류이다. 즉 태그가 가지는 의미 자체가 스타일이라면 이는 마크업 자체가 스타일을 갖는 것이기 때문에 시맨틱 마크업에 적합하지 않다.
    - b 태그와 strong 태그를 보면 된다.
    - b 태그는 bold의 약어이기 때문에 태그 자체에 스타일을 가지지만
    - strong은 그 안의 내용이 다른 내용보다 더 강조되어야 한다 라는 의미를 가지기 때문에 시맨틱 마크업에 더 적합하다.
  - 특징
    - 검색엔진이 시맨틱 태그를 중요한 키워드로 간주하기 때문에 검색엔진 최적화에 유리하다
    - 웹 접근성 측면에서 시각장애가 있는 사용자로 하여금 그 의미를 훨씬 더 잘 파악할 수 있다.
    - div, span으로 둘러싸인 요소들보다 코드를 볼 때 가독성이 더 좋다.

### CSS

- 마진 겹침 현상
  블록 레벨 엘리먼트에서 발생하는 현상으로 좌우로는 적용되지 않고 `오로지 수직 방향` 으로만 적용된다.
  만약 2개의 마진이 겹칠 때 더 큰 마진으로 덮어 씌우는 방식이고 하나의 마진이 음수일 경우 더하는 방식이다.
  마진 겹침은 블록 레벨 엘리먼트라는 가정하에 3가지 경우에 한해서 발생한다.

  1. 인접한 엘리먼트
  2. 부모와 처음, 마지막 자식 사이에서
  3. 빈 엘리먼트

     ```html
     <div class="empty"></div>
     <div class="element"></div>
     ```

- Box Model
  문서상의 요소들은 시각적인 목적을 위해서 모든 요소를 하나의 직사각형 박스로 여기는 모델이다.
  - 컨텐츠 영역 : 글이나 이미지, 비디오 등 요소의 실제 내용
  - 안쪽 영역 (Padding) : 안쪽여백 경계가 감싼 영역으로 컨텐츠 영역을 요소의 안쪽 여백까지 포함하는 크기로 확장한다.
  - 테두리 영역 (Border) : 테두리 경계가 감싼 영역으로 안쪽 여백 영역을 요소의 테두리까지 포함하는 크기로 확장한다.
  - 바깥여백 영역 (Margin) : 바깥여백 경계가 감싼 영역으로 테두리 영역을 확장해 요소와 인근 요소 사이의 빈 공간까지 포함하도록 한다.
  - box-sizing
    - 이 속성을 사용하면 width와 height가 컨텐츠 영역 기준인지 테두리 영역 기준인지 정한다.
    - border-box : 테두리 영역이 기준이며 마진 영역부터 포함하지 않는다.
    - content-box : 기본값이며 컨텐츠 영역 기준이다. 즉 패딩 영역부터 포함하지 않는다.
      ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7fec39b4-24a7-4240-8468-d4fd7b286734/Untitled.png)
- z-index 동작
  z-index를 알기 위해선 쌓임 맥락에 대해 알고있어야 한다.
  - 쌓임 맥락 : HTML 요소들에 사용자를 바라보는 기준으로 가상의 z축을 생성하여 3차원 개념으로 보는 것이고, 그 요소들의 우선순위를 z-index가 정하는 원리이다.
  - 쌓임 맥락을 형성하는 조건
    - position이 relative/absolute이면서 z-index가 auto가 아닌 요소
    - position이 fixed/sticky인 요소
    - opacity가 1보다 작은 요소
    - transform이 none이 아닌 요소
  - 우선 순위
    - 동일한 성격의 요소라면 마크업 순서로 쌓임이 결정된다.
    - 쌓임 맥락을 형성한다면 z-index 값을 설정할 수 있는 position: static이 아닌 요소들의 경우 동일한 마크업 레벨에서 z-index 값으로 우선순위를 결정한다.
    - z-index 값을 설정할 수 없는 요소라면 마크업 순서로 결정한다.
    ```html
    <div>1</div>
    <div>2</div>
    <div>
      3
      <div>4</div>
      <div>5</div>
      <div>6</div>
    </div>
    ```
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/11a081d2-0218-4b01-bc4a-33340674a6b5/Untitled.png)
- block vs inline vs inline-block
  | 특징 | block | inline | inline-block |
  | --------------------------------- | ----- | ------ | ------------ |
  | 상하 마진/패딩 | ✅ | ❌ | ✅ |
  | 좌우 마진/패딩 | ✅ | ✅ | ✅ |
  | 요소 사이 줄바꿈 | ✅ | ❌ | ❌ |
  | 기본너비가 부모너비 | ✅ | ❌ | ❌ |
  | 요소 사이 공백 | ❌ | ✅ | ✅ |
  | 너비와 높이 명시했을 때 적용 여부 | ✅ | ❌ | ✅ |
- Normalize.css vs Reset.css
  - Reset.css
    - 모든 브라우저의 내장 스타일을 없애는 기법
    - Eric Mayer의 Reset CSS가 유명하다
  - Normalize.css
    - 모든 브라우저의 스타일을 동일하게 하는 기법
    - Rest.css와는 다르게 기존 스타일을 유지하되 브라우저들의 다른 스타일을 고치는 방식
    - necolas의 Normalize CSS가 유명하다

### Javascript

- 기본자료형
  - Boolean
  - Number
  - String
  - Null
  - Undefined
  - Symbol
  - BigInt
- 콜백이란
  나중에 실행할 함수를 다른 함수의 매개변수로 전달하고 어떤 이벤트가 발생했을 때 매개변수로 전달한 함수가 다시 호출되는 것을 말한다. 즉, 어떤 일을 다른 객체에게 시키고 그 일이 끝나는 것은 기다리지 않고 끝나고 부를 때 까지 다른 일을 하는 것을 말한다!
- 콜백지옥이란
  콜백함수를 익명함수로 전달하는 과정이 반복되어 코드의 들여쓰기 수준이 감당하기 힘들정도로 깊어지는 현상을 말합니다. 이벤트처리나 서버 통신과 같은 비동기적 작업을 수행하기 위해 이런 형태가 등장하는데 가독성이 떨어지고 코드수정하기 어렵다.
- 쓰로틀, 디바운스 차이

  - 쓰로틀링

    - 마지막 함수가 호출된 이후 일정 시간이 지나기전에 다시 호출되지 않도록 하는것 (스크롤에 쓰인다.)

    ```jsx
    const throttle = (callback, delay) => {
      let timer;

      return (event) => {
        if (!timer) {
          timer = setTimeout(() => {
            callback(event);
            timer = null;
          }, delay);
        }
      };
    };

    $container.addEventListener(
      "scroll",
      throttle(() => {
        $throttleCount.textContent = ++throttleCount;
      }, 100)
    );
    ```

  - 디바운스

    - 연속으로 호출되는 함수중 마지막에 호출되는 함수만 실행되도록 하는 것 (딱 한번 호출, 키보드 검색에서 마지막에 딱 한번 실행)

    ```jsx
    const debounce = (callback, delay) => {
      let timer;
      return (event) => {
        if (timer) {
          clearTimeout(timer);
        }
        timer = setTimeout(callback, delay, event);
      };
    };

    $input.oninput = debounce((e) => {
      $msg.textContent = e.target.value;
    }, 300);
    ```

- 함수의 종류
  - 일반 함수
  - 익명 함수
  - 중첩 함수
  - 콜백 함수
  - 즉시 실행 함수
  - 화살표 함수
- AJAX
  - Asynchronous Javascript And XML의 약자로 비동기적으로 JS를 사용해 데이터를 받아와 동적으로 DOM 갱신 및 조작하는 웹 개발 기술
  - 장점
    - 페이지를 전환하지 않고 빠르게 화면 일부분 업데이트 가능
    - 수신하는 데이터 양을 줄일 수 있고, 클라이언트에게 처리를 맡길 수 있다.
    - 서버 처리를 기다리지 않고 비동기 요청 가능
  - 단점
    - 지원하지 않는 브라우저
    - 페이지 전환 없이 서버와 통신을 하기 때문에 보안상 문제가 있을 수 있다?
    - CORS 문제 발생할 수 있음
- 이벤트 위임
  - 이벤트 버블링 : 하위 엘리먼트에 이벤트가 발생했을 때 상위요소까지 이벤트가 전달됨
  - 이벤트 캡쳐링 : 하위 엘리먼트에 이벤트 핸들러가 있을 때 상위 엘리먼트로부터 이벤트가 발생하기 시작하여 하위 엘리먼트까지 전달되는 방식
  - 기본적으로 이벤트는 `버블링`으로 동작한다. addEventListener()에 마지막 인자로 {capture:true}를 전달해 주면 캡쳐링으로 동작한다.
  - 즉, 이러한 성질을 이용하여 하위 엘리먼트가 여러개 있을 때 각각 이벤트를 달아주는게 아니라 상위 요소에만 이벤트를 달아서 제어하는 방식이다.
  - 장점
    - 동적으로 엘리먼트를 추가할 때 마다 핸들러를 추가할 필요가 없다.
    - 코드가 깔끔
    - 메모리에 이벤트 핸들러가 적게 있기 때문에 퍼포먼스 측면에서 이점
- 실행컨텍스트
  **`즉, 실행 가능한 코드가 실행되기 위해 필요한 환경` 스코프, 식별자, 코드 실행 순서 (콜스택)**
  - 전역 코드, Eval 코드, 함수 코드
  - Eval is Evil → 악의적 영향을 받을 수 있는 문자열을 실행하면 위험
  - 또한 최신 자바스크립트 엔진은 코드 구조를 최적화 하는데 eval은 JS 인터프리터를 바로 사용하기 때문에 느리다.
    `코드의 실행환경에 대한 여러가지 정보`를 담고 있는 개념이다.
    전역에 존재하는 `전역코드`, 함수내부에 존재하는 `함수코드`, `EVAL` 함수에 인수로 전달되어 실행되는 소스코드, `모듈 내부`에 존재하는 소스코드가 각각의 실행 컨텍스트가 생성된다.
    자바스크립트 엔진은 `소스코드 평가`와 `소스코드 실행` 2가지로 나누어 처리한다.
  - 소스코드 평가 : 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 렉시컬 환경에 등록한다. 그러기 때문에 호이스팅 발생함
  - 소스코드 실행 : 소스코드 평가 과정이 끝나면 비로소 선언문을 제외한 소스코드가 순차적으로 실행되기 시작한다. 즉 `런타임`이 시작됨! , 이 때 소스코드 실행에 필요한 정보 (변수, 함수의 참조)를 실행 컨텍스트가 관리하는 렉시컬 환경에서 검색해서 취득한다. 그리고 변수의 값이 변경, 소스코드의 실행 결과는 다시 렉시컬 환경에 등록된다.
    **즉, 코드가 실행되려면 스코프, 식별자, 코드 실행 순서 등의 관리가 필요한데 이 모든 것을 관리하는 것이 실행 컨텍스트 이다.**
    특히 코드의 **실행 순서**는 콜 스택이라고 불리는 실행 컨텍스트 스택에서 관리한다.
- 스코프
  **`자바스크립트 엔진이 참조의 대상이 되는 식별자를 검색할 때 사용하는 규칙의 집합이다.`**
  **`변수에 접근할 수 있는 범위`**
  - 스코프 체인 : 현재 스코프에서 식별자를 검색할 때 상위 스코프를 연쇄적으로 찾아가는 방식이다.
  - var 는 함수 레벨 스코프
  - let const 는 블록 레벨 스코프
  - 전역 스코프 : 코드 어디에서 든지 참조 가능
  - 지역 스코프 : 함수 코드 블록이 만든 스코프로 함수 자신과 하위 함수에서만 참조 가능하다
- 렉시컬 환경 (스코프)
  특정 코드가 선언된 환경을 말한다.
  렉시컬 스코핑 - **`함수를 어디서 호출하느냐가 아니라 어디에 선언되었는지에 따라 결정되는 환경이다.`**
  ***
  스크립트 전체, 실행중인 함수, 코드 블록은 자신만의 렉시컬 환경을 갖는다.
  환경 레코드 - 모든 지역변수를 프로퍼티로 저장하고 있는 객체
  외부 렉시컬 환경 - 현재 렉시컬 환경보다 더 상위의 렉시컬 환경이다.
  this는 어디서 호출했는 지에 따라 동적으로 변함, 함수와 헷갈리면 안됨
  ***
  식별자와 식별자에 바인딩 된 값, 상위 스코프에 대한 참조를 기록하는 자료구조이다. 실행 컨텍스트가 실행 순서를 관리한다면 렉시컬 환경은 `**스코프와 식별자를 관리**`한다.
  외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킨다. 이 때 상위 스코프란 외부 렉시컬 환경, 즉 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경을 말한다.
  this는 어디서 호출했는 지에 따라 동적으로 변함
- 호이스팅
  변수 및 함수 선언문이 스코프 내의 최상단으로 끌어올려지는 현상 `**선언문**`만 끌어올려지고 `**대입문**`은 끌어올려지지 않는다.

  - 자바스크립트 엔진은 소스코드 평가, 소스코드 실행으로 나누어져 있다. 소스코드 평가 단계에서 런타임 이전에 선언한 변수와 함수를 실행 컨텍스트가 관리하는 환경에 등록하기 떄문에 발생한다. 그래서 선언문 보다 참조나 호출하는 것이 코드상 먼저 있어서 동작을 하는 것이다 (정확히 말하면 var일경우.. var는 선언과 동시에 undefined로 초기화 됨)
  - 왜 발생하느냐? 런타임 이전에 선언한 변수와 함수를 실행 컨텍스트가 관리하는 환경에 등록하기 때문임 (var 는 선언과 동시에 초기화가 됨 (undefined))
  - 자바스크립트 엔진은 코드를 실행하기 전에 모든 선언(var, let, const, 함수, 클래스)을 스코프에 등록합니다. 그러기 때문에 선언문 보다 참조나 호출하는 것이 나와도 동작을 하는 것입니다. 정확히 말하면 var 키워드로 선언했을 때 이야기 입니다.
  - let, const는 Temperal Dead Zone이 있다.
  - 함수 호이스팅 주의할 점

    - 함수 표현식은 호이스팅 되지 않는다.

    ```jsx
    func(); // TypeError
    var func = function() {};

    여기서 변수 func 자체는 호이스팅 되지만 그 값이 undefined이기 때문에 TypeError (func is not a function)이 발생
    ```

    - 함수와 변수 선언문 중에는 함수 **`선언문`**이 먼저다

    ```jsx
    func();
    var func = function () {
      console.log("변수 호이스팅");
    };
    function func() {
      console.log("함수 호이스팅"); // 이게 호출 됨
    }
    ```

- 클로저
  **`외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저 라고 합니다. 변수를 private 하게 사용할 때 씀`**
  - 렉시컬 스코프 : 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는 지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프 라고 한다.
- ECMAScript 란
  - Ecma 라는 기관이 만든 스크립트 언어 이다.
  - 스크립트 언어의 표준
  - 자바스크립트는 ECMAScript의 사양을 준수하는 범용 스크립팅 언어이다.
- Promise
  - 자바스크립트는 비동기 처리를 하기 위해 콜백 함수를 사용할 수 있다. 하지만 콜백은 콜백 지옥이란 말이 있듯이 가독성이 나쁘고 에러의 처리가 힘들다.
  - ES6에서는 비동기 처리를 위한 또다른 패턴인 Promise가 등장했다.
  - 프로미스의 상태
    - pending : 비동기 처리가 아직 수행되지 않음, 가장 기본 상태
    - fulfilled : 비동기 처리가 성공으로 수행 됨, resolve 함수 호출
    - rejected : 비동기 처리가 실패로 수행 됨, reject 함수 호출
    - settled : fulfilled 또는 rejected 된 상태, settled 상태가 되면 다른 상태로 변화할 수 없다.
  - catch는 모둔 then 메소드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러뿐만 아니라 then 메소드 내부에서 발생한 에러까지 모두 캐치 할 수 있다.
    - then 메서드의 2번 째 콜백 함수는 1번 째 콜백 함수 (성공)에서 발생한 에러를 캐치하지 못한다.
  - 정적 메서드
    - 주로 생성자 함수로 사용되지만 함수도 객체이기 때문에 메서드를 가질 수 있다.
    - Promise.resolve : 성공한 프로미스를 생성
    - Promise.reject : 실패한 프로미스를 생성
    - Promise.all : 여러개의 비동기 처리를 모두 병렬처리, 배열로 받아 서로 의존하지 않음, 모든 프로미스가 fulfilled 되면 resolve 된 처리 결과를 반환함. 처리 순서 보장됨. 하지만 reject가 하나라도 되면 reject 결과
    - Promise.allSettled : 실패해도 실패,성공 한 모든 결과가 나옴 settled 배열로
    - Promise.race : Promise 객체를 반환하고 가장 먼저 완료된 결과값
    - 프로미스 콜백은 마이크로태스크큐에 들어간다, 태스크큐보다 우선순위가 높다
- async, await
  - es8
  - 프로미스를 기반으로 동작한다.
  - 동기 처럼 프로미스를 사용할 수 있다. → 코드의 가독성이 더 좋아집니다.
  - await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
  - async 함수는 언제나 프로미스를 반환한다. 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다.
  - 클래스 생성자 메서드는 async가 될 수 없다.
  - await 키워드는 프로미스가 settled 상태가 되면 resolve 한 결과를 반환한다. await 은 반드시 프로미스 앞에서 사용해야 한다.
  - async 함수 내에서 catch 문을 사용해 에러차리 해야한다. 그러지 않으면 async 함수는 에러를 reject 하는 프로미스를 반환한다.
- Promise, async await의 트랜스파일 결과는?
  - Promise: 큐 사용
  - async await : Promise와 제너레이터, next() 사용
  - [https://dev-momo.tistory.com/entry/Asyncawait는-어떻게-구현하는가-1](https://dev-momo.tistory.com/entry/Asyncawait%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EA%B5%AC%ED%98%84%ED%95%98%EB%8A%94%EA%B0%80-1)
  - [https://medium.com/chequer/js-promise-패턴-구현해보기-a79cba99c9a5](https://medium.com/chequer/js-promise-%ED%8C%A8%ED%84%B4-%EA%B5%AC%ED%98%84%ED%95%B4%EB%B3%B4%EA%B8%B0-a79cba99c9a5)
- 제너레이터
  - 코드 블록의 실행을 일시 중지 했다가 필요한 시점에 재개할 수 있는 함수이다.
  - 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
  - 제너레이터는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다. (yield)
  - 제너레이터 함수는 화살표 함수로 정의할 수 없다.
  - 생성자 함수로도 호출할 수 없다.
  - 제너레이터 함수가 반환한 제너레이터는 이터러블 이면서 이터레이터 이다.
- Symbol 이란
  - Symbol() 함수로 생성되고 그 값은 고유한 값이다.
  - 매개변수로 문자열을 넣을 수 있으며 그저 description의 값이다. 의미를 주진 않음 (디버깅 시에만 의미)
  - 객체가 아니라 원시타입이다.
  - new 연산자를 사용하지 않음
- 이터러블, 이터레이터
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/055c060b-8e43-4456-b65a-36efccc6af3b/Untitled.png)
  - 이터러블
    - Symbol.iterator를 프로퍼티 키를 사용한 메서드를 구현하거나 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. `**이러한 규약을 이터러블 프로토콜**` 이라 한다.
    - for...of 문으로 순회할수 있고, 스프레드 문법, 구조분해 할당의 대상으로 사용할 수 있다.
    - [ Symbol.iterator ]() {...}
    - 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블 이다.
    - Symbol.iterator 를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다. 그래서 일반 객체는 for...of 로 순회할 수 없다.
    - Symbol.iterator in obj; // false
  - 이터레이터
    - Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
    - next 메서드를 가지고 있고 next 메서드를 호출하면 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
    - 이러한 규약을 이터레이터 프로토콜 이라한다.
    - { next () { return { value : any, done: boolean } } }
- 네이티브 객체 vs 호스트 객체
  - 네이티브 객체 : ECMAScript 명세에서 의미론적인 부분을 완전히 정의해놓은 객체
    - Object
    - Function
    - Date
    - Math
    - parseInt 등등..
  - 호스트 객체 : 자바스크립트를 실행하는 환경에 종속된 객체로 그 환경에서만 찾아볼 수 있는 객체이다. 만약 환경이 브라우저라면
    - window
    - document
    - location
    - XMLHttpRequest
    - querySelectorAll 등등..
- this의 바인딩
  - this는 자신이 속한 객체를 가리키는 식별자 이다. 자기 참조 변수이다.
  - 함수 호출 방식에 의해 동적으로 결정 된다.
    - 생성자 함수 내부에서의 this는 생성자 함수가 생성할 인스턴스 이다.
    - 메서드 내부에서 this는 메서드를 호출한 객체이다.
    - 일반함수의 this는 전역객체 window를 가리킨다. (중첩함수, 콜백함수 포함)
    - 화살표 함수는 상위 스코프의 this를 가리킨다.
      실행 컨텍스트가 생성될 때 마다 this의 바인딩이 일어난다.
  1. new 키워드를 사용했을 때 해당 객체로 바인딩 된다.
  2. call, apply, bind 와 같은 명시적 바인딩을 사용했을 때 인자로 전달된 객체에 바인딩 된다.
     1. call : 그냥 일반 파라미터
     2. apply : 두 번째 파라미터엔 배열
  3. 객체의 메소드로 호출할 경우 해당 객체에 바인딩 된다.
  4. strict mode일 때 undefined로 초기화 된다. 일반 모드일 경우 브라우저일 경우 window 객체에 바인딩 된다.
- 즉시실행함수
  함수 표현식을 즉시 실행한다. 익명, 기명함수 모두 사용 가능
  - 사용이유
    - 전역 스코프를 오염시키지 않기 위해 사용 하지만 지금은 대부분 모듈로 사용하기 때문에 잘 쓰지 않는다.
    - 즉시실행 함수 안에서 클로저를 생성하면 private 데이터를 만들 수 있고 외부에서 접근 불가
    - 최소화를 활용환 최적화 - uglify.js 같은 라이브러리(즉시실행함수를 만들어서)는 최소화를 한다.
- 모듈시스템
  모듈이란 여러 기능들에 관한 코드가 모여있는 하나의 파일이라고 생각하면 된다.

  - 유지보수 : 기능들이 모듈화 잘 되어 있으면 의존성을 줄일 수 있기 때문에 유지보수에 좋다
  - 네임스페이스화 : 자바스크립트에서 전역변수는 전역 공간을 가지기 때문에 코드의 양이 많아질수록 네임스페이스가 많아질 수 있다. 모듈로 분리하면 모듈만의 네임스페이스를 갖기 때문에 그 문제 해결
  - 재사용성 상승

  - CommonJS : 자바스크립트는 공식 스펙이 브라우저만 지원했기 때문에 이를 서버사이드 및 데스크탑 어플리케이션에 지원하기 위한 노력이 있었다. 그걸 위해 만든 그룹이 Common JS 이다. 자바스크립트가 범용적인 언어로 쓰이기 위한 스펙을 정의하고 있다.
  - AMD (Asynchronous Module Definition) : Common JS 그룹에서 의견이 맞지 않아 나온 사람들이 만든 그룹으로 비동기 모듈에 대한 표준안을 다루는 그룹이다. AMD는 브라우저 쪽에서 더 큰 효과를 발휘한다. 브라우저에서는 모든 모듈이 다 로딩될 때 까지 기다릴 수 없기 때문에 비동기 모듈로 구현을 해놨다. 이 스펙을 잘 구현한 모듈 로더는 RequireJS 이다.
  - UMD (Universal Module Definition) : CommonJS와 AMD를 통합하기 위한 하나의 패턴
  - ES6 : import와 export 구문을 사용하는 방식이고 모든 브라우저가 지원하지 않기 때문에 babel의 `@babel/plugin-transform-modules-commonjs` 를 통해 변환시켜 사용한다.
    - default export : 모듈 내에 한번만 사용
    - named export : 모듈 내에 여러번 사용

- 콜스택과 힙
  - 콜스택 : 원시타입 값과 함수 호출의 실행 컨텍스트를 저장하는 곳, 소스코드 평가 과정에서 생성된 실행 컨텍스트가 추가되는 자료 구조
  - 힙 : 객체, 배열, 함수와 같이 크기가 동적으로 변할 수 있는 참조 타입 값을 저장
- 이벤트 루프
  자바스크립트는 단일 스레드 기반 언어이므로 단일 콜 스택을 갖는다. 이 말은 요청을 동기적으로 처리한다는 의미이다. 그럼 비동기 처리는 어떻게 할까? (`자바스크립트는 동기적으로 동작`)
  그것은 바로 자바스크립트를 실행하는 환경인 브라우저나 Node.js가 담당한다. 여기서 자바스크립트 엔진과 그 실행 환경을 상호 연동시켜주는 장치가 바로 `이벤트 루프` 이다.
  - 태스크 큐 : 콜백함수가 담김 (setTimeout, setInterval, requestAnimationFrame)
  - 마이크로태스크 큐 : Promise, MutationObserver
    이벤트 루프는 위 2개의 큐를 감시하고 있다가 `콜스택이 비면` 콜백함수를 꺼내와 실행한다. 이 때 마이크로태스크 큐가 우선순위가 더 높다.
    콜스택에 쌓임 → 만약 setTimeout이나 그런 비동기 처리 함수가 들어왔을 경우 → WEB API (노드환경일 경우 백그라운드)로 넘어감 → 여기에 담긴 콜백 함수가 태스크 큐에 들어가 → 콜스택이 다 비면 이벤트 루프가 해당 콜백함수를 콜스택에 넣음
- 프로토타입
  자바스크립트의 모든 객체는 자신의 원형 (프로토타입)이 되는 객체를 가진다.
  보이지 않는 속성인 [[Prototype]] 이 자신의 프로토타입 객체를 참조한다.
  이를 `__proto__` 라는 속성으로 참조할 수 있으나 비표준이기 때문에 실제로 사용하진 말자
  헷갈리는 이유가 함수 객체는 접근할 수 있는 속성인 `prototype` 이 있다. 이름만 보면 같아보이지만 명확히 알아야 한다.
  - 상속!
  - [[Prototype]] : 자신의 프로토타입 객체를 참조하는 속성
  - .prototype : new 연산자로 자신을 생성자 함수로 사용한 경우 그걸로 만들어진 새로운 객체의 [[Prototype]]가 참조하는 값이다.
  - 프로토타입 체인 : 어떤 객체의 프로퍼티를 참조하거나 값을 할당할 때 해당 객체에 프로퍼티가 없을 경우 그 객체의 프로토타입 객체를 연쇄적으로 보면서 프로퍼티를 찾는 방식을 프로토타입 체인이라고 한다.
    - 참조할 때
      - 찾고자하는 프로퍼티가 객체에 존재하면 사용
      - 없으면 [[Prototype]]링크를 타고 끝까지 올라가면서 해당 프로퍼티를 찾는다.
      - 찾으면 그 값을 사용하고 없으면 undefined를 반환
    - 값을 할당할 때
      - 찾고자 하는 프로퍼티가 객체에 존재하면 값을 바꾼다.
      - 프로퍼티가 없고 [[Prototype]] 링크를 타고 올라가서 해당 프로퍼티를 찾았는데 변경가능한 값인 경우 새로운 직속 프로퍼티를 할당한다. 만약 변경하지 못하는 값일 경우 비엄격 모드에서는 무시되고 엄격모드에선 에러가 발생한다.
      - 하지만 해당 프로퍼티가 setter일 경우 이 setter가 호출되고 가려짐이 발생하지 않는다.
- 원시객체와 일반변수의 차이

  - 원시 래퍼 타입: 원시 타입의 값을 감싸는 형태의 객체이다.
  - 원시 래퍼 타입 : 원시 타입을 객체처럼 사용할 수 있다.

  ```jsx
  let name = "bit";
  console.log(name.concat(" coin")); // => "bit coin"

  // 내부적으로 이렇게 동작한다.
  let name = "bit"; // 원시 타입
  // 원시 타입을 객체처럼 사용하는 순간, JS는 임시변수(temp)를 만든다.
  let temp = new String(name); // 임시변수의 메서드를 만들어준다 // 원시 래퍼 타입
  console.log(temp.concat("coin")); // 실행 후!
  temp = null; // 메모리 해제
  ```

- 엄격모드
  ES6 부터 도입된 기능으로 기준에 무시되던 에러들로 하여금 에러를 발생시킨다. 파일 전체에 적용시킬 수도 있고 함수 스코프에 적용시킬 수 있지만 블록 스코프는 불가능하다.
  - var 가 생략된 변수를 전역 객체에 바인딩 하지 않는다.
  - NaN = 5 같은 구문 불가
  - 제거할 수 없는 프로퍼티를 제거할 수 없다
  - with 사용 불가
  - 함수의 매개변수 이름 중복 불가
  - 8진수 사용 불가
  - eslint를 사용하지 못한다면 use strict를 사용하는 것이 좋다.
- 얕은비교, 깊은비교
  - 얕은 비교
    - 동등성을 체크 함
    - 객체를 비교할 때 주소값을 비교 함
    - 그래서 최상위 오브젝트의 참조값이 변하지 않으면 render 함수를 호출하지 않음
  - 깊은 비교
    - 그 안의 값을 확인 함 (루프나 재귀로)
    - JSON, lodash로 깊은 복사 할 수 있음

### 타입스크립트

[https://ahnheejong.gitbook.io/ts-for-jsdev/](https://ahnheejong.gitbook.io/ts-for-jsdev/)

- enum과 object 차이
  - enum : 값들의 집합을 정의하고 이를 사용할 수 있다
  - 객체와의 차이 : 객체는 코드 내에서 새로운 속성을 자유롭게 추가할 수 있지만 enum은 선언 후 변경할 수 없다. 또한 enum의 속성값으로는 문자열, 숫자만 허용 된다.
  - enum 은 안티패턴 이다
    - number 할당이 가능하다
    - 중복되는 코드 작성이 있을 수 있다. (키, 값 같음)
    - enum은 js 코드로 남지만 type은 트랜스파일 시점에 없어 진다. (트리셰이킹 안됨)
- 장단점
  - 장점
    - 정적 타이핑 언어이고 런타임이 아니라 컴파일 시간에 타입 체킹이 가능하기 때문에 오류를 예방할 수 있다.
    - vscode 자동완성
    - 여러가지 패러다임 활용 가능 (절차, 객체, 함수형)
  - 단점
    - 컴파일 하는데 추가적인 시간이 걸릴 수 있다.
    - 자바스크립트 라이브러리를 타입스크립트에서 사용하기 위해 추가적인 작업 필요
    - 가독성
- 함수 오버로딩
  가능하다

### 리액트

[https://velog.io/@ricale/기술-면접에서-받은-질문-2](https://velog.io/@ricale/%EA%B8%B0%EC%88%A0-%EB%A9%B4%EC%A0%91%EC%97%90%EC%84%9C-%EB%B0%9B%EC%9D%80-%EC%A7%88%EB%AC%B8-2)

- 상태관리란
  - 라이브러리가 많이 떠올랐다... ㅠㅠ (개념보단)
  - `상태`는 주어진 시간에 대해 시스템을 나타내는 것으로 언제든지 변경될 수 있다. 즉 문자열, 배열, 객체등의 형태로 응용 프로그램에 저장된 데이터 → 개발자 입장에선 관리해야 하는 데이터들을 의미한다.
  - 상태관리는 상태를 관리하는 방법이다 → 프로덕트가 커짐에 따라 어려움도 커짐
  - 상태들은 시간에 따라 변함
  - 리액트는 단방향 바인딩이므로 프랍 드릴링 이슈 존재
  - 그래서 리덕스, 리코일, mobx같은 라이브러리를 활용해 해결하기도 함
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0980fc3a-40f1-46ad-beb7-956f065264b1/Untitled.png)
    중요한건 트렌드보단 why!
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c734d1bb-a7e7-47b5-8d90-53ae124ba941/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/76a4ed7a-618b-4613-ae42-d725636f1ad9/Untitled.png)
    스태일 타임을 좀 길게 주면 다른 컴포넌트에서 써도 두번 호출하지 않음 (쿼리 클라이언트 내부적으로 컨텍스트 사용함)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d0f2038-85a8-472f-9052-a61c98dd1c37/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c113f29-c5da-423c-b030-e463702ead8e/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/548ee96f-ea67-4a6e-8697-719f14b7b338/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5f3d1c1-3a7a-431c-b0ef-0a12aaa6ccd3/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56516243-58d4-4814-8b18-b302c7ac9ca2/Untitled.png)
  - 1번을 쓰다가 만약 불가능해지면 2번을 쓸것 같다 (배민근님의 의견)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2b111b7-257f-4981-82d9-38be73c28eb0/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f4d294e1-84f7-4db5-9c77-f36fc1a148b9/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/781a7c5c-6da8-41da-925b-800b0aebe0dd/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba0c53c1-3434-4f17-bc91-e07042b551b4/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/61faa54e-7498-46f2-aa1e-a620d62354dd/Untitled.png)
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/183ac40a-38d1-4831-830b-0aa4820ae512/Untitled.png)
- useReducer vs useState
  - useReducer - [https://react.vlpt.us/basic/20-useReducer.html](https://react.vlpt.us/basic/20-useReducer.html)
    - useState를 대체할 수 있다.
    - 다수의 하위값을 포함하는 로직을 사용하거나 현재의 상태에서 어떤 액션을 받아서 새로운 상태로 갱신하는 (리듀서 처럼) 형태인 경우 이것을 사용하는 것이 더 선호된다.
    - 첫 번째 인자로 리듀서 함수를 받고, 두 번째 인자로 초기 상태를 받는다.
    - 리턴 값은 [state, dispatch] 이다.
    - useState 사용했을 때 보다 로직을 파악하기 쉽다. (setState 함수를 여러번 사용하지 않아도 되고, 리듀서로 로직을 분리했기 때문)
- redux vs context api 차이
  - 유사점
    - 모두 Prop 드릴링 현상을 처리할 수 있다.
  - 차이점
    - 리덕스
      - devtools 사용할 수 있다.
      - 여러가지 미들웨어 사용할 수 있다.
      - 전체 앱의 데이터를 상태 객체로 유지관리 한다. 개발자가 제공한 리듀서 함수를 실행하여 데이터의 변경사항을 추적한다.
      - 컴포넌트 렌더링을 최적화
    - 컨텍스트
      - 오직 서로에게 소통하는 한 쌍의 컴포넌트(Provider-Consumer)를 다루게 된다. 이것은 데이터의 고립을 만들 수 있다.
      - 상태를 유지하지는 않고 데이터의 **통로**일 뿐이다. 데이터의 변경 사항을 추적하려면 상위 구성 요소의 상태에 의존해야 한다.
- 함수, 클래스 기반이 각각 무엇이고 왜 있는지
  - 클래스 기반
    - Component를 상속 받아야 함
    - 라이프 사이클 관련 API가 있음
    - render 함수가 꼭 있어야 한다.
    - 함수형에 비해 가독성이 좋지 않은 듯 하다
    - STATE는 객체형이다.
  - 함수 기반 16.7 버전 부터 라이프사이클 사용 가능 훅
    - 클래스형에 비해 선언하기 편하고 메모리 자원을 덜 쓴다.
    - 예전에는 함수형 컴포넌트에서 라이프사이클 API 를 사용할수 없었지만 훅이 도입되면서 사용할 수 있게 되었다.
  - 구분되어 있는 이유?
    - 16.7 이전 버전은 클래스 사용
- 리액트 훅 규칙
  - 최상위 에서만 훅을 호출해야 한다.
    - 그 이유는 컴포넌트가 렌더링 될 때 마다 항상 동일한 순서로 훅이 호출되는 것이 보장 된다.
    - useState, useEffect가 여러번 호출되는 중에도 훅의 상태가 올바르게 유지된다.
    - 그렇기 때문에 조건문 안에서 훅을 호출하면 안됨
    - 왜냐하면 리액트는 훅이 호출되는 순서에 의존
  - 오직 리액트 함수 내에서만 호출해야 한다.
    - 일반적인 자바스크립트 함수에서 호출하면 안된다.
    - 커스텀 훅에서 훅을 호출 해라~
- useLayoutEffect
  - 모든 DOM 변경 이후 동기적으로 발생하고, DOM에서 레이아웃을 읽고 동기적으로 리렌더링 하는 경우?
  - 원래 useEffect는 DOM 레이아웃 배치와 페인트가 끝난 후 호출한다.
  - 즉 돔을 그리기 전에 이를 실행한다. (화면이 그려지기 전에 실행 됨)
  - 즉 **렌더링 이전에 무언가를 하고싶다면 이것을 사용**
  - 그래서 깜빡임을 경험하지 않을 수 있다. (주의) 하지만 로직이 복잡할 경우 사용자가 레이아웃을 보는데 까지 오래걸릴 수 있기 때문에 기본적으론 useEffect를 사용한다.
- React.memo
  - 렌더링 최적화 리렌더링 방지
  - memo로 감싼 컴포넌트를 렌더링 할 때 이전에 기억하고 있던 결과물을 메모리에 기억해두고 있다가 재사용 한다.
  - 얕은 복사를 수행하고, 깊은 복사를 하려면 비교함수를 작성해서 prop으로 넘겨야 한다.
- Element와 Component 차이
  - DOM 노드나 리액트 컴포넌트에 대한 JSON 요약본이다.
  - Element : 컴포넌트 인스턴스나 DOM 노드를 기술하는 순수 객체이다. (버튼, 인풋 등)
    - `**화면에 표시되는 내용으며 리액트를 구성하는 가장 작은 블록이라고 할 수 있다. 브라우저의 DOM과 다르게 보다 적은 비용으로 생성할 수 있다. React DOM이 React Element 와 일치하도록 DOM을 업데이트 한다.**`
    - `**Immutable 엘리먼트 이다. 엘리먼트를 렌더링 한 후에는 해당 엘리먼트의 자식이나 어트리뷰트를 변경할 수 없다.**`
    - DOM의 관점에서 봤을 때 화면에 나타내고 싶은 순수 객체이다.
    - **`한번 생성되면 변형되지 않는다.`**
    - createElement로 생성할 수 있다.
    - 이 생성된 object가 ReactDOM.render()를 통해 렌더링 된다.
    - 스크린에서 보고자 하는 것을 리액트에게 알려주는 방식이다.
    - 엘리먼트는 immutable
  - Component : 여러 방법으로 선언된다. ⇒ 엘리먼트가 모여서 만들어진 것
    - **`컴포넌트는 엘리먼트로 구성될 수 있다. UI를 독립적이고, 재사용가능한 부분으로 분할할 수 있다.`**
    - class, 함수 input으로 props를 전달받고 output으로 JSX를 반환한다.
    - 입력된 컴포넌트를 바탕으로 엘리먼트 트리를 만든다.
- 순수컴포넌트 (PureComponent)
  - 동일한 상태에서 동일한 결과를 반환한다.
  - `shouldComponentUpdate` 를 어떻게 쓰는지가 다르다.
    - `shouldComponentUpdate` 는 props또는 state가 새로운 값으로 갱신되어 렌더링이 발생하기 직전에 호출된다.
    - 기본적으로 return 값은 true인데 false를 반환할 경우 `render` 와 `componentDidUpdate` 는 호출되지 않는다.
  - 순수 컴포넌트는 `shouldComponentUpdate` 가 이미 구현되어 있고, props랑 state를 얕은 비교를 통해 비교한 뒤 변경이 있을 때 true를 리턴하고 없으면 false를 리턴한다.
- state vs props
  - state : 리액트 컴포넌트의 라이프 사이클 동안 변경될 수 있는 정보를 가지고 있는 객체이다.
  - props : 리액트 컴포넌트에 전달되는 값을 포함하는 단일 값 또는 객체이고, 부모에서 자식으로만 전달 된다. 또한 props는 불변이여야 한다. 자식에서 변경하면 안됨
  - 차이점 : props는 부모로부터 전달되고, state는 해당 컴포넌트 안에서 관리된다.
- state를 직접 업데이트 하면 안되는 이유
  - 직접 업데이트 하면 리렌더링 되지 않는다.
  - 리액트가 변경 되었다는 것을 알게 해야 함
  - setState를 사용해야 한다. 그래야 리렌더링이 되면서 새로운 값이 잘 그려지게 된다.
- useState는 비동기로 동작함
  - 만약 하나의 이벤트 핸들러에서 setState가 여러번 호출되면 일괄적으로 업데이트하고 렌더링 한다. → 성능 향상을 위해 비동기로 처리 함 batch
  - **그 이유는 리액트가 가상돔을 사용하는데 만약 한줄 한줄 동기로 한다면 리렌더링이 많이 발생하기 때문이다.**
  - 제대로된 값을 사용하기 위해 useEffect로 사용 하거나 함수형 업데이트로 사용
  - 함수형 업데이트로 사용하면 함수들은 큐에 저장되어 순서대로 실행함
  - 동기적으로 동작하면 상태 업데이트를 한 개씩 할 때마다 계속 리렌더링 해야 하므로 비동기 방식을 사용해서 효율적으로 만든다
- HTML과 React의 이벤트 처리 차이
  - HTML
    - 이벤트 이름이 소문자 이다. onclick ...
  - React
    - 이벤트 이름이 카멜케이스 이다. onClick ...
    - 기존 이벤트 객체를 래핑한 이벤트 객체를 사용
- this 바인딩

  - 클래스 컴포넌트에서 this의 바인딩은 생성자에서 할 수 있다.
  - 생성자에서 하지 않는 경우 클래스 필드 구문을 활용하여 바인딩 할 수 있다.

    ```jsx
    handleClick = () => {
      console.log("this is:", this);
    };

    <button onClick={this.handleClick}> Click me </button>;
    ```

- 리액트 합성이벤트란?
  - `SyntheticEvent`
  - 브라우저마다 이벤트를 처리하는게 다르기 때문에 이를 동일하게 쓰게 위해
  - 모든 브라우저에서 이벤트를 동일하게 처리하기 위해 브라우저의 기본 이벤트를 감싼 cross-browser 래퍼 이다. 브라우저 네이티브 이벤트와 같은 인터페이스를 가지고 있다.
- list 에서 key를 사용하는 이유
  - 가상돔에서 리스트의 변경사항이나 추가, 제거된 항목을 분별할 수 있도록 도와준다.
  - 그렇기 때문에 배열의 인덱스로 사용하는 것을 지양해야 한다. (항목의 순서가 변경될 가능성이 있을 경우!)
  - 자식 컴포넌트에서 key를 사용하면 안된다. 부모 컴포넌트에서 자식 컴포넌트를 사용할 때 써야 한다.
- ref 용도
  - DOM 요소나 컴포넌트에 직접 접근할 때 사용
  - 렌더링에 영향을 끼치지 않는 값에 사용 (ex. 타이머의 아이디)
- forwardRef

  - ref를 받아 child component에 전달한다.
  - 리액트 컴포넌트에서 ref prop을 사용하려면 이것을 써야 한다.
  - 이렇게 감싸면 두번째 매개변수로 ref를 받는다

  ```jsx
  const FancyButton = React.forwardRef((props, ref) => (
    <button ref={ref} className="FancyButton">
      {props.children}
    </button>
  ));

  // 이제 DOM 버튼으로 ref를 작접 받을 수 있습니다.
  const ref = React.createRef();
  <FancyButton ref={ref}>Click me!</FancyButton>;
  ```

- 라이프사이클
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9871057-fbfb-4919-b9e3-6fa60ad30d13/Untitled.png)

  - 컴포넌트 라이프 사이클

    - Mount : DOM이 생성되고 웹 브라우저 상에 나타남
    - Update
      - Props, State가 바뀔 때
      - 부모 컴포넌트가 리렌더링 될 때
      - forceUpdate로 강제 렌더링 할 때
    - Unmount : DOM에서 제거됨

  - 리액트 라이프 사이클
    - **getDerivedStateFromProps** : 최초 마운트시, 부모 컴포넌트에서 전달해주는 props 가 변경돼었을 때, state가 변경되었을 때 호출됨
    - ~~componentWillMount~~ : 렌더링 전에 실행 (deprecated)
    - **componentDidMount** : 첫 렌더링 이후 실행 여기서 비동기 요청
    - **shouldComponentUpdate** : 컴포넌트의 업데이트 여부 결정함. 기본값은 true 임, true 이면 렌더링 된다.
    - ~~componentWillUpdate~~ : shouldComponentUpdate 에서 true가 반환되었을 때 state와 props의 변화를 확인하고 리렌더링 되기전에 실행한다. (deprecated)
    - **getSnapshotBeforeUpdate** : 렌더링 결과가 실제 돔에 반영되기 직전에 호출 됨, `**여기에서 반ㅇ환 값이 componentDidUpdate 의 세 번째 인자로 들어감**`
    - **componentDidUpdate** : 컴포넌트의 갱신이 일어난 직후에 호출됨. **`최초 렌더링에서는 호출되지 않음`** (shouldComponentUpdate 에서 false를 반환하면 호출되지 않음)
    - **componentWillUnmount** : 네트워크 요청 취소, 컴포넌트에 관련된 이벤트 리스너를 제거하는데 사용

- **가상돔이란**
  - 실제 돔을 메모리상에서 표현한 것이다.
  - 가상돔의 가장 큰 이점은 변화된 부분만 수정하는 원리 입니다. 즉 최소한의 수정을 최대한의 성능을 만들어 낸다
  - UI 표현은 메모리에 유지되고 렌더 함수와 화면을 표시하는 사이에 실제 돔과 동기화 된다. 이 단계를 `**reconciliation**`라고 한다.
  - reconciliation
    - JSX가 React.createElement로 바벨에 의해 트랜스파일링 됨
    - React.createElement 함수 호출에 의해 리액트 엘리먼트 트리가 반환됨
    - reconciliation 알고리즘에 의해 리액트 엘리먼트 트리를 재귀적으로 순회하면서 이전 트리와 현재 트리의 변경사항을 비교한다음 변경된 부분만 실제 돔에 반영한다.
  - 동작 방법
    - 데이터가 변경될 때 전체 UI는 가상돔안에서 리렌더링 된다.
    - 변경되기 전 DOM과 새로운 변경된 DOM의 변경점을 계산한다.
    - 계산이 완료되면 실제 DOM에서 계산된 변경점만 업데이트 한다.
- React Fiber
  - React 16에서 핵심 알고리즘 재구현함
  - 애니메이션, 레이아웃, 제스처등의 성능을 높힌다.
- Controlled 컴포넌트 vs UnControlled
  - Controlled
    - 사용자 입력을 기반으로 자신의 state를 관리하고 업데이트 한다. 즉 리액트에 의해 값이 제어 됨
    - 입력 요소를 제어하는 컴포넌트를 말한다.
    - 상태 변경에는 별도의 onChange 핸들러가 있다.
    - 폼을 제어할 때는 이것을 쓰는게 좋다. 그 이유는 unControlled 에서는 직접 DOM을 업데이트 하기 때문
  - UnControlled (⇒ useRef를 쓰는 방식)
    - 기존의 바닐라 자바스크립트와 방식이 크게 다르지 않다.
    - useState를 쓰지 않기 때문에 값이 실시간으로 동기화 되지않을 수 있다.
      - 왜 ref는 계속 같은 값을 가지고 있을까?
      - heap 영역에 저장되는 일반적인 객체이기 때문
      - 매번 렌더링할 때 동일한 객체를 제공한다. heap 에 있기 때문에 어플리케이션이 종료되거나 가비지 컬렉팅이 될 때까지 같은 값을 가진다.
      - 값이 변경되어도 주소값은 동일하기 때문에 변경을 감지할 수 없다!
    - 내부적으로 자기 자신의 state를 가지고 있음
    - 현재 값을 찾기 위해 ref을 사용한다.
- HOC
  - 컴포넌트를 받아서 새로운 컴포넌트를 리턴하는 함수이다.
  - 컴포넌트 로직을 재사용, 상속할 수 있음
- context (위에 있는 내용 참조)
  - 모든 레벨에 수동으로 props를 전달하지 않고 컴포넌트 트리를 통해 데이터를 전달할 수 있도록 제공한다.
  - 전역 상태관리로 사용할 수 있다.
- reconciliation - 41 (위에 있는 내용 참조)
  - 컴포넌트의 props, state가 변경될 때 리액트는 새로 반환된 컴포넌트를 이전에 렌더링 된 컴포넌트와 비교하여 DOM update가 필요한지 결정한다.
  - 두 개의 컴포넌트가 같지 않을 경우 리액트는 DOM을 업데이트 한다. 이것을 **reconciliation** 라고 한다.
- div vs fragment
  - fragment가 div보다 빠르다
  - 그 이유는 DOM node를 만들지 않기 때문에 메모리를 적게 사용한다.
- portals
  - 상위 컴포넌트의 DOM 계층 구조 외부에 존재하는 DOM 노드로 자식을 렌더하는데 권장되는 방법이다.
- stateless 컴포넌트
  - 내부에서 state를 사용하지 않는 컴포넌트
  - 컴포넌트의 크기가 작아지기 때문에 이해하기 쉬워 진다.
  - 테스트가 빠르고 쉽다
  - Presentation 컴포넌트라고 볼 수 있음
- 리액트의 장점
  - 가상DOM을 사용해 성능 향상
  - JSX는 코드를 작성하고 이해하기 쉽다
  - 클라이언트, 서버사이드 렌더링 둘다 가능하다
  - view 라이브러리이기 때문에 다른 프레임워크와 통합하기 쉽다.
- 리액트의 한계
  - 프레임워크가 아닌 라이브러리이다.
  - 러닝커브가 있다
  - 과도한 보일러플레이트 코드가 있을 수 있다. 함수형으로 오면서 많이 줄었다고 생각
- Suspense - [https://ko.reactjs.org/docs/react-api.html#reactsuspense](https://ko.reactjs.org/docs/react-api.html#reactsuspense)

  - 리액트 v18 에 정식으로 들어오게 된다.
  - 컴포넌트가 렌더링 되기 전까지 기다릴 수 있다.
  - 비동기 API 호출을 기다릴 수 있다.
  - 데이터 불러오기에 대한 구현이 아님. rest, graphql과 같은 특정한 데이터 형식, 라이브러리를 사용한다고 가정하지 않음
  - 데이터 불러오기 라이브러리들이 리액트와 결합할 수 있게 해준다.

  ```jsx
  // 이 컴포넌트는 동적으로 불러옵니다
  const OtherComponent = React.lazy(() => import("./OtherComponent"));

  function MyComponent() {
    return (
      // Displays <Spinner> until OtherComponent loads
      <React.Suspense fallback={<Spinner />}>
        <div>
          <OtherComponent />
        </div>
      </React.Suspense>
    );
  }
  ```

- ErrorBoundary

  - (문제점) 리액트는 렌더링시 오류가 발생하면 전체 화면 모두를 렌더링 하지 않는다. 그래서 이 때 오류가 발생했을 경우 에러 바운더리 클래스를 활용한다.
  - 하위 컴포넌트 트리 어디에서든 자바스크립트 에러를 발견하면 fallback 으로 지정해준 UI를 보여준다.
  - 클래스 컴포넌트에서 componentDidCatch 를 정의하면 된다.

  ```jsx
  import React, { Suspense } from "react";
  import MyErrorBoundary from "./MyErrorBoundary";

  const OtherComponent = React.lazy(() => import("./OtherComponent"));
  const AnotherComponent = React.lazy(() => import("./AnotherComponent"));

  const MyComponent = () => (
    <div>
      <MyErrorBoundary>
        <Suspense fallback={<div>Loading...</div>}>
          <section>
            <OtherComponent />
            <AnotherComponent />
          </section>
        </Suspense>
      </MyErrorBoundary>
    </div>
  );
  ```

- React DOM의 render 메소드의 목적
  - 제공된 컨테이너의 DOM에 리액트 엘리먼트를 렌더하고 컴포넌트에 대한 참조를 반환한다.
- 리액트에서 스타일 사용
  - CSS 문자열 대신 카멜 케이스 속성의 자바스크립트 객체 허용
  - 이것은 DOM style Javascript 속성과 일치하고, XSS 보안의 허점을 방지할 수 있다.
- 컴포넌트 대문자
  - JSX를 사용하여 컴포넌트를 렌더링 하는 경우 대문자로 써야 한다. 이렇게 하지 않으면 리액트는 알 수 없는 태그로 인식한다.
  - html과 svg 태그만 소문자로 시작할 수 있기 때문
- Flux
  - React 뿐만 아니라 **단방향** 데이터 흐름의 개념을 보완해주는 아키텍쳐 이다.
  - 액션이 발생하면 디스패쳐에서 이를 받아 해석하고 스토어의 정보에 변경을 가하고 그 결과가 다시 뷰로 전달되도록 한다.
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68e91531-f25a-4584-86bf-9fefaca060c7/Untitled.png)
- Redux
  - Flux 설계 패턴을 기반으로 한 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너 이다.
  - 단일출처 : 전체 어플리케이션의 상태는 하나의 스토어의 객체 트리에 저장 된다.
  - 읽기전용 상태 : 상태를 변경하는 유일한 방법은 액션을 보내는 것이다.
  - 순수함수 : 상태 트리가 액션에 의해 어떻게 변환될지 리듀서에 작성해야 한다. 리듀서는 순수 함수이고 다음 상태를 반환해준다.
    - 왜 순수 함수여야 하나? → 리덕스의 state는 불변해야 한다. 불변해야 한다는 뜻은 변경되면 안되는 것이 아니라 state가 직접 수정되면 안된다는 것이다. → state가 변경되었는지 확인하기 위해 주소값을 비교하기 때문이다.→ 그럼 왜 주소값을 비교하냐? 객체의 속성으로 비교하려면 재귀나 반복문을 돌면서 객체의 모든 속성을 다 확인해야 한다. 이 부분이 무거워 질 수 있기 때문이다.
  - 리듀서에서 액션을 보내는 것은 안티패턴 이다. 그 이유는 사이드 이펙트가 발생할 수 있기 때문 (순수함수가 깨진다?)
- Thunk - [https://ko.wikipedia.org/wiki/썽크](https://ko.wikipedia.org/wiki/%EC%8D%BD%ED%81%AC)
  - 특정 작업을 나중에 하도록 미루는..
  - 객체 대신 함수를 생성하는, 액션생성 함수를 작성할 수 있도록 도와준다.
  - 즉 액션 객체가 아닌 함수를 디스패치할 수 있기 해줌 (함수의 인자로 dispatch를 받는다.)
  ```jsx
  const thunk = (store) => (next) => (action) =>
    typeof action === "function"
      ? action(store.dispatch, store.getState)
      : next(action);
  ```

### 네트워크

- TCP UDP
  이 두 프로토콜은 OSI 7계층에서 전송계층에 속하는 전송 프로토콜이다.
  - TCP : 데이터가 반드시 전달되는 것을 보장하는 프로토콜
    - 연결지향으로 2개의 호스트가 통신을 하기 전 연결이 이뤄져야 한다.
    - 높은 신뢰성과 순서대로 전송하는 것을 보장한다.
      - 흐름 제어 : 송신자의 데이터 양을 조절
      - 혼잡 제어 : 네트워크 상황을 감지하고 송신자의 데이터 양을 조절한다.
      - 에러 감지 : 잘못 전송되었을 경우 재전송
    - 전 이중 방식으로 두 호스트 모두 송신자, 수신자가 될 수 있다.
    - 세그먼트 라는 단위의 패킷으로 쪼개서 보낸다.
    - HTTP, FTP, SMTP, TELNET 등에서 사용
    - 3-way handshaking
      - TCP가 호스트 간에 연결을 설정하는 방법이다.
      - 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정
  - UDP : 데이터의 신뢰성을 보장하지 않음
    - 신뢰성이 없고 데이터의 순서를 보장하지 않음
      - 에러 감지는 체크섬을 이용한 정도임
    - 패킷의 단위가 데이터 그램으로 경계가 분명하다. 그래서 수신자는 송신자가 보낸 그대로의 크기로 받게 된다.
    - TCP에 비해 하는일이 없기 때문에 속도가 빠르다
    - DNS, DHCP, 비디오, 오디오 스트리밍에 사용
- 양방향 통신 기법 Polling
  - Http 는 양방향이 되지 않는 모델이다.
  - 오직 클라이언트만이 서버로 연락할 수 있다.
  - 지금은 websocket으로 할 수 있었지만 과거엔 Polling을 썼다.
  - Polling은 나(서버)는 너(클라)를 계속 관찰하겠어 라는 원리임. 서버가 클라쪽에 n초 간격으로 물어본다. 그래서 실시간으로 주는건 안됨, 모두 부담되므로
  - Long Polling : 일단 보내고 타임아웃 될 때 까지 무한정 기다린다.
  - 웹소켓은 클라이언트와 서버가 양방향 통신을 하는 방식으로 실시간 통신 가능! http 에서 ws 프로토콜을 사용해야 한다.
- 동일출처
  - 프로토콜, 포트, 호스트가 같아야 동일 출처이다.
- Preflight 란
  - 서버에서 어떤 메서드와 어떤 헤더를 허용하는지 본 요청을 보내기전에 확인하는 요청이다.
  - 서버로 OPTIONS 메서드로 먼저 확인함
- HTTP
  클라이언트-서버 모델을 따르는 프로토콜로 TCP/IP 위에서 동작한다. 표준은 1.1 이고 2, 3이 등장하였다.
  - 비-연결 지향 (connectionless) - 1.0 기준
    - 클라이언트가 서버에서 리소스 요청 후 응답 받으면 연결을 끊는다. 그 이유는 연결 유지하면 서버에 많은 부담을 준다. 하지만 리소스를 요청할 때 마다 연결해야 하는 (문제점) 오버헤드 비용이 발생하여 (해결책) 요청헤더의 `**Connection: keep-alive**` 속성(1.1에 있는 속성)으로 지속적 연결 상태를 유지할 수 있다. 즉 요청마다 연결하지 않고 **`기존 연결 재사용`**! 1.1 부터는 이게 기본 상태이다.
  - 무상태성 (stateless)
    - 각각 요청이 독립적으로 여겨진다. 서버는 클라이언트의 상태를 유지하지 않음 그래서 클라이언트에 맞게 리소스를 응답하는 것은 불가능 하다. 이를 해결 하기 위해 쿠키나 세션 토큰 방식의 Oauth JWT가 사용됨
  - Method
    - 클라이언트가 서버에 요청방법을 정의하는 것이다.
    - GET : 서버에게 조회할 리소스를 요청 (조회)
    - POST : 서버에게 본문에 생성할 데이터를 삽입하여 전송 (생성)
    - PUT : 서버에게 본문에 수정할 데이터를 삽입하여 전송 (수정)
    - PATCH : PUT과 비슷하지만 일부만 수정함 (PUT은 전체 데이터 수정)
    - DELETE : 서버에게 삭제할 리소스 요청
  - 응답 상태 코드
    - 서버가 클라이언트에게 요청을 받으면 응답상태에 따라 다른 상태코드를 클라이언트에게 돌려준다.
    - 1xx (요청에 대한 정보) : 요청을 받았으면 작업을 계속한다.
      - 100 : 진행 중, 현재까지 진행상태에 문제 없음
      - 101 : 스위칭 프로토콜, 서버에서 프로토콜을 변경할 것임을 알려줌
      - 102 : 프로세싱, 서버가 요청을 수신하였으며 이를 처리하고 있지만 아직 제대로 된 응답 알려줄 수없음
    - 2xx (성공) : 요청을 성공적으로 수행했다.
      - 200 : 성공
      - 201 : 새 리소스 작성
      - 202 : 요청접수, 아직 처리는 안함
      - 204 : 노 컨텐츠 , 요청에 대해 보내줄 수 있는 컨텐츠가 없지만 헤더는 의미있을 수 있다.
      - 205 : 리셋 컨텐츠,
    - 3xx (리다이렉션) : 클라이언트가 요청을 마치기 위해 추가적인 동작을 해야 함
      - 300 : 여러개의 응답, 선택필요
      - 301 : 요청한 페이지가 영구적으로 이동됨
      - 302 : 임시이동
    - 4xx (클라이언트 오류) : 클라이언트에 오류 있음
      - 400 : bad request , 잘못된 문법으로 인해 서버가 이해할 수 없음
      - 401 : Unauthorized 권한 없음
      - 403 : Forbidden 금지됨, 리소스에 대한 권한없음
      - 404 : 찾을 수 없음
      - 405 : Method not allowed , 허용할 수 없는 메소드로 요청했음
    - 5xx (서버 오류) : 서버에 오류 있음
      - 500 : 내부서버 오류
      - 501 : 요청수행 기능 없음, 메서드 인식 불가
      - 502 : bad gateway
      - 503 : 서비스 사용 불가
  - 요청 헤더
    - Host : 서버 도메인 이름과 TCP 포트번호
    - Content-Type : POST/PUT 메소드를 사용할 때 본문의 타입
      - `Content-Type: application/x-www-form-urlencoded`
    - if-Modified-Since : 명시한 날짜 이후로 변경된 리소스만 획득
    - Origin : 요청이 어느 도메인 에서 왔는지 명시, 서버의 `Access-Control-*` 속성에 필요
    - Cookie : 서버의 Set-Cookie로 설정된 쿠키 값
  - 응답 헤더
    - Access-Control-\* : CORS를 허용하기 위한 웹사이트 명시
    - Set-Cookie : 클라이언트에 쿠키 설정
    - Last-Modified : 요청한 리소스가 마지막으로 변경된 시각
    - Location : 3xx 상태 코드일 때 리다이렉션 되는 주소
    - Allow : 요청한 리소스에 대해 가능한 메서드들
- HTTPS
  기존의 HTTP를 암호화한 프로토콜이다. 원래 SSL의 약자였지만 TLS로 이름이 바뀌고 혼용하고 있다.
  TCP의 연결이 이루어진 후 TLS를 통해 암호화 설정이 되고 통신을 하는 방식이다.
  - 공개키, 비밀키 : 공개키는 모두가 볼 수 있는 키, 비밀키는 소유자만 가지고 있는 키이고 암호화, 복호화에 사용됨
  - 대칭키 : 서버와 클라이언트가 암호화 복호화에 동일한 비밀키를 사용, 키 공유가 어렵지만 빠르다
  - 비대칭키 : 서버와 클라이언트가 암호화 복호화에 서로 다른 비밀키를 사용한다. 속도가 느림
  - 인증기관 (CA) : 클라이언트가 접속을 요청한 서버가 의도한 서버가 맞는지 인증해주는 역할
- HTTP 1.1 vs 2.0 - [https://medium.com/@shlee1353/http1-1-vs-http2-0-차이점-간단히-살펴보기-5727b7499b78](https://medium.com/@shlee1353/http1-1-vs-http2-0-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-5727b7499b78)
  - HTTP 1.1
    - 무겁고 중복이 많은 헤더 구조
    - 요청하고 응답이 와야(동기 느낌) 다음 요청을 할 수 있었는데 1.1에서 **`파이프라이닝 기법`**을 통해 응답받지 않고도 여러개의 요청을 보낼 수 있게 되었다. 하지만 처음 요청에 대한 응답이 오래걸리는 경우 그 다음 응답까지 시간이 지연되는 현상이 발생했다. **요청은 여러 개 막 보낼 수 있어도 응답이 와야 다음 요청을 보낼 수 있다.**
  - HTTP 2.0
    - 하나의 커넥션으로 동시에 여러개의 메세지를 주고받을 수 있고, 응답은 요청 순서에 상관없이 스트림으로 받는다.
    - 리소스간의 우선순위를 설정하여 브라우저가 리소스를 수신하는 순서를 적절하게 결정한다.
    - 브라우저가 요청하지 않으면 서버는 응답하지 않는 것이 보통이지만 요청한 HTML 문서에 리소스가 포함되어 있는 경우 서버가 브라우저에게 밀어주는 방식을 취하여 요청을 최소화 한다.
    - 헤더 압축하여 중복과 크기를 줄인다.
    - 요청을 여러 개 보낼 수 있고 응답도 오는 대로 받을 수 있다(비동기 느낌)
- URL URN URI

  - URI : 인터넷 상의 리소스를 고유하게 식별할 수 있는 식별자다
    - 위치를 알려주는 URL과 전 세계를 통틀어 고유한 이름을 의미하는 URN이 존재한다.
  - URL : 해당 위치에서 어떻게 리소스를 얻어낼 것인가에 대한 정보
  - URN : 리소스를 유일하고 영구적인 이름으로 식별하지만 인터넷상 위치는 알려주지 않음

  - **URL**
    - [http://example.com/mypage.html](http://example.com/mypage.html) (프로토콜: http)
    - ftp://example.com/download.zip (프로토콜: ftp)
    - mailto:[user@example.com](mailto:user@example.com) (프로토콜: mailto)
  - **URN**
    - urn:isbn:0451450523 (책을 식별하는 ISBN 번호)
    - urn:uuid:6e8bc430-9c3a-11d9-9669-0800200c9a66 (전 세계에서 유일한 번호)

- RESTFul
  - 슬래시를 통한 계층 관계
  - 명사로 표현
  - 파일 확장자는 포함하지 않는다.
- REST API
  - Representational State Transfer
  - REST : 웹 상에서 통신 체계에 있어 범용적인 스타일을 규정한 아키텍쳐 이다.
  - REST : 웹에 존재하는 모든 자원에 고유한 URI를 부여해 활용하는 방법
  - API : 카카오 지도, 구글 지도 API 와 같은기존에 있는 응용 프로그램을 통해 데이터를 제공받거나 기능을 사용하고자 할 때 사용하는 인터페이스 및 규격이다.
  - REST API : REST 원칙을 적용하여 서비스 API를 설계한 것을 말한다.
  - 특징
    - `**균등한 인터페이스**` (Uniform Interface) : 플랫폼이나 언어의 제약에 구애받지 않는다. 주로 JSON을 많이 사용하지만 XML도 가능하다.
    - **`무상태성`** (Stateless) : 서버는 클라이언트의 상태를 고려하지 않고 API 요청에 대해서만 처리 한다.
    - **`캐싱 가능`** : HTTP의 특징인 캐싱을 사용할 수 있다. GET 메소드를 Last-Modified 값과 함께 보낼 경우 컨텐츠의 변화가 없을 때 캐시된 값을 사용한다.
    - **`자체 표현성`** : 이 자체로 의미를 지니기 때문에 어떤것을 표현하는지 알아보기 쉽다.
    - **`클라이언트-서버`** 구조 : REST 서버가 API를 제공하기 때문에 클라이언트에서 처리하는 부분과 독립적이다. 서로 의존이 줄어듬
    - URI는 리소스를 표현해야 한다. 동사가 아닌 명사를 사용
    - 컬렉션은 복수 사용
    - **`행위는 HTTP Method`**로 표현한다
- 쿠키와 세션
  HTTP는 상태가 없는 프로토콜이기 때문에 사용자가 브라우저를 통해 특정 웹사이트에 접속하게 될 경우 어떤 사용자가 접속했는지에 대한 정보를 파악할 수 없어서 쿠키, 세션을 통해 사용자를 구분한다.
  - 쿠키 : 클라이언트의 웹 브라우저에 저장되는 작은 데이터 조각으로 서버가 클라이언트의 요청을 식별하는데 사용된다. 하지만 클라이언트가 직접 쿠키수정할 수 있고 해커가 탈취할 수 있기 때문에 보안에 취약하다.
    - 세션 ID 관리, 사용자 테마, 트래킹, 사용자 행동 분석 등을 저장
    - 클라이언트에 저장 (브라우저)
    - 브라우저가 꺼져도 삭제되지 않음, 사용자가 삭제하거나 정해진 시간만큼 유지
    - 문자열만 저장
    - 클라이언트에서 보내기 때문에 빠르다.
    - 보안에 취약
    - HttpOnly 속성을 사용하면 document.cookie 에 바로 접근할 수 없다.
  - 세션 : 브라우저가 서버에 연결되어 있는 동안 유지하는 데이터, 서버에서 관리한다.
    - 사용자가 웹 사이트에 방문하여 서버에 요청을 보내면 사용자의 정보를 서버에 저장하고 그 정보를 식별할 수 있는 세션 ID를 Set-Cookie 헤더로 클라이언트에게 전송한다.
    - 그럼 쿠키로 세션 ID를 관리하고 해당 서버에 요청할 때 마다 쿠키 헤더에 세션 ID를 포함시켜 전송하기 때문에 서버는 클라이언트를 식별할 수 있다.
    - 민감정보, 사용자의 비밀번호 및 개인정보를 식별한다.
    - 서버에 저장
    - 문자열 뿐 아니라 객체도 저장
    - 서버쪽에서 처리하기 때문에 느리다.
    - 서버에서 민감 정보(ex. 세션 아이디 로그인할 때마다 달라지는 JWT 토큰)를 가지고 있어 보안에 좋다.
- 브라우저에 URL 입력하면 - [https://mindnet.tistory.com/entry/네트워크-쉽게-이해하기-22편-TCP-3-WayHandshake-4-WayHandshake](https://mindnet.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-22%ED%8E%B8-TCP-3-WayHandshake-4-WayHandshake)
  1. 브라우저에 URL 입력
  2. 브라우저가 URL을 해석하고 문법에 맞지 않으면 기본 검색엔진으로 검색
  3. 문법에 맞으면 URL의 호스트 부분을 인코딩 한다.
  4. HTTPS 혹은 HTTP로 요청한다
  5. DNS를 조회하여 해당 도메인에 IP가 있는지 확인한다.
  6. 없으면 OS에게 DNS 서버에 요청하라고 지시하고 있으면 DNS 서버는 해당 도메인의 IP를 돌려준다.
  7. TCP 소켓을 열고 3-way handshake로 연결을 설정한다.
  8. HTTPS 요청이면 TLS 과정을 통해 세션키 생성
  9. 브라우저는 응답받은 HTML CSS JS 등의 리소스를 사용하여 렌더링한다.
  10. 서버와 세션이 종료되면 4-way handshake로 연결을 종료한다.
- CDN (컨텐츠 전송 네트워크)
  **`웹사이트를 구성하는 수많은 리소스들이 있을 때 어떻게 최적화 하는가`**? 바로 CDN을 사용하는 것이다. 즉, 수십개의 이미지로 구성되어 있을 때 용량이 상당하기 때문에 동일한 웹 서버에 요청해서 이를 가져오면 사이트 로딩 속도가 매우 느려지는데 이를 CDN으로 해결 한다.
  컨텐츠를 전달하는 네트워크를 구성하는 것이다.
  사용자가 접속한 위치에서 가장 가까운 서버에 리소스를 캐싱해두고 빠르게 가져오는 기법이다.
  - 장점
    - 리소스를 캐싱하기 때문에 빠르다.
    - 1개의 웹서버에서만 리소스를 가져오지 않기 때문에 서버의 부하 줄어듬
  - 단점
    - 별도의 서버를 구축해야 하므로 비용이 든다.
    - 해외 CDN을 사용할 경우 더 느려질 수도 있다.
- 리프레시 토큰

### 보안

- CORS
  Same-Origin Policy (동일 출처 정책)
  어떠한 문서나 스크립트가 다른 `**프로토콜 / 포트 / 호스트**` 이것들을 `**출처**` 라고 한다.에 있는 리소스 사용을 제한하는 정책이다.
  만약 `http://website.com/ex/ex.html` 이 사이트에서 다른 곳으로 요청해본다고 가정하자.
  | 리소스 요청 | 허용 여부 |
  | -------------------------------- | ---------------------- |
  | http://website.com/ex/ | 성공 |
  | http://website.com/ex1/ | 성공 |
  | http://website.com:81/ex/ex.html | 실패 (포트가 다름) |
  | http://wwebsite.com/ex/ | 실패 (호스트가 다름) |
  | https://website.com/ex/ex.html | 실패 (프로토콜이 다름) |
  - CORS - HTTP 헤더를 사용하여 클라이언트가 서버로 하여금 서로에 대해 인지하고 한 출처에서 다른 출처의 자원을 사용할 수 있게 하는 매커니즘이 바로 CORS이다. HTTP 요청을 보낼 때 HTTP 헤더의 `Origin` 속성에 자동으로 값이 할당된다.
  - 응답헤더에 `Access-Control-Allow-Origin` 에 요청을 허용하는 도메인을 명시한다.
- XSS, CSRF
  - XSS (Cross Site Scripting) 사이트간 스크립팅
    - 웹사이트 관리자가 아닌 사람이 웹사이트에 악성 스크립트를 삽입할 수 있는 취약점
    - 저장 XSS : 취약점이 있는 서버에 스크립트를 저장시켜 해당 웹사이트를 요청하는 사용자로 하여금 스크립트를 실행하게 한다. (게시글 내용에 악성 스크립트 저장)
    - 반사 XSS : 악성 URL을 배포하여 클릭하도록 유도
    - DOM 기반 XSS : 공격 스크립트가 DOM 생성의 일부로 실행되면서 공격하는 기법으로 악성 URL을 배포하여 클릭하도록 유도 (innerHTML)
    - XSS를 사용하여 사용자의 쿠키 정보 및 세션 ID를 획득할 수 있다. 시스템 관리자 권한 확득할 수 있다
    - 대응법
      - 입/출력 값 검증 무효화 : 기본적으로 <script> 태그를 사용하니 `<`를 `&lt;` 로 바꾼다.
      - 보안 라이브러리 사용
      - httpOnly 옵션 (서버에서)
  - CSRF (Cross Site Request Forgery) 사이트간 요청 변조 - [https://forested-bird-156.notion.site/XSS-CSRF-df6a4ef37f594435b3094ca699d2d7c0](https://www.notion.so/df6a4ef37f594435b3094ca699d2d7c0)
    - 사용자가 의도치 않게 공격자가 의도한 행동을 하여 취약점 노출, 수정 삭제 생성등을 하게 함
    - 어느 사이트에 접속해있다.
    - 이 사이트에서는 `[http://www.mybank.com/transfer?to=<계좌번호>&amount=<액수>](http://www.mybank.com/transfer?to=<계좌번호>&amount=<액수>)` 의 형태로 요청을 보낸다.
    - 어떤 악성 사이트에 접속하면 위의 요청을 실행하는 CSRF 스크립트를 실행하게 해서 돈을 보내게 한다.
    - 즉 사용자가 의도하지 않은 행동을 한다.
    - 대응법
      - referrer 검증 : 요청헤더에 있는 `referrer` 속성을 검증하여 신뢰할 수 있는 도메인에서 들어오는 요청인지 검증한다.
      - CSRF 토큰 : 난수를 서버쪽 사용자 세션에 저장하고 요청할 때 난수를 CSRF 토큰으로 지정하여 사용자에게 전송한다. 이후 요청부터 토큰이 일치하는지 확인
      - 캡챠 사용 : 사용자와 상호작용을 통해 숫자 문자를 입력하여 검증

### Next.js

- getStaticProps, getServerSideProps 차이
  - getStaticProps
    - 정적 생성에 사용
    - 클라이언트 사이드에서 실행되지 않는다.
    - 빌드시 고정되는 값으로 빌드 이후에 변경 불가함
    - 블로그
  - getServerSideProps
    - 서버사이드 렌더링에 사용되고 빌드와 상관없이 매 요청마다 데이터를 서버로부터 가져옴
  - 가장 큰 차이점
    - 빌드 이후 데이터 변경 가능 여부이다.

## 인성 면접

## 좋은 링크들

- https://github.com/kesuskim/Front-End-Checklist
- [https://github.com/appear/reactjs-interview-questions-ko#what-are-the-limitations-of-react](https://github.com/appear/reactjs-interview-questions-ko#what-are-the-limitations-of-react)
- [https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/frontend/csr-ssr.md](https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/frontend/csr-ssr.md)
- [https://bboglebbogle.tistory.com/40?category=1011421](https://bboglebbogle.tistory.com/40?category=1011421)
