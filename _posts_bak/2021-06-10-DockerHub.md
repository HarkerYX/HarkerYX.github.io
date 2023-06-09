---
layout: post
title: DockerHub
date: '2021-06-10'
description: 'Docker Hub란?'
categories: [Docker]
tags: [Docker, DockerHub]
---
# Docker Hub란?



## 도커 허브(Docker Hub)

도커 허브는 Docker에서 제공하는 서비스이다. 

도커 허브에서는 여러 기능을 제공해준다.

- 저장소 : 컨테이너 이미지를 푸시하고, 가져온다.
- 팀 및 조직 : 컨테이너 이미지의 개인 저장소에 대한 액세스를 관리한다.
- 공식 이미지 : Docker에서 제공하는 고품질 컨테이너 이미지를 가져와 사용한다.
- 게시자 이미지 : 외부 공급 업체에서 제공하는 고품질 컨테이너의 이미지를 가져와 사용한다.
- 빌드 : **GitHub**및 Bitbucket에서 컨테이너 이미지를 자동으로 빌드하고 Docker Hub로 푸시한다.
- 웹 훅스 : Docker Hub를 다른 서비스와 통합하기 위해 리포지토리에 성공적으로 푸시한 후 작업을 트리거 한다.



간단하게 정리하자면,

GitHub및 Bitbucket에서 CI/CD툴을 이용해서 이미지를 빌드한 후 Docker Hub에 배포(푸시)할 수 있으며, 그 이미지를 서버에서 가져와(풀) 컨테이너를 구동시킬 수 있다.

