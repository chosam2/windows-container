---


---

<h3 id="windows-server-2019">Windows Server 2019</h3>
<h3 id="step-1--hyper--v--활성화">Step 1 : Hyper -V  활성화</h3>
<p>두 가지 형태의 격리된 컨테이너를 비교하기 위해서는 Hypver-V가 활성화 되어야 합니다.</p>
<pre class=" language-powershell"><code class="prism  language-powershell">Install<span class="token operator">-</span>Module <span class="token operator">-</span>Name DockerMsftProvider <span class="token operator">-</span>Repository PSGallery <span class="token operator">-</span>Force
</code></pre>
<h3 id="step-2--docker-설치">Step 2 : Docker 설치</h3>
<pre class=" language-powershell"><code class="prism  language-powershell">Install<span class="token operator">-</span>Package <span class="token operator">-</span>Name docker <span class="token operator">-</span>ProviderName DockerMsftProvider
</code></pre>
<pre class=" language-powershell"><code class="prism  language-powershell"><span class="token function">Restart-Computer</span> <span class="token operator">-</span>Force
</code></pre>
<pre class=" language-powershell"><code class="prism  language-powershell"><span class="token function">PS</span> C:\Users\nobreak&gt; Get<span class="token operator">-</span>Package <span class="token operator">-</span>Name Docker <span class="token operator">-</span>ProviderName DockerMsftProvider

Name                           Version          Source                           ProviderName
<span class="token operator">--</span>-<span class="token operator">-</span>                           <span class="token operator">--</span>-<span class="token operator">--</span>-<span class="token operator">-</span>          <span class="token operator">--</span>-<span class="token operator">--</span><span class="token operator">-</span>                           <span class="token operator">--</span>-<span class="token operator">--</span>-<span class="token operator">--</span>-<span class="token operator">--</span><span class="token operator">-</span>
docker                         19<span class="token punctuation">.</span>03<span class="token punctuation">.</span>5          DockerDefault                    DockerMsftProvider
</code></pre>
<pre class=" language-powershell"><code class="prism  language-powershell"><span class="token function">PS</span> C:\Users\nobreak&gt; docker version                                                                                     Client: Docker Engine <span class="token operator">-</span> Enterprise                                                                                       Version:           19<span class="token punctuation">.</span>03<span class="token punctuation">.</span>5                                                                                              API version:       1<span class="token punctuation">.</span>40                                                                                                 Go version:        go1<span class="token punctuation">.</span>12<span class="token punctuation">.</span>12                                                                                            Git commit:        2ee0c57608                                                                                           Built:             11<span class="token operator">/</span>13<span class="token operator">/</span>2019 08:00:16                                                                                  OS<span class="token operator">/</span>Arch:           windows<span class="token operator">/</span>amd64                                                                                        Experimental:      false                                                                                                                                                                                                                       Server: Docker Engine <span class="token operator">-</span> Enterprise                                                                                       Engine:                                                                                                                  Version:          19<span class="token punctuation">.</span>03<span class="token punctuation">.</span>5                                                                                               API version:      1<span class="token punctuation">.</span>40 <span class="token punctuation">(</span>minimum version 1<span class="token punctuation">.</span>24<span class="token punctuation">)</span>                                                                           Go version:       go1<span class="token punctuation">.</span>12<span class="token punctuation">.</span>12                                                                                             Git commit:       2ee0c57608                                                                                            Built:            11<span class="token operator">/</span>13<span class="token operator">/</span>2019 07:58:51                                                                                   OS<span class="token operator">/</span>Arch:          windows<span class="token operator">/</span>amd64                                                                                         Experimental:     false   
</code></pre>
<h3 id="isolation-비교">Isolation 비교</h3>
<h3 id="이미지-파일">이미지 파일</h3>
<p>호스트 운영체제와 동일한 버전의 이미지를 다운받습니다.</p>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker pull mcr.microsoft.com/windows/nanoserver:1809
</code></pre>
<p>다운받은 이미지를 확인합니다.</p>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
mcr.microsoft.com/windows/nanoserver   1809                080394ef5494        5 weeks ago         251MB
</code></pre>
<h3 id="process-isolation">Process Isolation</h3>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker run -d --name process --isolation<span class="token operator">=</span>process mcr.microsoft.com/windows/nanoserver:1809  localhost -t
cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
</code></pre>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> docker inspect cece6585ce23 <span class="token operator">|</span> Select-String isolation

            <span class="token string">"Isolation"</span><span class="token keyword">:</span> <span class="token string">"process"</span>,
</code></pre>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker <span class="token function">top</span> cece6585ce235d4790d6ab4d86c425338a6c9091b0f45c2e6a4f883fd7348f7a
Name            PID                 CPU                 Private Working Set
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
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name <span class="token function">ping</span>                                                                                                                                                                                               Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName                                                -------  ------    -----      -----     ------     --  -- -----------
     75       6      816       3236       0.08   4872   3 PING    
</code></pre>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> get-process -Name vmwp
Handles  NPM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>    PM<span class="token punctuation">(</span>K<span class="token punctuation">)</span>      WS<span class="token punctuation">(</span>K<span class="token punctuation">)</span>     CPU<span class="token punctuation">(</span>s<span class="token punctuation">)</span>     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    270      14     5760      19296       1.61   2216   0 vmwp
    956      16     6820      15900       1.53   6704   0 vmwp
</code></pre>
<h3 id="hyper-v-isolation">Hyper-V Isolation</h3>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker run -d --name hyperv --isolation<span class="token operator">=</span>hyperv mcr.microsoft.com/windows/nanoserver:1809 <span class="token function">ping</span> localhost -t
a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b
</code></pre>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak<span class="token operator">&gt;</span> docker inspect a503f75e9583 <span class="token operator">|</span> Select-String isolation

            <span class="token string">"Isolation"</span><span class="token keyword">:</span> <span class="token string">"hyperv"</span>,
</code></pre>
<pre class=" language-bash"><code class="prism  language-bash">PS C:\Users\nobreak docker <span class="token function">top</span> a503f75e958370f0bb544c94e28b65eb38e9dc59a1087297316c5006503f997b
Name            PID                 CPU                 Private Working Set
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
</code></pre>

