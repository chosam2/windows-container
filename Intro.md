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
### Process Isolation

```bash
PS C:\Users\nobreak> docker run -d --name process --isolation=process mcr.microsoft.com/windows/nanoserver:1809 ping localhost -t
cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
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
PS C:\Users\nobreak> get-process -Name vmwp                                                                                                                                                                                               
Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                
-------  ------    -----      -----     ------     --  -- -----------                                                    
951      17     7268      16088       1.55   4368   0 vmwp                                                           
272      14     4964      18764       1.05   6584   0 vmwp 
```

### Hyper-V Isolation

```bash
PS C:\Users\nobreak>  docker run -d --name hyperv --isolation=hyperv mcr.microsoft.com/windows/nanoserver:1809 ping localhost -t
a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b
```

```bash
PS C:\Users\nobreak> get-process -Name ping

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
     75       5      820       3252       0.03   4872   3 PING


PS C:\Users\nobreak> docker top a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b
Name                PID                 CPU                 Private Working Set
smss.exe            980                 00:00:00.328        249.9kB
csrss.exe           1004                00:00:00.296        479.2kB
wininit.exe         276                 00:00:00.296        806.9kB
services.exe        428                 00:00:00.515        1.335MB
lsass.exe           468                 00:00:00.812        2.912MB
svchost.exe         492                 00:00:00.093        774.1kB
svchost.exe         616                 00:00:00.156        766kB
svchost.exe         432                 00:00:00.500        1.319MB
svchost.exe         1032                00:00:00.093        864.3kB
CExecSvc.exe        1056                00:00:00.109        798.7kB
svchost.exe         1112                00:00:00.484        3.076MB
svchost.exe         1156                00:00:00.343        1.593MB
svchost.exe         1300                00:00:03.000        11.1MB
PING.EXE            1308                00:00:00.046        516.1kB
```

```bash
PS C:\Users\nobreak> get-process -Name ping

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
     75       5      820       3252       0.03   4872   3 PING
```

```bash
PS C:\Users\nobreak> get-process -Name vmwp

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    951      17     7268      16088       1.55   4368   0 vmwp
    272      14     4964      18764       1.05   6584   0 vmwp
   
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTA3MjEyNjgzLDQ4ODI1NTkzNiwtMzMyND
U1MzYzXX0=
-->