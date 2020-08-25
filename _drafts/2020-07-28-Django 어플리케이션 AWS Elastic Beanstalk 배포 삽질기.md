---
title: "Django 어플리케이션 AWS Elastic Beanstalk 배포 삽질기"
date: 2020-07-28
categories: django
tags: django aws beanstalk 장고 빈스톡
---

간단한 사이드 프로젝트를 시작했다. 로컬서버에서만 올려 테스트를 하다가, 실제로 반영을 해보기로 했다. aws의 여러가지 서비스를 고려해보았다. EC2, lightsail, elastic beanstalk 이렇게 3가지 중에 제일 간단해 보이는 beanstalk 으로 결정을 했다.

빈스톤 간단한 설명

- [Django 애플리케이션을 Elastic Beanstalk에 배포](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-django.html)

일단 AWS에 굉장히 자세한 설명이 있고, 그 외에도 몇몇 블로그들이 있어서 어렵지 않게 적용해 볼 수 있었다. 보통은 awscli를 사용해서 터미널에서 바로 적용을 하는 방법을 설명하고 있지만, aws 웹페이지에서 손쉽게 배포할 수 있고, 현재 별다른 업데이트가 없어서 아직은 웹을 통해서 배포 하고 있다. 웹을 통한 배포는 딱히 설명히 필요없을 만큼 간단하다. 직접 해보면 클릭 2-3만에 끝날 수 있다.

- [애플리케이션 시작 - AWS Elastic Beanstalk 사용](https://aws.amazon.com/ko/getting-started/hands-on/launch-an-app/)

#### 삽질

하지만 역시나 한번에 잘 되진 않았다. 물론 샘플 프로젝트로 실행 했을 때는 한번에 잘 됐지만, 내 프로젝트를 적용 했을 때, 문제가 몇가지 있었다.

1. 플랫폼 문제

아마존 리눅스2 안됨
디비 연결
스태틱파일
