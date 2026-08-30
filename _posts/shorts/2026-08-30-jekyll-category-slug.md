---

title:  "한글 카테고리가 URL을 망치고 있었다"
excerpt: "한글 카테고리를 쓰면 URL이 퍼센트 인코딩으로 깨진다. 값과 표시를 분리하는 방법."
categories:
- shorts

tags:
- Jekyll
- 블로그
- minimal-mistakes

header:
  teaser: /assets/images/jekyll-category-slug/teaser.jpg
  overlay_image: /assets/images/jekyll-category-slug/teaser.jpg
  overlay_filter: 0.5

last_modified_at: 2026-08-30T23:30:00

---

블로그 글 주소를 오랜만에 들여다보다가 좀 이상한 걸 발견했어요.

```
/객체지향 이야기/oop-1-intro/
```

카테고리를 한글로 적어놨더니 그게 그대로 경로가 된 거죠. 심지어 공백까지 들어가 있어서, 실제로 브라우저 주소창에 찍히는 건 `%EA%B0%9D%EC%B2%B4...%20...` 같은 암호문이었습니다. 링크를 복사해서 어디 붙여넣으면 아무도 무슨 글인지 알 수 없는 형태가 되고요.

## 원인은 permalink 한 줄

Jekyll의 `_config.yml`에 이렇게 적혀 있었습니다.

```yaml
permalink: /:categories/:title/
```

`:categories`가 포스트 frontmatter의 `categories` 값을 그대로 가져다 씁니다. 그러니 거기에 "객체지향 이야기"라고 적으면 경로도 그렇게 되는 거예요. 당연한 동작인데, 카테고리를 처음 정할 때는 화면에 어떻게 보일지만 생각했지 주소가 될 거라는 생각은 못 했습니다.

## 그런데 표시는 한글로 하고 싶다

주소만 영문으로 바꾸면 되지 않나 싶지만, 그러면 글 하단 카테고리 링크에도 `oop`라고 뜹니다. 읽는 사람 입장에서는 "객체지향 이야기"가 훨씬 낫죠.

그래서 **값과 표시를 분리**했습니다. 카테고리 값은 영문 slug로 두고, 표시할 때만 매핑 테이블을 거치게요.

`_data/category_names.yml`:

```yaml
oop: "객체지향 이야기"
shorts: "토막상식"
```

그리고 카테고리를 출력하는 자리에서 이 매핑을 조회합니다.

{% raw %}
```liquid
{% assign category_label = site.data.category_names[category_word] | default: category_word %}
<a href="...">{{ category_label }}</a>
```
{% endraw %}

`default` 필터가 폴백입니다. 매핑에 없는 카테고리는 slug가 그대로 나오니까, 새 카테고리를 추가하면서 매핑을 깜빡해도 페이지가 깨지진 않아요.

## 한 가지 함정

Liquid에서 이렇게 쓰면 조회가 안 됩니다.

{% raw %}
```liquid
{{ site.data.category_names[category[0]] }}
```
{% endraw %}

대괄호 안에 또 대괄호가 들어간 형태를 Liquid가 제대로 파싱하지 못하더군요. 조용히 빈 값이 나와서 한참 헤맸습니다. 변수로 한 번 빼주면 해결됩니다.

{% raw %}
```liquid
{% assign category_key = category[0] %}
{% assign category_label = site.data.category_names[category_key] | default: category_key %}
```
{% endraw %}

## 결국

주소는 기계가 읽고, 표시는 사람이 읽습니다. 둘을 같은 값으로 묶어두면 언젠가 한쪽이 불편해져요.

이미 발행한 글의 카테고리를 바꾸면 URL이 바뀌니까, 기존 유입이 있다면 리다이렉트를 먼저 준비해두는 게 맞습니다. 저는 어차피 한글+공백 주소라 검색 유입이 없다시피 해서 그냥 갈아엎었어요.
