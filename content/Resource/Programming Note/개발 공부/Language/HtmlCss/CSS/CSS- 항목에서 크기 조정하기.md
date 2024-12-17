---
tags:
  - css
---


1. px, %, viewport 등의 개념 정리

> [!info] CSS 에서 항목 크기 조정 - Web 개발 학습하기 | MDN  
> 지금까지 다양한 수업에서는 CSS 를 사용하여 웹 페이지의 항목 크기를 조정하는 여러가지 방법을 살펴 보았습니다. 디자인의 다양한 기능이 얼마나 큰지 이해하는 것이 중요하며, 이 수업에서는 CSS 를 통해 요소의 크기를 결정하는 다양한 방법을 요약하고 크기 조정과 관련하여 향후 도움이 될 몇 가지 용어를 정의합니다.  
> [https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Sizing_items_in_CSS](https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Sizing_items_in_CSS)  

> [!info] 반응형 웹 뚝딱 만들기 (1) - 뷰포트 메타태그와 미디어 쿼리  
> 이 글은 공동 기술 블로그(tech.yeon.me)에도 올린 글입니다. (여기에서도 숨겨진 좋은 글을 발견할지도 몰라요!) 프롤로그 모바일 사용자가 점점 늘어나는 요즘 반응형으로 만든 웹 사이트를 쉽게 찾아볼 수 있습니다. 빠트릴 수 없는 기술이 된 만큼, 오늘은 반응형을 만들기 위한 몇 가지 방법들을 정리해봤습니다 🤘 이 글에서는 반응형을 위한 기본인 뷰포트와 미디어 쿼리를 다루며, 다음 글에서는 CSS 속성을 활용해 빠르게 반응형을 만드는 법을 다룰 생각입니다.  
> [https://nykim.work/84](https://nykim.work/84)  

> [!info] 반응형 웹 뚝딱 만들기 (2) - vw, vh, vmin, vmax, em, rem 속성  
> 프롤로그 지난 글에는 반응형을 위해 필요한 뷰포트 메타태그와 미디어 쿼리에 대해 다뤘었는데, 이번에는 CSS 속성을 통해 좀 더 편하고 쉽게 반응형을 만드는 방법을 알아보려고 합니다 🤟 히어뤼고! 이 시리즈의 이전 글: 반응형 웹 뚝딱 만들기 (1) 1. vw / vh CSS 작업을 할 때 주로 사용하는 단위는? 픽셀(px)이요!  
> [https://nykim.work/85](https://nykim.work/85)  

```
.box {  width : 16px; /* 기본 px 단위 */  width : 1.5rem; /* html태그 혹은 기본 폰트사이즈의 1.5배 */  width : 2em; /* 내 폰트사이즈 혹은 상위요소 폰트사이즈의 2배 */  width : 50vw; /* 브라우저(viewport) 화면 폭의 50% */  width : 50vh; /* 브라우저(viewport) 화면 높이의 50% */}
```