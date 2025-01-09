---
tags:
  - jQuery
---


1. Ajax(비동기 자바스크립트 및 XML)는 빠르고 동적인 웹 페이지를 만들기 위한 기술입니다. Ajax를 사용하면 전체 페이지를 새로 고치지 않고도 웹 페이지의 일부를 업데이트할 수 있습니다. Ajax로 이벤트를 처리하면 대화형 및 반응형 웹 페이지를 만들 수 있습니다.
2. onreadystatechange: 이 이벤트는 XMLHttpRequest 객체의 readyState 속성이 변경될 때마다 트리거됩니다. readyState 속성은 다음 값 중 하나를 가질 수 있습니다
    1. 0(초기화되지 않음), 1(로드 중), 2(로드됨), 3(대화형), 4(완료됨).
3. onload: 요청이 성공적으로 완료되면 이 이벤트가 트리거됩니다. onreadystatechange 이벤트와 유사하지만 readyState 속성이 4(완료)일 때만 트리거됩니다.
4. onerror: 이 이벤트는 요청에 오류가 있을 때 트리거됩니다.
5. onprogress: 이 이벤트는 요청이 진행 중일 때 트리거되며 요청의 진행 상황을 추적할 수 있습니다.
6. onabort: 요청이 취소되면 이 이벤트가 트리거됩니다.
7. ontimeout: 요청이 완료되는 데 지정된 시간보다 오래 걸리면 이 이벤트가 트리거됩니다.
8. onloadstart: 요청이 시작될 때 이 이벤트가 트리거됩니다.
9. onloadend: 요청이 완료되면(성공 또는 실패) 이 이벤트가 트리거됩니다.
10. 또한 $.ajaxStart(), $.ajaxStop(), $.ajaxComplete(), $.ajaxError(), $.ajaxSuccess() 등과 같은 jQuery 이벤트 핸들러를 사용하여 ajax 호출과 관련된 이벤트를 처리할 수도 있습니다.