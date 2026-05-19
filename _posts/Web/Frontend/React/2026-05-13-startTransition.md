---
title: React StartTransition 제대로 이해하기
date: 2026-05-13
categories: [Web, Frontend, React]
tags: [RSC, React]
---

## StartTransition란?

> startTransition을 사용하면 UI의 일부를 백그라운드에서 렌더링할 수 있다.  -React-

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

### startTransition의 중요한 역할
startTransition을 사용하여, Urgent라인에 있는 상태를 Background에서 상태를 업데이트 할 수 있다.

이후 Urgent라인에 중요한 작업이 들어오면 백그라운드에 있는 상태를 중지할 수 있다.

**ex:**
- Urgent: 유저와 가장 직접적인 입력, 클릭, 키 입력 등
- Backgound: 큰 목록 업데이트, 차트 업데이트, 큰 렌더링 등 

## 사용예시
```tsx
import { useState, useTransition } from "react";

function Search() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  function handleChange(e) {
    // 긴급. 입력을 즉시 업데이트합니다.
    setQuery(e.target.value);

    // 긴급하지 않음. 리액트가 시간이 될 때 필터링된 목록을 업데이트합니다.
    startTransition(() => {
      setResults(filterItems(e.target.value));
    });
  }

  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending && <span>Filtering...</span>}
      <ItemList items={results} />
    </div>
  );
```

## StartTransition vs Debouncing

이러한 문제를 Debouncing을 사용하여 해결할 수 도 있다.
하지만 디바운싱의 경우 500ms동안 요청 지연을 할 경우  유저가 마지막으로 입력을 멈춘 후 인위적 500ms지연을 발생시킨 후 결과를 조회한다.

하지만 StartTransition은 추가지연 없이 라인으로 구분하여 렌더링 작업을 하기 때문에 훨씬 효율적이다.

각 상황에 맞게 사용하면 될 거 같다.

## requestIdleCallback  
사람들이 자주 비교하는 또 다른 개념이다.  
리액트 외부에서 작업을 스케줄링하기 위한 훅이다.   
리액트의 렌더링과는 전혀 연동되지 않는다.  
리액트는 이 기능을 인식하지 못하며, 리액트의 렌더링을 중단시킬 수도 없다.  