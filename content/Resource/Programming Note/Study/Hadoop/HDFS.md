---
tags:
  - bigdata
---

> [!info] HDFS 이해(2) - HDFS 사용자 커맨드  
> version .bash_profile 수정 export HADOOP_HOME=/Users/groom/Platform/hadoop-3.3.0 export PATH=$HADOOP_HOME/bin:$PATH 수정 후 source ~/.bash_profile 적용 hadoop version 명령어를 통해 버전 확인 가능 -mkdir, -ls mkdir와 ls는 기본 리눅스 명령어와 동일 hadoop fs -ls / hadoop fs -mkdir /tmp hadoop fs -ls -R / (특정 디렉토리의 하위 디렉토리를 모두 보여준다) -tail `hadoop fs -tail  
> [https://velog.io/@dnstlr2933/HDFS-%EC%9D%B4%ED%95%B42-HDFS-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%BB%A4%EB%A7%A8%EB%93%9C](https://velog.io/@dnstlr2933/HDFS-%EC%9D%B4%ED%95%B42-HDFS-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%BB%A4%EB%A7%A8%EB%93%9C)