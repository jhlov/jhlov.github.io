---
title: '[jekyll] jekyll 블로그에 google analytics 적용하기'
date: 2020-02-19T00:00:00.000Z
categories: jekyll
tags: jekyll google-analytics
published: true
---
## 1. google analytics 가입
[https://analytics.google.com/](https://analytics.google.com/) 사이트에서 가입을 하고 앱을 생성한다.  
너무나 쉽게 잘 되어 있어서 과정은 생략한다. github에 jekyll 블로그 생성하는 것보다 200배는 쉽다.

## 2. 블로그 적용
jekyll 테마에 따라서 조금씩 적용하는 방법이 다르다. 일단 이 블로그 테마인 `minimal mistakes` 테마에서는 상당히 쉽게 되어 있다.  

결론부터 먼저 말하면 
``` javascript
// _config.xml
# Analytics
analytics:
  provider               : false # false (default), "google", "google-universal", "custom"
  google:
    tracking_id          : 
```
위 코드를 아래와 같이 세팅해준다.
``` javascript
// _config.xml
# Analytics
analytics:
  provider               : "google-gtag" # false (default), "google", "google-universal", "custom"
  google:
    tracking_id          : UA-158746697-1  // <- 내 추적 ID
```
`provider` 에 `"google-gtag"` 를 설정하고 `tracking_id`에 `추적 ID`를 넣으면 된다. 


위 주석에는 `"google", "google-universal", "custom"` 3가지만 나와 있는데, `"google-gtag"`는 갑자기 어디서 나왔는가? 하면 이건 코드를 봐도 알 수가 있고, 아니면 minimal mistakes 공식페이지에서도 확인 할 수 있다.


[minimal-mistakes/docs/configuration/#analytics](https://mmistakes.github.io/minimal-mistakes/docs/configuration/#analytics)


> #### AnalyticsPermalink
  Analytics is disabled by default. To enable globally select one of the following:
>
> |Name|Analytics Provider|
  |------|---|
  |google|[Google Standard Analytics]("https://www.google.com/analytics/")|
  |google-universal|[Google Universal Analytics]("https://www.google.com/analytics/")|
  |google-gtag|[Google Analytics Global Site Tag]("https://www.google.com/analytics/")|
  |custom|Other analytics providers|
>
> For Google Analytics add your Tracking Code:
  ``` javascript
  analytics:
  provider: "google-gtag"
  google:
    tracking_id: "UA-1234567-8"
    anonymize_ip: false # default
  ```


### 실제 코드에서 어떻게 동작하는지 간단히 알아 보자.
최종적으로 내 사이트 모든 페이지에 google analytics의 추적코드가 심어있으면 google analytics가 동작하게 된다.

> 웹사이트 추적  
> #### 범용 사이트 태그(gtag.js)  
  이 속성의 범용 사이트 태그(gtag.js) 추적 코드입니다. 이 코드를 복사하여 추적할 모든 웹페이지의 `<HEAD>`에 첫 번째 항목으로 붙여넣으세요. 이미 페이지에 범용 사이트 태그가 있다면 아래 스니펫의 config 행을 기존 범용 사이트 태그에 추가하기만 하면 됩니다.
  ``` html
  <!-- Global site tag (gtag.js) - Google Analytics -->
  <script async src="https://www.googletagmanager.com/gtag/js?id=UA-158746697-1"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'UA-158746697-1');
  </script>
  ```

어떤 방식으로 사이트에 심어지는지 코드를 살펴보자.

``` javascript
// default.html
{% raw %}{% include scripts.html %}{% endraw %}
```
`default.html`의 하단을 보면 `scripts.html` 을 포함한다.

``` javascript
// scripts.html
{% raw %}{% include analytics.html %}{% endraw %}
```
`scripts.html`의 중간부분에서 `analytics.html` 을 포함한다.

``` javascript
// analytics.html
{% raw %}
{% case site.analytics.provider %}
{% when "google" %}
  {% include /analytics-providers/google.html %}
{% when "google-universal" %}
  {% include /analytics-providers/google-universal.html %}
{% when "google-gtag" %}
  {% include /analytics-providers/google-gtag.html %}
{% when "custom" %}
  {% include /analytics-providers/custom.html %}
{% endcase %}
{% endraw %}
```
`analytics.html`코드에서 `analytics.provider`에 따라 다른 `html` 파일을 포함하는 것을 알 수 있다.

``` javascript
// google-gtag.html
{% raw %}
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.analytics.google.tracking_id }}"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', '{{ site.analytics.google.tracking_id }}', { 'anonymize_ip': {{ site.analytics.google.anonymize_ip | default: false }}});
</script>
{% endraw %}
```
`google-gtag.html`코드에서 google analytics 의 추적코드와 같은 모양의 코드를 확인 할 수 있다.  
그래서 우리가 `_config.xml`의 `provider: "google-gtag"`와 `google.tracking_id`를 세팅하면 `{% raw %}{{ site.analytics.google.tracking_id }}{% endraw %}` 이 부분에 출력이 되서 google analytics 가 잘 동작하게 된다.


혹시 `minimal mistakes` 테마가 아니라서 위에서 설명한 코드와 다르다면, `default.html` 에 추적코드를 그대로 심어도 잘 작동하게 된다.
