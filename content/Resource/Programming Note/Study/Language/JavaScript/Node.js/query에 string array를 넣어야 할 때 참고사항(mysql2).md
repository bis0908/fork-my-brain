---
tags:
  - Nodejs
---


- 바로 코드부터 보자

```js
export async function checkUnsubscribe(idList) {
  assert(Array.isArray(idList), "idList is not an array");
  const UnsubscribeIdList = idList.map((id) => `'${id}@naver.com'`).join(",");
  const query = `  select distinct substring_index(email, '@', 1) as id  from mail_unsubscribe  where email in (?)`;
  try {
    const [rows, fields] = await pool.query(query, [UnsubscribeIdList]);
    // console.log("rows: ", rows);    
    return rows;
  } catch (error) {
    logger.error("checkUnsubscribe(): Error querying database:" + error);
    return false;
  }
}
```

- SQL Injection을 대비하기 위해서 위와 같이 사용했었다.
- 쿼리문의 ? 에 내가 원하는 값을 대입하는 방식이었는데, 쉼표가 포함된 긴 문자열은 ? 하나에 대입이 되지 않았다. 그래서 아래와 같이 수정했다.

```js
export async function checkUnsubscribe(idList) {
  assert(Array.isArray(idList), "idList is not an array");
  const placeholders = idList.map(() => "?").join(",");
  const values = idList.map((id) => `${id}@naver.com`);
  const query = `SELECT DISTINCT SUBSTRING_INDEX(email, '@', 1) as id    FROM mail_unsubscribe    WHERE email IN (${placeholders})  `;
  
  try {
    const [rows, fields] = await pool.query(query, values);
    // console.log("rows: ", rows);    
    return rows;
  } catch (error) {
    logger.error("checkUnsubscribe(): Error querying database:" + error);
    return false;
  }
}
```

- 위 처럼 placeholder를 사용해 array 길이만큼 동적으로 ? 을 만들어주고 query 뒤에 values 배열을 넣어주면 값 하나당 ? 하나로 대응되어 대입이 제대로 된다.
- 만약 쿼리 대입문이 별개로 나눠진다면? (단일 값 ‘?’ 하나와 다른 값 ‘?’이 배열로 들어온다면?

```js
const placeholders = idList.map(() => "?").join(",");
const query = `SELECT id  FROM mail_sendlist  WHERE agent_no = ?  AND id IN (${placeholders})`;
const values = [agent_no, ...idList];

try {
  const [rows, fields] = await pool.query(query, values);
  if (rows.length > 1) {
    return rows;
  } else {
    return false;
  }
} catch (error) {
  logger.error("realTimeIdCheck(): Error querying database:" + error);
  return false;
}
```

- spread 문법으로 하나의 배열로 합친 최종 배열을 쿼리에 던져주면 된다.
- **단, 넘어오는 모든 파라미터 값을 사용한다는 전제.**