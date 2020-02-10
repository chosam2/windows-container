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

### Process Isolation





### Hyper-V Isolation
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg4MjU1OTM2LC0zMzI0NTUzNjNdfQ==
-->