---
layout: default
title: Java Docker 이미지 선정
nav_order: 1
has_children: false
parent: 자바 도커 컨테이너
# grand_parent: 
permalink: /docs/a-java-docker-container/select-a-java-docker-image
---


# Java Docker 이미지 선정
{: .no_toc }
<br>


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

<br>

### 참고자료
{: .fs-6 .fw-700 }
- [효율적인 도커 이미지 만들기](https://bcho.tistory.com/1356)
- [docker base image Debian 과 alpine - 효율적인 도커 이미지](https://mio-java.tistory.com/72)
<br>
<br>

### 참고
{: .fs-6 .fw-700 }
- [openjdk 공식 docker hub 리포지터리](https://hub.docker.com/_/openjdk)

<br>
<br>

### 주요 고려사항들
{: .fs-6 .fw-700 }

- 알파인(alpine) 리눅스 기반 이미지 또는 slim 이미지를 선택하는 이유
- 버전명 원칙
- JRE vs JDK
<br>
<br>

#### 알파인(alpine) 리눅스 기반 이미지 또는 slim 이미지를 선택하는 이유
{: .fs-5 .fw-700 }

상용 OS 중 개발용도로 자주 쓰이는 Ubuntu 같은 OS 이미지 위에 JDK/JRE 를 설치하면 이미지가 꽤 무거워진다. 왜냐하면 불필요한 유틸들인 ftp, telnet 및 OS 바이너리까지 불필요하게 포함되기 때문에 직접 설치해보면 OS 만 포함되었는데도 200MB \~ 500MB 까지 용량을 차지한다.<br>
<br>

반면, 알파인 리눅스의 경우 약 5.5 MB 정도를 를 차지한다. ubuntu, debian 이미지는 100MB, centos 이미지는 200MB 가량을 차지한다는 점에 비하면 매우 가볍다. ubuntu, debian, centos 에 비해 20배 이상 가벼우면서, 필요한 기능을 갖췄고, 보안 역시 취약하지 않다.<br>
<br>

알파인 리눅스의 이런 장점들 때문에 가급적 Java 이미지를 사용할 때는 가급적이면 알파인 리눅스 기반의 Java 도커 이미지를 사요하거나 slim 이미지를 사용하는 것이 권장되는 편.<br>
<br>

##### 무거운 Java 이미지를 사용하는 것의 단점
{: .fs-4 .fw-700 }

배포 시에 빌드가 꽤 오래걸릴 수 있다는 점은 꽤 단점이다. 비교적 빌드해야 하는 것이 많은 ubuntu, centos, debin 계열 리눅스 기반 이미지는 시간이 오래 걸리고 이미지의 사이즈도 갈수록 커진다. 반면 알파인 기반 리눅스 이미지는 가볍기에 빌드,배포하는데에 시간이 다소 적게 걸린다. 가급적이면 빌드/배포시에 드는 시간을 줄이고, 이미지를 가볍게 하는 것이 좋다. 
<br>

뭔가 단적인 예를 들어보면, [20-ea-17-jdk](https://hub.docker.com/layers/library/openjdk/20-ea-17-jdk/images/sha256-3669b75f66492d825ec091152c613db9f7f33659df88b858dfac623c3a6033aa?context=explore) 같은 버전의 도커 이미지를 예로 들 수 있을 것 같다. [20-ea-17-jdk](https://hub.docker.com/layers/library/openjdk/20-ea-17-jdk/images/sha256-3669b75f66492d825ec091152c613db9f7f33659df88b858dfac623c3a6033aa?context=explore) 이미지는 windows 기반 이미지든, Linux 기반 이미지든 2G가 훨씬 넘어가는데, 로컬 개발환경을 처음 세팅하는 사람은 개발 PC에 2G의 도커 이미지를 다운받고 있어야 한다. ㄷㄷㄷ
<br>
<br>

##### 특정 버전의 java 이미지가 알파인 리눅스를 지원하지 않는다면?
{: .fs-4 .fw-700 }

JDK 버전에 따라서 alpine 리눅스를 안정적으로 지원하지 않는 경우도 있다. 이런 경우 Docker slim 이미지를 사용하면 된다.<br>
<br>
<br>

#### 알파인 리눅스 기반 버전명 원칙
{: .fs-5 .fw-700 }

알파인 리눅스 용도의 openjdk는 아래와 같은 규칙으로 찾을 수 있다.
- openjdk:{자바버전}-jre-alpine
- openjdk:{자바버전}-jdk-alpine
  - e.g. [17-jdk-alpine 으로 검색](https://hub.docker.com/_/openjdk/tags?page=1&name=17-jdk-alpine)
<br>

가끔 알파인 리눅스 버전이 없는 경우도 있다. 비교적 최신 버전일 경우 없는 경우도 있다. 이런 경우는 Docker slim 버전을 다운로드 한다.
<br>
<br>

#### JRE vs JDK
{: .fs-5 .fw-700 }

JDK 는 컴파일러까지 포함된 버전이고 JRE는 실행환경만을 포함한 환경이다.<br>

운영만을 위해서라면 JRE를 사용할 수 있지만, 간혹가다 스레드덤프를 jdk 를 이용해서 떠야 하는 경우 역시 있다. 이런 경우는 jdk를 사용하기도 한다.
<br>

실무에서는 힙 덤프나 여러가지 자바 기본 유틸을 사용해야하는 경우도 있어서, 굳이 JRE 만 선호하기 보다는 필요에 따라 JDK 기반 이미지를 사용하는 것도 나쁘지는 않은 것 같다. JDK와 JRE는 용량에서 조금 크기가 차이가 나지만 그렇게 크게 차이가 나지 않는다는 생각도 든다.
<br>

