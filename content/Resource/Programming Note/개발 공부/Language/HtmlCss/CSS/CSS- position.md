---
tags:
  - css
---


어쩌다보니 내용 길어져서 페이지 만듬;;

- `position: static` 좌표 이동 X
- `position: fixed` 현재 화면을 기준으로 고정함. (viewport)
- `position: absolute` 내 부모 태그중 `position: relative` 가진 부모를 기준으로 고정
    - `top`, `bottom` 적용하면 부모 태그 기준으로 위치를 움직이게 할 수 있음 ㅇㅇ
- `position: absolute` 요소 가운데 정렬하려면

```
left: 0;right: 0;margin: auto; <!-- auto만 할당해줘도 가운데 정렬 되는듯? -->
```

### (`position` ) 속성 부여하게 되면

1. 좌표 이동 가능해짐
    - `relative` : 내 원래 위치 기준
    - `absolute` : `relative` 태그를 가진 부모 기준
    - `fixed` : 현재 화면에 고정
2. ==공중에 뜨게 됨==