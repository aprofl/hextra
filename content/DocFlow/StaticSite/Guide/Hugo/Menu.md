## 섹션 추가

- `config/_default/menus/menus.en.toml` 파일에서 섹션을 관리할 수 있습니다.
- 섹션 추가 시 url 은 폴더 경로와 일치해야 합니다.

```toml file:menus.en.toml
[[main]]
  name = "DocFlow"
  url = "/DocFlow/about/about"
  weight = 10

[[main]]
  name = "Docs"
  url = "/docs/"
  weight = 20

[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 30
```

![hugo_section.png](/Resources/hugo_section.png)

## 메뉴 관리

Doks 테마에서는 각 섹션별로 다른 레이아웃을 적용하고 있습니다.
예를 들어, docs 섹션에서는 사이드바가 활성화되지만, Blog 테마에서는 활성화되지 않습니다.
마찬가지로 추가 된 DocFlow 섹션에서도 사이드바는 보이지 않습니다.
섹션이 여러개인 경우 레이아웃을 달리 적용하는 방법에 대해서는 조금 복잡할 수 있으므로 여기에서는 다루지 않으며, Docs 섹션을 이용하여 메뉴를 구성하였습니다.
변경된 구성은 다음과 같습니다.

```toml
[[main]]
  name = "DocFlow"
  url = "/docs/about/about"
  weight = 10

[[main]]
  name = "Blog"
  url = "/blog/"
  weight = 30
```