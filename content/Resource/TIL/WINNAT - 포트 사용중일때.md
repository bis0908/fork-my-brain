---
tags:
  - TIL/winnat
---

This worked for me on windows pc. This one is for those are not seeing the port when you run this command netstat -a -o -n on your command prompt.


**Open your command prompt in administrator mode and run this command**
```shell
net stop winnat
```

you'll get this response:
```Shell
The Windows NAT Driver service was stopped successfully.
```

Them you run this next:
```shell
net start winnat
```


then you will get this response:
```shell
The Windows NAT Driver service was started successfully.
```

once you do that. Start the react server and it would work. Same too if your backend server doesn't run on 3000