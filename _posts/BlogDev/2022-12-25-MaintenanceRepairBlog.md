---
title: "[BlogDev] 깃허브 블로그(minimal-mistakes) 유지보수 및 활용에 도움되는 정보"
excerpt: "계속해서 사용하면서 도움이 될 것 같은 정보를 발견할 때마다 업데이트"

categories:
  - BlogDev
tags:
  - [minimal-mistakes, Github]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-25
last_modified_at: 2023-01-20
---

<br>

# 블로그 유지보수 및 개선할 때 정보 찾기

- [공식 사이트](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)를 적극 참고하기
- 단순히 블로그에 있는 정보만으로는 부족할 때가 많음

<br><br>

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

# git 관련

- 변경사항 적용
  - git add .
  - git commit
  - git push blog

<br><br>

# 마크다운 언어

## header 표시시 유의사항

- toc(table of contents)는 각 포스트의 헤더를 파악하고, 이를 목차로 띄워준다.
- header를 여러개 작성할 때 문제가 발생할 수 있다.
- `#제목1  ##제목2  ###제목3`처럼 순서대로 작성하면 문제 없다.
- 하지만 `#제목1 ###제목3 #제목1`처럼 두 번째 헤더가 없고 맘대로 이렇게 쓰면, toc에서 폰트가 적용이 안 되었다.

## md 파일에서 <> 안에 글자 쓰면 가끔 오류

- post의 title에 <>를 쓰면 제목이 이상하게 표시는 경우가 있다.
- md 파일에서 `#`으로 표시되는 제목에 <> 안에 글자를 넣으면 이상해 짐도 경험했다.
- ex)

```md
title: "[스프링 입문(김영한)] <프로젝트 생성> ~ <회원 리포지토리 테스트 케이스 작성>"
```

=> <프로젝트 생성=""> ~ <회원 리포지토리="" 테스트="" 케이스="" 작성="">으로 페이지에 출력됨.

## Code block is improperly handled and generates Liquid syntax error

- 내가 jsp 관련 코드를 쓴 후에 오류가 나서, 일단 jsp 코드 블록 안 내용을 다 raw tag로 일단 감쌌다. 그 후 실행하니 오류가 사라졌다.
- With Jekyll, Markdown files are fist processed by Liquid, and then Markdown, so Liquid syntax is interpreted, even within Markdown code-blocks.
- To avoid the problem, the raw tag can be used to disable Liquid processing

## raw 메서드

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

<br><br>

# 특정 포스트 비공개하기

- 비공개: Post 등록 시 다음과 같이 포스트 header에 `published: false`를 추가한다.

<br><br>

# 로컬에서 보기

- jekyll serve
  - 로컬호스트로 push하지 않고 변경사항 확인 가능
  - (css 파일 수정할 때 도움되는 정보) jekyll serve를 쳐서 localhost로 연 블로그의 경우, 크롬 개발자 도구를 이용해서 선택한 요소가 어떤 css 파일의 영향을 받는지 구체적으로 알 수 있음.
    - 일반적으로 선택한 요소가 영향받은 css파일은 주로 main.css로 나오는데, jekyll serve로 열면 구체적인 파일 이름(ex- \_base.sass)이 나옴.
- 추가 옵션
  - -unpublished: 비공개된 포스트 보기
  - --incremental: 수정된 부분만 반영하기. 선택사항
  - --watch: 수정되면 전체를 다시 반영하는 거. 선택사항
- --incremental
  - 이거 하면 빌드 속도는 빨라짐
  - 하지만 몇몇 부분은 수정을 했는데도 반영되지 않는 문제가 발생함.

<br><br>

# 및 비공개 포스트 보기

- 비공개 포스트 보기
  - jekyll serve -unpublished
    - 추가로 "--incremental"나 "--watch"를 쓸 수도 있다.
    - jekyll serve만 하면 --watch는 자동으로 됐지만, -unpublished를 붙이면 --watch를 직접 써줘야지 작동하는 것 같다.
  - 근데 귀찮다... 명령어도 길고, 제대로 수정되는 게 반영되지 않는 듯하다.
    - 내가 블로글 제목을 바꾸면 해당 페이지 내에서는 제목이 바뀐다.
    - 하지만 글 목록에서 해당 글의 제목은 반영이 안된다.
