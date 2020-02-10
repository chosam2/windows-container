---


---

<h1 id="windows-컨테이너">Windows 컨테이너</h1>
<p>클라우드 네이티브 환경에서 컨테이너는 필수적인 사항입니다. <strong>Docker</strong>는 컨테이너를 쉽게 사용할 수 있도록 제공해왔으며, …했습니다.<br>
지금껏 대부분 Linux 기반의 컨테이너를 제공해 왔으나 최근 Microsoft에서는 Docker와 협업하며 Windows 기반의 컨테이너 환경을 지속적으로 제공해 왔습니다.</p>
<h2 id="windows-컨테이너-소개">Windows 컨테이너 소개</h2>
<p>Microsoft는 컨테이너 기반의 앱을 개발 및 배포하기 위한 다양한 도구와 플랫폼을 제공합니다.<br>
Linux 기반의 컨테이너 기술이 업계에서 주목을 받던 시기부터 Microsoft는 직접 개발을 진행했습니다.<br>
Windows Server 2016에서 처음으로 정식으로 탑재되었으며, 2019년 봄에는 Kubernetes 1.14에서 Kubernetes Worker Node를 Windows Server위에서  구동할 수 있도록 정식으로 지원하게 되었습니다.</p>
<h3 id="windows-컨테이너를-사용해야하는-이유는">Windows 컨테이너를 사용해야하는 이유는?</h3>
<p>Linux 기반의 환경에서 컨테이너를 사용하는 것과같이 Windows 기반의 환경에서 앞서 언급한 것과같은 컨테이너의 장점을 활용하기 위해 필수적입니다.</p>
<p>기존의 Linux 컨테이너와 유사한 구조로 되어있으며, 사용법 또한 OCI 표준을 따르므로 동일합니다.</p>
<p>다만, 라이센스 문제로 인하여 제한적인 컨테이너 OS 이미지를 사용해야 합니다.<br>
<a href="https://hub.docker.com/_/microsoft-windows-base-os-images">mcr.microsoft.com 이미지 레지스트리</a>에서는 크게 3가지 형태의 이미지를 제공합니다.</p>
<h2 id="windows-컨테이너-아키텍처">Windows 컨테이너 아키텍처</h2>
<h3 id="프로세스-격리">프로세스 격리</h3>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/manage-containers/media/container-arch-process.png" alt="enter image description here"></p>
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
<h1 id="windows---linux-컨테이너-플랫폼-비교">Windows - Linux 컨테이너 플랫폼 비교</h1>
<h3 id="linux">Linux</h3>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/deploy-containers/media/docker-on-linux.png" alt="enter image description here"></p>
<h3 id="windows">Windows</h3>
<p><img src="https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/deploy-containers/media/hcs.png" alt="enter image description here"></p>

