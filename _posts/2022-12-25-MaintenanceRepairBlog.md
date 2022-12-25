---
title: "[BlogDev] 제대 후 다 까먹은 나를 위해... 깃허브 블로그(minimal-mistakes) 유지보수에 도움이 되는 정보들"
excerpt: "계속해서 사용하면서 도움이 될 것 같은 정보를 발견할 때마다 업데이트"

categories:
  - BlogDev
tags:
  - [BlogDev, minimal-mistakes, Github]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-25
last_modified_at: 2022-12-25
---

<br>

# 사이드바(카테고리) 항목 추가

## 같은 카테고리만 모아두는 페이지 추가

- \_pages 폴더 안 categories 폴더 속에 같은 카테고리만 모아두는 페이지 만들기

  - "catagory-카테고리이름.md" 파일 만들기
  - 아래 내용 작성

```md
---
title: "맘대로 변형해서 이름 지을 수 있음"
layout: archive
permalink: categories/카테고리이름
author_profile: true
sidebar_main: true
---

{% raw %}
{% assign posts = site.categories.카테고리이름 %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}
{% endraw %}
```

=> 이 때 `{% raw %}{% include archive-single.html type=page.entries_layout %}{% endraw %}`에서 archive-single.html 혹은 archive-single2.html을 사용할 수 있음. 전자는 처음부터 있는 기본 파일이고, 후자는 다른 블로그에서 공유한 양식

<br>

## 만든 카테고리 페이지를 사이드바에 추가하기

- \_includes 폴더 안 nav_list_main 파일이 사이드바를 담당하는 파일이다.
- 사이드바(nav_list_main 파일) 구조는 아래와 같다.

```html
<!--전체 카테고리인 바깥쪽 ul-->
<ul class="nav__items" id="category_tag_menu">
  <!--전체 글 수 나타내는 li-->
  <li>
    <span>Total Amount: {{ sum }} </span>
  </li>

  <!--각 카테고리1-->
  <li>
    <span class="nav__sub-title"> 카테고리 분류 이름 1 </span>
    <ul>
      <!--목차 넣기-->
    </ul>
  </li>

  <!--각 카테고리2-->
  <li>
    <span class="nav__sub-title"> 카테고리 분류 이름 2 </span>
    <ul>
      <!--목차 넣기-->
    </ul>
  </li>
</ul>
```

- `<ul> <!--목차 넣기--> </ul>` 에 해당하는 내용은 아래와 같다.

```html
{% raw %}
<ul>
  {% for category in site.categories %} {% if category[0] == "카테고리이름" %}
  <li>
    <a href="/categories/BlogDev" class=""
      >맘대로 변형해서 이름 지을 수 있음({{ category[1].size }})</a
    >
  </li>
  {% endif %} {% endfor %}
</ul>
{% endraw %}
```

<br><br>

# 잡다한 메모

## git 관련

- 변경사항 적용
  - git add .
  - git commit
  - git push blog

<br>

## 마크다운 언어

### header 표시시 유의사항

- toc(table of contents)는 각 포스트의 헤더를 파악하고, 이를 목차로 띄워준다.
- header를 여러개 작성할 때 문제가 발생할 수 있다.
- `#제목1  ##제목2  ###제목3`처럼 순서대로 작성하면 문제 없다.
- 하지만 `#제목1 ###제목3 #제목1`처럼 두 번째 헤더가 없고 맘대로 이렇게 쓰면, toc에서 폰트가 적용이 안 되었다.

### raw 메서드

- liquid 코드 실행 정지하기
- 아래처럼 liquid 언어가 포함되어 있을 때 코드 블록을 만들어도 안 liquid 언어는 안 보임
- {raw} 와 {endraw} 사이에 표시할 내용 집어넣기(단 실제로 쓸 때는 raw와 endraw 양 옆에 %를 써야 됨)
- raw 안 썼을 때

```html
<ul>
  {% for category in site.categories %} {% if category[0] == "카테고리이름" %}
  <li>
    <a href="/categories/BlogDev" class=""
      >맘대로 변형해서 이름 지을 수 있음({{ category[1].size }})</a
    >
  </li>
  {% endif %} {% endfor %}
</ul>
```

- raw 썼을 때

```html
{% raw %}
<ul>
  {% for category in site.categories %} {% if category[0] == "카테고리이름" %}
  <li>
    <a href="/categories/BlogDev" class=""
      >맘대로 변형해서 이름 지을 수 있음({{ category[1].size }})</a
    >
  </li>
  {% endif %} {% endfor %}
</ul>
{% endraw %}
```

<br>

## 기타

- jekyll serve
  - 로컬호스트로 push하지 않고 변경사항 확인 가능
