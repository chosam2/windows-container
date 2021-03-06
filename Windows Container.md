---


---

<h1 id="windows-컨테이너">Windows 컨테이너</h1>
<p>최근 IT 시장은 클라우드를 넘어 클라우드 네이티브 시대에 도래했습니다.  이러한 클라우드 네이티브 컴퓨팅은 컨테이너를 기반으로 서비스 또는 애플리케이션 제공합니다.</p>
<p>컨테이너는 기존 환경에 비해 더욱 안정적이고 효율인 운영/관리를 제공합니다. 또한 비즈니스 요구의 변화에 빠르게 대응 가능하며 시스템 확장이 용이한 특징을 가지고 있습니다.</p>
<p>지금까지  대부분 컨테이너는 Linux 시스템에서 제공되어 왔습니다. 하지만 많은 기업과 조직에서 제공하는 서비스와 애플레케이션은 상당 부분 Windows 애플리케이션으로 구성되어 있습니다.</p>
<p>Microsoft는 <strong>Docker</strong>를 사용하여 Windows 환경에서도 컨테이너를 사용할 수 있도록 제공하고 있습니다.</p>
<h2 id="windows-컨테이너-소개">Windows 컨테이너 소개</h2>
<p>Microsoft는 컨테이너 기반의 앱을 개발 및 배포하기 위한 다양한 도구와 플랫폼을 제공합니다.<br>
Linux 기반의 컨테이너 기술이 업계에서 주목을 받던 시기부터 Microsoft는 직접 개발을 진행했습니다.<br>
Windows Server 2016에서 처음으로 정식으로 탑재되었으며, 2019년 봄에는 Kubernetes 1.14에서 Kubernetes Worker Node를 Windows Server위에서  구동할 수 있도록 정식으로 지원하게 되었습니다.</p>
<p><s>### Windows 컨테이너를 사용해야하는 이유는?<br>
빠르게 변화하는 애플리케이션 배포 환경을  ‘클라우드 네이티브’ 환경에서의 개발은 필수요소로 자리매김 하고있습니다. 이러한 '클라우드 네이티브’를 위한 핵심 요소로는 데브옵스(DevOps), 지속적인 통합 및 배포(CI/CD),  마이크로 서비스 아키텍처(MSA), 그리고 컨테이너(Container)가 있습니다.</s></p>
<h3 id="windows-컨테이너-특징">WIndows 컨테이너 특징</h3>
<p>기존 Linux 컨테이너와 유사한 구조로 되어있으며, 사용법 또한 OCI 표준을 따르므로 동일합니다. 다만, 라이센스 문제로 인하여 제한적인 컨테이너 OS 이미지를 사용해야 합니다.</p>
<p><a href="https://hub.docker.com/_/microsoft-windows-base-os-images">mcr.microsoft.com 이미지 레지스트리</a>에서는 크게 3가지 형태의 이미지를 제공합니다.</p>
<h2 id="windows-컨테이너-아키텍처">Windows 컨테이너 아키텍처</h2>
<p>앞서 언급한 것 처럼 Windows  컨테이너의 사용법은 Linux 컨테이너와 큰 차이가 없습니다. 하지만 세부적인 구조를 살펴본다면 큰 차이를 가지고 있습니다.</p>
<p>Linux 환경에서는 Docker와 같은 컨테이너 관리 도구를 통해 저수준/고수준 런타임 관리를 위해  runC/containerd 등과 같은 세분화 된 컨테이너 도구를 사용합니다.</p>
<p>하지만 Windows 컨테이너에서는 Host Compute Servicve(HCS), Host Network Service(HNS) 등을 통해 컨테이너 관리를 가능하게 제공하고 있습니다. 자세한 설명은 <a href="%5Bhttps://techcommunity.microsoft.com/t5/containers/introducing-the-host-compute-service-hcs/ba-p/382332%5D(https://techcommunity.microsoft.com/t5/containers/introducing-the-host-compute-service-hcs/ba-p/382332)">이곳</a>을 통해 확인하시길 바랍니다.</p>
<h2 id="linux">Linux</h2>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/deploy-containers/media/docker-on-linux.png" alt="enter image description here"></p>
<h2 id="windows">Windows</h2>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/deploy-containers/media/hcs.png" alt="enter image description here"></p>
<h2 id="격리모드">격리모드</h2>
<p>Windows 컨테이너는 두 가지 런타임 격리 모드인 <code>Process</code>와 <code>Hypver-V</code> 격리모드를 제공하고 있습니다. 두 격리모드는 OCI 표준을 따르는 이미지를 사용하기 때문에 동일하게 사용 가능합니다.</p>
<p>다만, 생성 방식의 차이에 따른 호스트-컨테이너 간의 격리 수준의 차이등이 존재합니다.<br>
<code>process</code> 방식의 경우는 전통적인 컨테이너 생성 방식과 같이 호스트의 커널을 공유하는 방식을 사용하지만 <code>hyper-v</code> 격리는 Hyper-V 상에 컨테이너를 생성하므로 별도의 커널을 기반으로 컨테이너가 생성되므로 격리성이 보장된다는 장점이 있습니다. 반면, 경량화된 가상머신 위에  생성되는 방식이기 때문에 process 방식에 비해 오버헤드가 크다는 단점이 있습니다.</p>
<h3 id="process-격리">Process 격리</h3>
<p>앞서 언급한 것 처럼 이는 기존<br>
<img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/manage-containers/media/container-arch-process.png" alt="enter image description here"></p>
<h3 id="hyper-v-격리">Hyper-V 격리</h3>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/manage-containers/media/container-arch-hyperv.png" alt="enter image description here"></p>
<h2 id="격리-확인">격리 확인</h2>
<h3 id="컨테이너-생성">컨테이너 생성</h3>
<p>Docker를 사용하여 프로세스 격리 컨테이너와 Hyper-V 컨테이너를 관리하는 것은 거의 동일합니다.<br>
<code>--isolation</code> 옵션을 사용하여 <strong>격리 모드</strong>를 지정할 수 있습니다.</p>
<ul>
<li>Process : <code>--isolation=process</code></li>
<li>Hyper-V : <code>--isolation=hyperv</code></li>
</ul>
<h3 id="windows---linux-container-비교">Windows - Linux Container 비교</h3>
<h3 id="windows-container-요구사항">Windows Container 요구사항</h3>
<h3 id="운영체제-요구사항">운영체제 요구사항</h3>
<h3 id="가상화-된-컨테이너-호스트">가상화 된 컨테이너 호스트</h3>
<h3 id="windows-container-버전-확인">Windows Container 버전 확인</h3>
<ul>
<li>Powershell</li>
</ul>
<pre class=" language-bash"><code class="prism  language-bash"><span class="token punctuation">[</span>Environment<span class="token punctuation">]</span>::OSVersion.Version
</code></pre>
<ul>
<li></li>
</ul>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzY0NDg3MjU3XX0=
-->