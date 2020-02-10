    ## Windows Server 2019
    
    ### Step 1 : Hyper -V  활성화
    두 가지 형태의 격리된 컨테이너를 비교하기 위해서는 Hypver-V가 활성화 되어야 합니다.
    
    ### Step 2 : Docker 설치
     
    
    <h2 id="isolation-비교">Isolation 비교</h2>
    <h3 id="이미지-파일">이미지 파일</h3>
    <p>호스트 운영체제와 동일한 버전의 이미지를 다운받습니다.</p>
    <p><code>PS C:\Users\nobreak&gt; docker pull mcr.microsoft.com/windows/nanoserver:1809</code></p>
    <p>다운받은 이미지를 확인합니다.</p>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
    mcr.microsoft.com/windows/nanoserver   1809                080394ef5494        5 weeks ago         251MB
    </code></pre>
    <h3 id="process-isolation">Process Isolation</h3>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> docker run -d --name process --isolation<span class="token operator">=</span>process mcr.microsoft.com/windows/nanoserver:1809 <span class="token function">ping</span> localhost -t
    cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
    </code></pre>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> docker <span class="token function">top</span> cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
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
    </code></pre>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name <span class="token function">ping</span>
    </code></pre><p>
    Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName</p>
    <hr>
    <pre><code> 75       5      820       3252       0.03   4872   3 PING
    </code></pre>
    <p></p>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name vmwp                                                                                                                                                                                               
    Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName                                                
    -------  ------    -----      -----     ------     --  -- -----------                                                    
    951      17     7268      16088       1.55   4368   0 vmwp                                                           
    272      14     4964      18764       1.05   6584   0 vmwp 
    </code></pre>
    <h3 id="hyper-v-isolation">Hyper-V Isolation</h3>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span>  docker run -d --name hyperv --isolation<span class="token operator">=</span>hyperv mcr.microsoft.com/windows/nanoserver:1809 <span class="token function">ping</span> localhost -t
    a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b
    </code></pre>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name <span class="token function">ping</span>
    </code></pre><p>
    Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName</p>
    <hr>
    <pre><code>
    -------  ------    -----      -----     ------     --  -- -----------
         75       5      820       3252       0.03   4872   3 PING
    </code></pre>
    <p>PS C:\Users\nobreak<span class="token operator">&gt;</span> docker <span class="token function">top</span> a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b<br>
    Name                PID                 CPU                 Private Working Set<br>
    smss.exe            980                 00:00:00.328        249.9kB<br>
    csrss.exe           1004                00:00:00.296        479.2kB<br>
    wininit.exe         276                 00:00:00.296        806.9kB<br>
    services.exe        428                 00:00:00.515        1.335MB<br>
    lsass.exe           468                 00:00:00.812        2.912MB<br>
    svchost.exe         492                 00:00:00.093        774.1kB<br>
    svchost.exe         616                 00:00:00.156        766kB<br>
    svchost.exe         432                 00:00:00.500        1.319MB<br>
    svchost.exe         1032                00:00:00.093        864.3kB<br>
    CExecSvc.exe        1056                00:00:00.109        798.7kB<br>
    svchost.exe         1112                00:00:00.484        3.076MB<br>
    svchost.exe         1156                00:00:00.343        1.593MB<br>
    svchost.exe         1300                00:00:03.000        11.1MB<br>
    PING.EXE            1308                00:00:00.046        516.1kB<br>
    </p>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name <span class="token function">ping</span>
    Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName 75       5      820       3252       0.03   4872   3 PING
    </code></pre>
    <pre class="  language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name vmwp
    
    Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName
    -------  ------    -----      -----     ------     --  -- -----------
        951      17     7268      16088       1.55   4368   0 vmwp
        272      14     4964      18764       1.05   6584   0 vmwp
       
    </code></pre>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEzMDYyMjMyNiwtMTcyNjYyMjA1MF19
-->