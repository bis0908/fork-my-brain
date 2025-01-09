---
tags:
  - linux/Centos
---


[https://wnw1005.tistory.com/289](https://wnw1005.tistory.com/289)

  

![[opengraph.png]]

## 개별 압축 백업

### 기존 저장소 파일 확인

```
[study@localhost ~]$ ls /etc/yum.repos.dCentOS-AppStream.repo  CentOS-Extras.repo      CentOS-Vault.repoCentOS-Base.repo       CentOS-Media.repo       CentOS-centosplus.repoCentOS-CR.repo         CentOS-PowerTools.repo  CentOS-fasttrack.repoCentOS-Debuginfo.repo  CentOS-Sources.repo[study@localhost ~]$
```

### 저장소 파일 개별 압축 백업

```
[study@localhost ~]$ sudo bzip2 /etc/yum.repos.d/CentOS-*.repo[sudo] study의 암호:[study@localhost ~]$
```

### 저장소 파일 개별 압축 백업 후 확인

```
[study@localhost ~]$ ls /etc/yum.repos.dCentOS-AppStream.repo.bz2  CentOS-PowerTools.repo.bz2CentOS-Base.repo.bz2       CentOS-Sources.repo.bz2CentOS-CR.repo.bz2         CentOS-Vault.repo.bz2CentOS-Debuginfo.repo.bz2  CentOS-centosplus.repo.bz2CentOS-Extras.repo.bz2     CentOS-fasttrack.repo.bz2CentOS-Media.repo.bz2[study@localhost ~]$
```

## 확장자 변경 백업

```
[study@localhost ~]$ cd /etc/yum.repos.d[study@localhost yum.repos.d]$ sudo rename -v .repo .bak *.repo[sudo] study의 암호:`CentOS-AppStream.repo' -> `CentOS-AppStream.bak'`CentOS-Base.repo' -> `CentOS-Base.bak'`CentOS-CR.repo' -> `CentOS-CR.bak'`CentOS-Debuginfo.repo' -> `CentOS-Debuginfo.bak'`CentOS-Extras.repo' -> `CentOS-Extras.bak'`CentOS-Media.repo' -> `CentOS-Media.bak'`CentOS-PowerTools.repo' -> `CentOS-PowerTools.bak'`CentOS-Sources.repo' -> `CentOS-Sources.bak'`CentOS-Vault.repo' -> `CentOS-Vault.bak'`CentOS-centosplus.repo' -> `CentOS-centosplus.bak'`CentOS-fasttrack.repo' -> `CentOS-fasttrack.bak'[study@localhost yum.repos.d]$
```

## gedit로 저장소(repo) 파일 편집

```
[study@localhost yum.repos.d]$ sudo gedit
```

### gedit 실행 오류의 경우

```
[study@localhost yum.repos.d]$ sudo gedit[sudo] study의 암호:No protocol specifiedUnable to init server: 연결할 수 없습니다: 연결이 거부됨(gedit:????): Gtk-WARNING **: ??:??:??.???: cannot open display: :0[study@localhost yum.repos.d]$
```

현재 CentOS 8은 기본 디스플레이 서버가 Wayland(스탠다드)로 설정 되어 있습니다. 이런 경우 관리자 권한으로 gedit 같은 GUI 프로그램을 실행하는 경우 위 메시지처럼 cannot open display: :0 오류 메시지를 출력하며 실행되지 않습니다.

이것을 해결하기 위해서는 아래의 명령을 실행해주어야 합니다.

위 명령은 관리자 권한(root) 사용자가 실행중인 X 서버에 액세스할 수 있도록 권한을 부여해주는 명령입니다.

```
[study@localhost ~]$ xhost +SI:localuser:rootlocaluser:root being added to access control list[study@localhost ~]$
```

위 명령을 실행 후 관리자 권한(sudo)으로 gedit를 실행하면 문제 없이 실행되는 것을 확인할 수 있습니다.

## CentOS 8 단일 저장소 파일 생성

CentOS 8의 저장소는 기본적으로 11의 저장소가 각각의 파일로 관리 됩니다. 하나의 파일 안에 모든 저장소 설정을 담는 것이 관리하기 편할 것입니다.

국내에는 CentOS 미러 저장소로 포털사이트인 Kakao와 Naver 그리고 호스팅 업체인 Aonenetworks가 정식으로 등록되어 있으며 이외에도 다른 미러 서버가 존재할 수 있습니다.(만약 아시는 분들은 댓글로 알려주시기 바랍니다.)

개인적으로 카카오의 미러를 사용할 것을 추천드립니다.

### 저장소 파일 생성

위와 같은 형식으로 파일을 생성해주시면 됩니다. 확장자명을 .repo로 정해주시면 됩니다.

### 기본 활성화 저장소

### 기본 활성화 저장소 포함 추가 저장소 활성화

추가 저장소 역시 위 예시처럼 수정하여 내용을 추가해주시면 됩니다.

CentOS 8 저장소 기본값은 /etc/yum.repos.d 디렉터리리 안의 파일들을 참고하시거나, 아래 링크를 참고하시기 바랍니다.

## 추천 저장소 추가

### EPEL 저장소 추가

```
[study@localhost ~]$ sudo dnf install epel-release[sudo] study의 암호:CentOS-8 - AppStream                            1.6 kB/s | 4.3 kB     00:02CentOS-8 - Base                                 1.6 kB/s | 3.9 kB     00:02CentOS-8 - Extras                               646  B/s | 1.5 kB     00:02종속성이 해결되었습니다.================================================================================ 꾸러미                아키텍처        버전               리포지토리       크기================================================================================Installing: epel-release          noarch          8-5.el8            extras           22 k거래 요약================================================================================설치  1 꾸러미총 다운로드 크기 : 22 k설치 크기 : 30 k이게 괜찮습니까 [y / N] : y패키지 다운로드중:epel-release-8-5.el8.noarch.rpm                  21 kB/s |  22 kB     00:01--------------------------------------------------------------------------------합계                                            9.1 kB/s |  22 kB     00:02트랜잭션 점검 실행 중트랜잭션 검사가 성공했습니다.트랜잭션 테스트 실행 중트랜잭션 테스트가 완료되었습니다.거래 실행 중  준비 중입니다  :                                                          1/1  Installing     : epel-release-8-5.el8.noarch                              1/1  스크립틀릿 실행: epel-release-8-5.el8.noarch                              1/1  확인 중        : epel-release-8-5.el8.noarch                              1/1설치됨:  epel-release-8-5.el8.noarch완료되었습니다![study@localhost ~]$
```

### 저장소 확인

```
[study@localhost ~]$ dnf repolistExtra Packages for Enterprise Linux 8 - x86_64  1.0 MB/s | 5.1 MB     00:05마지막 메타 데이터 만료 확인 : 0:00:05 전에 2020년 01월 05일 (일) 오후 01시 00분 00초.Repo ID            레포 이름                                               상태AppStream          CentOS-8 - AppStream                                    5,089BaseOS             CentOS-8 - Base                                         2,843*epel              Extra Packages for Enterprise Linux 8 - x86_64          4,352extras             CentOS-8 - Extras                                           3[study@localhost ~]$
```

epel 관련 저장소 설정 파일 기본값

epel 저장소 설정 파일 기본값

epel-playground 저장소 설정 파일 기본값

epel-testing 저장소 설정 파일 기본값

### Remi 저장소 추가

```
[study@localhost ~]$ sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm[sudo] study의 암호:마지막 메타 데이터 만료 확인 : 0:20:00 전에 2020년 01월 05일 (일) 오후 01시 20분 00초.remi-release-8.rpm                              8.2 kB/s |  20 kB     00:02종속성이 해결되었습니다.================================================================================ 꾸러미             아키텍처     버전                  리포지토리          크기================================================================================Installing: remi-release       noarch       8.0-4.el8.remi        @commandline        20 k거래 요약================================================================================설치  1 꾸러미총합 크기: 20 k설치 크기 : 14 k이게 괜찮습니까 [y / N] : y패키지 다운로드중:트랜잭션 점검 실행 중트랜잭션 검사가 성공했습니다.트랜잭션 테스트 실행 중트랜잭션 테스트가 완료되었습니다.거래 실행 중  준비 중입니다  :                                                          1/1  Installing     : remi-release-8.0-4.el8.remi.noarch                       1/1  확인 중        : remi-release-8.0-4.el8.remi.noarch                       1/1설치됨:  remi-release-8.0-4.el8.remi.noarch완료되었습니다![study@localhost ~]$
```

### 저장소 확인

```
[study@localhost ~]$ dnf repolistRemi's Modular repository for Enterprise Linux   52 kB/s | 527 kB     00:10Safe Remi's RPM repository for Enterprise Linux  90 kB/s | 1.4 MB     00:15Repo ID       레포 이름                                                    상태AppStream     CentOS-8 - AppStream                                         5,089BaseOS        CentOS-8 - Base                                              2,843*epel         Extra Packages for Enterprise Linux 8 - x86_64               4,352extras        CentOS-8 - Extras                                                3remi-modular  Remi's Modular repository for Enterprise Linux 8 - x86_64       14remi-safe     Safe Remi's RPM repository for Enterprise Linux 8 - x86_64   2,092[study@localhost ~]$
```

위 내용을 보면 아시겠지만 remi 저장소가 활성화되지 않으 것을 알 수 있습니다.

Remi 관련 저장소 설정 파일 기본값

Remi 저장소 설정 파일 기본값

Remi-modular 저장소 설정 파일 기본값

Remi-safe 저장소 설정 파일 기본값