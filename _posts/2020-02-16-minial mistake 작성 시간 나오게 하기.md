---
title: '[jekyll] github 블로그 포스팅 시간 출력하기'
date: 2020-02-16
categories: jekyll
tags: jekyll
---

`minimal mistakes` 테마에서는 기본적으로 포스팅을 읽을 때 예상되는 소요시간을 표시해준다.

![github_블로그_포스팅_시간_출력하기_01.png](/assets/img/blog_postdate_01.png)

소요시간이 출력되는 `read-time.html` 코드를 보면, `_config.yml`에서 세팅한 `words_per_minute` 값과 `words`를 계산해서 출력하는 것 같다.

``` javascript
// _includes/read-time.html
{% raw %}
{% assign words_per_minute = page.words_per_minute | default: site.words_per_minute | default: 200 %}

{% if post.read_time %}
  {% assign words = post.content | strip_html | number_of_words %}
{% elsif page.read_time %}
  {% assign words = page.content | strip_html | number_of_words %}
{% endif %}

{% if words < words_per_minute %}
  {{ site.data.ui-text[site.locale].less_than | default: "less than" }} 1 {{ site.data.ui-text[site.locale].minute_read | default: "minute read" }}
{% elsif words == words_per_minute %}
  1 {{ site.data.ui-text[site.locale].minute_read | default: "minute read" }}
{% else %}
  {{ words | divided_by:words_per_minute }} {{ site.data.ui-text[site.locale].minute_read | default: "minute read" }}
{% endif %}
{% endraw %}
```

---
개인적인 생각으로 크게 필요한 정보라고 생각되지 않아서 일반적인(?) 블로그 처럼 게시된 날짜를 표시하도록 변경하였다.

글의 목록을 보여주는 부분(`_includes/archive-single.htm`)과 글을 보여주는 부분(`_includes/single.html`) 2군데를 수정하면 된다.

``` html
// _includes/archive-single.html 
{% raw %}
<h2 class="archive__item-title" itemprop="headline">      
    {% if post.link %}
    <a href="{{ post.link }}">{{ title }}</a> <a href="{{ post.url | relative_url }}" rel="permalink"><i class="fas fa-link" aria-hidden="true" title="permalink"></i><span class="sr-only">Permalink</span></a>
    {% else %}
    <a href="{{ post.url | relative_url }}" rel="permalink">{{ title }}</a>
    {% endif %}
</h2>
{% if post.read_time %}
    <p class="page__meta"><i class="far fa-clock" aria-hidden="true"></i> {% include read-time.html %}</p>
{% endif %}
{% endraw %}
```
위 코드가 글의 제목과 소요 시간(`read_time옵션 값이 true일 때, read-time.html 파일을 포함`)을 표시해 주는 부분이다.

최대한 기존 코드를 건들고 싶지 않아서 바로 밑에 아래 코드를 추가 해 주었다.
``` html
{% raw %}
{% if post.date %}
    <span class="page__date"><i class="far fa-clock" aria-hidden="true"></i>{{ post.date | date: "%Y-%M-%d" }}</span>
{% endif %}
{% endraw %}
```
`page__date` 클래스를 만들어 간단한 css요소(폰트크기 등)를 추가했다.

---

마찬가지로 `_includes/single.html`파일에서
``` html
// _includes/single.html
{% raw %}
<header>
    {% if page.title %}<h1 id="page-title" class="page__title" itemprop="headline">{{ page.title | markdownify | remove: "<p>" | remove: "</p>" }}</h1>{% endif %}
    {% if page.read_time %}
    <p class="page__meta"><i class="far fa-clock" aria-hidden="true"></i> {% include read-time.html %}</p>
    {% endif %}
</header>
{% endraw %}
```
위 코드 아래에 포스팅 날짜를 출력해주는 코드를 넣으면 된다.

기존의 `read_time`이 출력 안되도록, `_config.xml` 에서 `read_time: true` 로 되어 있는 부분을 `false`로 변경하면 아래와 같이 소요시간은 표시되지 않고, 포스팅 날짜가 잘 출력되게 된다.

![github_블로그_포스팅_시간_출력하기_02.png](/assets/img/blog_postdate_02.png)
