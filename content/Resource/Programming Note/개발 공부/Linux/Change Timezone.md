---

```
apt-get install -y rdaterdate -s time.bora.net
```

시간을 동기화 해줘도 맞지 않는 경우는 UTC, KST 설정 문제인 경우가 많다.

  

변경하는 방법

```
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```