---
tags:
  - Html
---


1. html에서 표 만들때 사용하는 태그 `<table>`
2. `<tr>` : table row
3. `<td>` : 열 만들때 사용
4. `<th>` : 제목용 세로 열 만들때 사용
5. `<thead>`, `<tbody>` : 제목행, 일반행
6. `<table>` 태그는 기본적으로 사이사이 틈이 존재함
    
    1. 틈을 없애려면?
    
    ```
    table {	border-collapse: collapse;}
    ```
    
7. 셀 안의 요소 상하정렬
    1. `vertical-align`
        1. `inline` , `inline-block` 요소간 세로정렬 시 사용 가능함
            1. 폭과 너비가 없이 **항상 옆으로** 채워지는 요소들
        2. `<table>` 안에서 세로 정렬할때 사용 (`top` , `middle` , `bottom` )
8. `<div>` 로 테이블 만드는 방법 (만질일은 없음)
    1. `style="display: table"` 속성 넣어두면 가능
        1. `row` : `display: table-row`
        2. `column` : `display: table-cell`