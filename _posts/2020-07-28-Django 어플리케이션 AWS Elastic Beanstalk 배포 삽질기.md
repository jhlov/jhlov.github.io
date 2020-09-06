---
title: "Django 어플리케이션 AWS Elastic Beanstalk 배포 삽질기"
date: 2020-07-28
categories: django
tags: django aws beanstalk 장고 빈스톡
---

간단한 사이드 프로젝트를 시작했다. 로컬서버에서만 올려 테스트를 하다가, 실제로 반영을 해보기로 했다. aws의 여러가지 서비스를 고려해보았다. EC2, lightsail, elastic beanstalk 이렇게 3가지 중에 제일 간단해 보이는 beanstalk 으로 결정을 했다.

> AWS Elastic Beanstalk는 Java, .NET, PHP, Node.js, Python, Ruby, Go, Docker를 사용하여 Apache, Nginx, Passenger, IIS와 같은 친숙한 서버에서 개발된 웹 애플리케이션 및 서비스를 간편하게 배포하고 조정할 수 있는 서비스입니다.

> 코드를 업로드하기만 하면 Elastic Beanstalk가 용량 프로비저닝, 로드 밸런싱, Auto Scaling부터 시작하여 애플리케이션 상태 모니터링에 이르기까지 배포를 자동으로 처리합니다. 이뿐만 아니라 애플리케이션을 실행하는 데 필요한 AWS 리소스를 완벽하게 제어할 수 있으며 언제든지 기본 리소스에 액세스할 수 있습니다.

> Elastic Beanstalk는 추가 비용 없이 애플리케이션을 저장 및 실행하는 데 필요한 AWS 리소스에 대해서만 요금을 지불하면 됩니다.

- 참고
    - [AWS Elastic Beanstalk](https://aws.amazon.com/ko/elasticbeanstalk/)

---

- [Django 애플리케이션을 Elastic Beanstalk에 배포](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-django.html)

일단 AWS에 굉장히 자세한 설명이 있고, 그 외에도 몇몇 블로그들이 있어서 어렵지 않게 적용해 볼 수 있었다. 보통은 awscli를 사용해서 터미널에서 바로 적용을 하는 방법을 설명하고 있지만, aws 웹페이지에서 손쉽게 배포할 수 있고, 현재 별다른 업데이트가 없어서 웹을 통해서 배포하고 있다. 웹을 통한 배포는 딱히 설명이 필요 없을 만큼 간단하다. 직접 해보면 클릭 2-3번 만에 끝날 수 있다.

- [애플리케이션 시작 - AWS Elastic Beanstalk 사용](https://aws.amazon.com/ko/getting-started/hands-on/launch-an-app/)

### 삽질

하지만 역시나 한 번에 잘 되진 않았다. 샘플 프로젝트로 실행했을 때는 한 번에 잘 됐지만, 내 프로젝트를 적용했을 때, 문제가 몇 가지 발생했다.

1. 플랫폼 문제
    - ec2 생성 시, `Python 3.7, Amazon Linux 2` 플랫폼으로 생성을 했고, 라이브러리 인스톨시에 문제가 발생했다.
    - `Python 3.6, Amazon Linux` 플랫폼으로 새로 생성한 후에는 정상적으로 라이브러리 인스톨이 되었다.
    - 주의할 점은 `django.config` 파일에서 `Amazon Linux 2` 에서는 `WSGIPath: ebdjango.wsgi:application` 로 설정을 하고 `Amazon Linux`에서는 `ebdjango/wsgi.py` 으로 사용해야 된다. (ebdjango 는 프로젝트 이름)
2. DB 연결
    - DB로 Amazon RDS를 사용했다.
    - 정말 기초적인 내용이지만, 기본적으로 외부에서 접근이 막혀 있다.
    - 보안그룹 Inbound rules에 ip 를 추가해서 해결하였다.
    - 별다른 에러 로그가 나오지 않아서 의외로 여기서 시간이 오래 걸렸다.
    - 참고
        - [AWS RDS 사용방법 - 외부 접속](https://develop-im.tistory.com/13)
        - [Amazon RDS DB 인스턴스에 연결할 때 발생하는 문제를 해결하려면 어떻게 해야 합니까?](https://aws.amazon.com/ko/premiumsupport/knowledge-center/rds-cannot-connect/)
3. 스태틱파일 연결 문제
    - 위의 문제들을 해결하고는 접속이 잘 되었다. 하지만 css 파일들이 적용되지 않아, 레이아웃이 다 깨져서 보이는 현상이 발생했다.
    - static 파일을 제대로 읽지 못해서 발생한 문제이다.
    - 장고 세팅으로 아래와 같이 되어있고 
        ```python
        STATIC_ROOT = 'staticfiles'
        STATIC_URL = '/s/'
        ```
    - `django.config` 파일에 아래와 같이 설정하고 페이지가 정상적으로 잘 동작되었다.
        ```ini
        aws:elasticbeanstalk:container:python:staticfiles:
          /s: staticfiles
        ```
    - 참고
        - [Elastic Beanstalk Python 플랫폼 사용](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-container.html)

---

처음에 삽질을 겪은 후에는 정말 편하게 사용하고 있다. 개발자는 개발 외의 다른 인프라에 대해 전혀 신경 쓰지 않아도, 손쉽게 서버를 배포할 수 있고, 별도의 비용도 들지 않는다. 간단한 프로토타이핑이나 개인 사이드 프로젝트 배포 등에 정말 좋은 서비스라고 생각이 된다.
