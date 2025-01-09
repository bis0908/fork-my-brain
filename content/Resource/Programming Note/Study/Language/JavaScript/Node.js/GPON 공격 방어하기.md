---
tags:
  - 취약점
---


1. 배경: AWS에서 돌아가는 웹서비스의 Ubuntu 서버 로그에서 이상한 URI Error를 발견했고 구글링 해보니 **CVE-2021-41773 취약점 공격**이라는 걸 알게 되었다.

```
URIError: Failed to decode param '/cgi-bin/.%%%%32%%65/.%%%%32%%65/.%%%%32%%65/.%%%%32%%65/.%%%%32%%65/bin/sh'
```

- 좀 더 찾아보니 GPON 공격이라는걸 알게 되었고 방어할 수단을 찾아보게 되었다.
- 나는 이 특정 경로로 post를 보내는 ip에 대해 Ubuntu 방화벽에 등록해 차단해버리는 방법을 시도해봤다. 잘 막히는지는 결과를 봐야 할 것 같다.

```js
const {
  exec
} = require('child_process');
const rateLimit = require('express-rate-limit');
// ...
// create a rate limiter middlewareconst 
limiter = rateLimit({
  windowMs: 60 * 1000,
  // 1 minute    
  max: 10,
  // limit each IP to 10 requests per windowMs    
  message: 'Too many requests from this IP, please try again later'
});
app.post('/GponForm/diag_Form', limiter, (req, res) => {
  // your regular request handling code goes here    
  logger.warn('gpon router vulnerability detected');
  logger.warn('req.ip: ' + req.ip);
  exec(`sudo iptables -A INPUT -s ${req.ip} -j DROP && sudo sh -c 'iptables-save > /etc/iptables/rules.v4'`, (err, stdout, stderr) => {
    if (err) {
      // node couldn't execute the command            
      return;
    }
    // the *entire* stdout and stderr (buffered)        
    console.log(`stdout: ${stdout}`);
    console.log(`stderr: ${stderr}`);
  });
  logger.info('req.route: ' + req.route);
  res.status(404).send('Page not found');
});
```

- iptables는 서버를 재부팅하면 정보가 휘발된다고 해서 iptables-persistent라는 패키지도 우분투에서 설치했다.

```
sudo apt-get install iptables-persistent
```