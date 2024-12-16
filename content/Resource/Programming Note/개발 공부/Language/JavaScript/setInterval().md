---

1. setInterval()은 지정된 시간 간격으로 제공된 함수를 반복적으로 실행하는 내장 자바스크립트 함수입니다. 주기적인 프로세스, 애니메이션 또는 기타 시간 기반 작업을 만드는 데 자주 사용됩니다.
2. setInterval() 함수는 두 개의 인수를 받습니다:
3. 각 간격마다 실행할 함수, 제공된 함수가 실행되어야 하는 시간 간격(밀리초)입니다.

```
setInterval(function, interval)
```

- 함수 매개변수는 각 간격마다 실행할 함수의 이름입니다. 기존 함수에 대한 참조이거나 즉석에서 정의한 익명 함수일 수 있습니다.
- 간격 매개변수는 제공된 함수가 실행되어야 하는 시간 간격(밀리초)입니다. 예를 들어 간격을 1000으로 설정하면 함수가 매초마다 실행됩니다.
- setInterval() 함수는 clearInterval() 함수로 간격을 중지하는 데 사용할 수 있는 ID를 반환합니다. 나중에 간격을 중지하려면 이 ID를 **변수에 저장**해야 합니다.
- 다음은 setInterval()을 사용하여 5초마다 함수를 실행하는 방법의 예입니다:

```
function logMessage() {  console.log('Executing function...');}const intervalID = setInterval(logMessage, 5000);
```

- 이 예제에서는 logMessage() 함수가 5초마다 실행되고 간격 ID가 intervalID 변수에 저장됩니다. 간격을 중지하려면 clearInterval() 함수를 호출하고 간격 ID를 전달하면 됩니다:

```
clearInterval(intervalID);
```

- 이렇게 하면 간격을 중지하고 지정된 간격에 함수가 실행되지 않도록 합니다.