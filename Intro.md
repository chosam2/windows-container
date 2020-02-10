## Isolation 비교


### 이미지 파일

호스트 운영체제와 동일한 버전의 이미지를 다운받습니다.

`PS C:\Users\nobreak> docker pull mcr.microsoft.com/windows/nanoserver:1809`

다운받은 이미지를 확인합니다.
```bash
PS C:\Users\nobreak> docker images
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
mcr.microsoft.com/windows/nanoserver   1809                080394ef5494        5 weeks ago         251MB
```
```bash
PS C:\Users\nobreak> docker run -d --name process --isolation=process mcr.microsoft.com/windows/nanoserver:1809 ping localhost -t
```
```bash
PS C:\Users\nobreak> docker top cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
Name                PID                 CPU                 Private Working Set
smss.exe            5364                00:00:00.062        245.8kB
csrss.exe           3692                00:00:00.000        417.8kB
wininit.exe         716                 00:00:00.031        651.3kB
services.exe        6552                00:00:00.203        1.266MB
lsass.exe           4952                00:00:00.281        2.798MB
svchost.exe         7020                00:00:00.015        868.4kB
svchost.exe         2020                00:00:00.031        995.3kB
svchost.exe         6352                00:00:00.125        1.63MB
svchost.exe         4664                00:00:00.000        1.13MB
CExecSvc.exe        6216                00:00:00.015        790.5kB
svchost.exe         6280                00:00:00.062        3.043MB
PING.EXE            4872                00:00:00.031        528.4kB
svchost.exe         6668                00:00:00.203        2.72MB
svchost.exe         4216                00:00:01.984        11.75MB
```

```bash
PS C:\Users\nobreak> get-process -Name ping

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
     75       5      820       3252       0.03   4872   3 PING
```

```bash
PS C:\Users\nobreak> get-process -Name vmwp                                                                                                                                                                                               Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                -------  ------    -----      -----     ------     --  -- -----------                                                    951      17     7268      16088       1.55   4368   0 vmwp                                                           272      14     4964      18764       1.05   6584   0 vmwp 
```

### Process Isolation





### Hyper-V Isolation
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQ3MjA1NjEzLDQ4ODI1NTkzNiwtMzMyND
U1MzYzXX0=
-->