# Architecture

> 프로젝트의 아키텍처 인덱스. architect-sync 스킬로 관리됩니다.

## Bird's Eye View

cogito90.github.io — Jekyll 정적 블로그. minimal-mistakes 테마 위에 Glacier 커스텀 레이어를 덮어 쓰고 GitHub Pages로 배포한다. 개발·철학·AI 세 갈래의 글을 다루며, 분류 축은 카테고리 하나뿐이다.

## Code Map

```
.
├─ _config.yml          사이트 설정 — permalink, exclude, 아카이브 방식, 저자, 공용 OG 카드
├─ _data/               화면 문자열 데이터
│  ├─ navigation.yml       상단 내비 (카테고리 3개)
│  ├─ category_names.yml   카테고리 slug → 표시명 매핑 (표시명의 단일 진실 원천)
│  └─ ui-text.yml          테마 로케일 문자열
├─ _layouts/            페이지 골격 — single(글), category·categories·archive(목록), home, default
├─ _includes/           조각 — archive-single(목록 카드), gl-sidebar(같은 카테고리 글),
│                       category-list(글 하단 배지), page__taxonomy, seo
├─ _pages/              수동 생성 페이지 — 카테고리 아카이브 3개 + /categories/ 인덱스 + 404
├─ _posts/<category>/   글 원본. 하위 폴더는 정리용이고 카테고리는 front matter가 결정한다
├─ _sass/
│  ├─ minimal-mistakes/    테마 원본 — 수정하지 않는다
│  └─ custom/_glacier.scss 전면 재정의 레이어
└─ assets/              이미지·JS. 글별 이미지는 assets/images/<post-slug>/ 에 둔다
```

## Cross-Cutting Concerns

- **카테고리 표시명**: 렌더 위치 4곳(목록 카드, 글 상단 kicker, 글 하단 배지, 카테고리 인덱스)이 모두 `_data/category_names.yml` 매핑을 통과해야 한다. 한 곳이라도 raw slug를 출력하면 같은 글 안에서 표기가 어긋난다
- **공유 카드**: `_includes/seo.html`이 `header.og_image → header.overlay_image → header.image → header.teaser → site.og_image` 순으로 떨어진다. 글이 이미지를 갖지 않아도 공용 OG 카드가 채운다
- **테마 전환**: 라이트 기본. 다크는 마스트헤드 토글로만 전환하고 선택을 기억한다 (OS 설정을 따라가지 않는다)
- **빌드 제외**: 개발용 파일(`AGENTS.md`, `ARCHITECT.md`, `slophyspec/` 등)은 `_config.yml`의 `exclude`에 넣는다. 빠뜨리면 공개 사이트에 그대로 배포된다

## Invariants & Design Decisions

- permalink이 `/:categories/:title/`이라 **카테고리 slug가 URL의 첫 경로 세그먼트**다. slug를 바꾸면 기존 글 URL이 깨지며, 리다이렉트 장치(`jekyll-redirect-from`)는 도입하지 않았다. 표시만 바꿀 때는 `category_names.yml`을 건드린다
- 카테고리는 `ai` / `philosophy` / `tech` 셋뿐이다. 소재가 아니라 **글의 결론이 향하는 곳**으로 고른다 — AI를 소재로 썼어도 결론이 사람·커리어로 향하면 `philosophy`다
- **태그를 쓰지 않는다.** 글마다 분류를 고민하는 비용이 분류의 효용보다 컸다
- 글별 teaser 이미지는 **선택**이다. 사이트 안에서는 렌더되지 않고 OG 공유 카드로만 쓰인다
- 글 상단에 **오버레이 히어로를 두지 않는다** — 제목 대비가 이미지 밝기에 종속되고 본문 진입이 밀린다. `.gl-post-head` 텍스트 블록이 그 자리를 대신한다
- `_sass/minimal-mistakes/**` 테마 원본은 수정하지 않는다. 모든 커스텀은 `_sass/custom/_glacier.scss`에 덮어쓰기로 넣는다
