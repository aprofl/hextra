## 개요

Hugo의 테마에서 섹션을 이해하는 것은 중요한 개념입니다. 섹션을 활용하여 사이트의 구조를 관리하고 콘텐츠를 조직화하며 사이트 내비게이션을 쉽게 구성할 수 있습니다. 섹션별로 레이아웃과 메뉴를 설정하여 사용자에게 일관된 경험을 제공할 수 있습니다. 

## 섹션(Sections)

Hugo에서는 콘텐츠 디렉토리의 하위 폴더를 섹션이라고 합니다. 예를 들어, `content/docs`, `content/blog`, `content/projects`와 같은 폴더들이 섹션입니다. Doks 테마는 이러한 섹션을 활용하여 다양한 페이지와 메뉴 항목을 생성합니다.

## 기본 섹션 구조

Doks 테마는 보통 다음과 같은 섹션 구조를 가집니다:

```sh
content/
├── docs/
│   ├── _index.md
│   ├── guide/
│   │   ├── _index.md
│   │   └── installation.md
│   └── tutorial/
│       ├── _index.md
│       └── getting-started.md
├── blog/
│   ├── _index.md
│   └── my-first-post.md
└── projects/
    ├── _index.md
    └── my-project.md
```

## 섹션의 역할

각 섹션은 해당 섹션의 루트에 `_index.md` 파일을 가질 수 있으며, 이는 섹션의 메인 페이지 역할을 합니다. 예를 들어, `content/docs/_index.md`는 `/docs/` URL에 해당하는 메인 페이지를 생성합니다. 섹션 내의 다른 파일들은 하위 페이지로 작동합니다.

## 메뉴 구성

일반적으로 `config.toml` 또는 `config.yaml` 파일을 통해 메뉴를 구성합니다. 메뉴 항목은 섹션과 매핑되어 사이트의 내비게이션을 구성합니다. 예를 들어, 다음과 같이 설정할 수 있습니다:

```toml
[menu]
  [[menu.main]]
    name = "Docs"
    url = "/docs/"
    weight = 1
  [[menu.main]]
    name = "Blog"
    url = "/blog/"
    weight = 2
  [[menu.main]]
    name = "Projects"
    url = "/projects/"
    weight = 3
```

doks 테마에서는 `/config/_default/menus` 폴더에 언어별 별도 파일로 분리되어 있습니다. 
 
```toml
[[main]]
  name = "Docs"
  url = "/docs/guides/example-guide/"
# url = "/docs/1.0/prologue/introduction/"
  weight = 10

[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 30
```

## 레이아웃 및 템플릿

각 섹션은 특정 레이아웃과 템플릿을 사용할 수 있습니다. 예를 들어, `layouts/blog/single.html`은 `content/blog/` 아래에 있는 Markdown 파일들이 사용할 템플릿입니다. 이를 통해 섹션별로 다른 디자인과 레이아웃을 적용할 수 있습니다.

## 예제

- `content/docs/guide/installation.md`의 예제:

```md
# Installation Guide

이 문서는 Doks 테마를 설치하는 방법에 대해 설명합니다.
```

`config.toml`에서 메뉴 항목 추가:

```toml
[menu]
  [[menu.main]]
    name = "Installation Guide"
    url = "/docs/guide/installation/"
    weight = 4
```