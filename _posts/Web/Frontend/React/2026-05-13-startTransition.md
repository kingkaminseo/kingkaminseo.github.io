---
title: React StartTransition 제대로 이해하기
date: 2026-05-13
categories: [Web, Frontend, React]
tags: [RSC, React]
---

## StartTransition란?
startTransition는 Jank상황과 같은 무거운 렌더링 작업에서 작업 우선순위를 매겨 최적화할 수 있게 도와주는 React Hook이다.

  
  
> **Jank란?**  
메인 스레드가 너무 오래 걸리는 작업으로 인해 차단될 때 인터페이스가 끊기거나 반응이 느리게 느껴지는 현상을 말한다.  
{: .prompt-tip }

StartTransition을 정확히 이해하기 위해 React렌더링(상태 업데이트) 방식을 이해해보자.

### React 렌더링 (상태 업데이트) 방식
setState를 호출하면 리액트는 렌더링을 예약한다.  
렌더링이란 리액트가 컴포넌트 함수를 실행하고, 새로운 요소 트리를 생성한 뒤, 이를 이전 트리와 비교하여 차이점을 DOM에 적용하는 과정을 의미한다. 
이는 "재조정(reconciliation)"이다.


**React에서 모든 상태 업데이트는 Urgent라인으로 간주되어 모든 업데이트를 동일하게 취급한다.**    

업데이트는 순서대로 처리되며, 각 업데이트가 완료되어야만 다음 업데이트가 시작된다.
