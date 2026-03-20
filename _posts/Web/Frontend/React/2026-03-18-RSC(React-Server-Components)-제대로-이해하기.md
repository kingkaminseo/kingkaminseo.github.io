---
title: RSC(React-Server-Components) 제대로 이해하기
date: 2026-03-19
categories: [Web, Frontend, React]
tags: [RSC, React]
---

## RSC(React-Server-Components)란?
React 18에서 발표된 기능으로,  
컴포넌트를 클라이언트가 아닌 서버에서 렌더링하는 방식이다. 
서버에서 컴포넌트를 실행해 결과를 직렬화된 UI 트리 형태로 생성한 뒤, 클라이언트에 전달한다.

이 과정에서 해당 컴포넌트는 브라우저에서 실행되지 않고 클라이언트는 단순히 렌더링된 결과를 받아 표시하는 역할만 수행하기 때문에,    
클라이언트 측 부담이 줄고 불필요한 렌더링 로직을 제거할 수 있어 애플리케이션의 성능이 최적화된다.
  

**React 공식 문서 예시**  
server-component
```js
import marked from 'marked'; // Not included in bundle
import sanitizeHtml from 'sanitize-html'; // Not included in bundle

async function Page({page}) {
  // NOTE: loads *during* render, when the app is built.
  const content = await file.readFile(`${page}.md`);

  return <div>{sanitizeHtml(marked(content))}</div>;
}
```

client
```html
<div><!-- html for markdown --></div>
```
다음과 같이 클라이언트는 렌더링된 출력결과만 받게 된다.  



## RSC의 핵심 특징
- **번들 크기 제로 (Zero Bundle Size)**
  - 서버 컴포넌트의 코드는 클라이언트로 전송되지 않기 때문에 번들 크기가 줄어들고, 초기 로딩 속도를 크게 개선할 수 있다.

> 서버에서 컴포넌트를 실행하기 때문에
`서버 부하 및 응답 시간 증가에 유의`해야 한다
{: .prompt-tip }
- **데이터베이스 직접 접근**
  - 서버에서 실행되므로 API를 거치지 않고 DB, 파일 시스템 등 서버 리소스에 직접 접근할 수 있다

### 주의해야 할 점
- 데이터를 직렬화로 내려주기 때문에 RSC에서 useState, useEffect등 클라이언트에서 사용되는 훅이나 onClick이나 onFocus와 같은 이벤트 핸들러를 사용할 수 없다.
- 서버에서 컴포넌트를 실행하기 때문에
`서버 부하 및 응답 시간 증가에 유의`해야 한다

## SSR과 비슷해보이는데 차이점이 있나요?
SSR은 서버에서 완성된 HTML을 생성해 클라이언트에 전달한다.  
반면 RSC는 서버에서 컴포넌트를 실행한 결과를 **RSC Payload**(직렬화된 데이터 형태)로 전달한다.  

또한 SSR은 클라이언트에서 JavaScript를 다시 실행해 이벤트를 연결하는 **하이드레이션** 과정이 반드시 필요하다.  
RSC는 서버 컴포넌트 자체는 하이드레이션이 필요 없지만,  
함께 사용되는 Client Component는 여전히 하이드레이션이 수행된다.  

## + 취약점 이슈

React및 Next.js에서 취약점 CVE-2025-55182, CVE-2025-66478 이 발표되었다.

이 취약점은 인증 없이 악의적인 요청 한번으로 서버에서 임의 코드를 실행할 수 있는 원격코드 실행 이슈로 위험도가 가장 높은 CVSS10.0등급으로 분류 하였다.

이에 Vercel은 이를 Critical 수준의 이슈로 분리하였다.
React및 Next.js의 버전 업데이트를 권장하였다.

React팀은 서버가 처리하는 일부 입력 데이터가 충분히 검증되지 않은 상태로 역직렬화되고 있었다는 점이 이번 취약점의 핵심 원인으로 지목되었다.








**관련 공식 문서**  
[React 공식 RSC 문서](https://ko.react.dev/reference/rsc/server-components)    
[Vercel 보안 공식 문서](https://vercel.com/kb/bulletin/react2shell)    
[React팀 공식 취약점 문서](https://react.dev/blog/2025/12/03/critical-security-vulnerability-in-react-server-components)     


**관련 참고 문서**  
[https://knightk.tistory.com/432](https://knightk.tistory.com/432)   
[https://jiohjung-dev.tistory.com/81](https://jiohjung-dev.tistory.com/81)  
[https://velog.io/@minsang9735/RSC](https://velog.io/@minsang9735/RSC)   
[https://kangs-develop.tistory.com/7](https://kangs-develop.tistory.com/7)  
[https://ko.react.dev/reference/rsc/server-components](https://ko.react.dev/reference/rsc/server-components)    
[https://www.ahnlab.com/ko/contents/content-center/36033](https://www.ahnlab.com/ko/contents/content-center/36033)  
