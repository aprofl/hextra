`hugo.toml` 파일은 Hugo static site generator를 설정하는 구성 파일입니다. 
각 항목은 사이트의 동작과 모양새를 조절합니다.

```toml file:hugo.toml
baseURL = "https://example.docsy.dev/"
title = "Aprofl"
contentDir = "content"
defaultContentLanguage = "ko"
defaultContentLanguageInSubdir = true
enableMissingTranslationPlaceholders = true
enableRobotsTXT = true
enableGitInfo = true
[taxonomies]
tag = "tags"
category = "categories"
[params.taxonomy]
taxonomyCloud = ["tags", "categories"]
taxonomyCloudTitle = ["Tag Cloud", "Categories"]
taxonomyPageHeader = ["tags", "categories"]
# Highlighting config
pygmentsCodeFences = true
pygmentsUseClasses = false
pygmentsUseClassic = false
pygmentsStyle = "tango"
[permalinks]
blog = "/:section/:year/:month/:day/:slug/"
[imaging]
resampleFilter = "CatmullRom"
quality = 75
anchor = "smart"
[languages]
  [languages.ko]
  languageName = "한국어"
  weight = 1
    [languages.ko.params]
    title = "Aprofl"
    description = "생산성을 위한 블로그"
  [languages.en]
  languageName = "English"
  weight = 2
    [languages.en.params]
    title = "Aprofl"
    description = "Blog for Productivity"
[markup]
  [markup.goldmark]
    [markup.goldmark.parser.attribute]
      block = true
    [markup.goldmark.renderer]
      unsafe = true
  [markup.highlight]
    style = "tango"
[outputs]
section = ["HTML", "print", "RSS"]
[params]
privacy_policy = "https://policies.google.com/privacy"
version_menu = "Releases"
archived_version = false
version = "0.0"
url_latest_version = "https://example.com"
github_repo = "https://github.com/google/docsy-example"
github_project_repo = "https://github.com/google/docsy"
github_branch = "main"
gcs_engine_id = "d72aa9b2712488cc3"
offlineSearch = false
prism_syntax_highlighting = false
[params.copyright]
  authors = "Aprofl"
  from_year = 2024
# User interface configuration
[params.ui]
breadcrumb_disable = false
navbar_logo = true
navbar_translucent_over_cover_disable = false
sidebar_menu_compact = false
sidebar_search_disable = false
disable_search = false
[params.ui.feedback]
enable = false
yes = 'Glad to hear it! Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'
no = 'Sorry to hear that. Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'
[params.ui.readingtime]
enable = false
[module]
  [module.hugoVersion]
    extended = true
    min = "0.110.0"
  [[module.imports]]
    path = "github.com/google/docsy"
    disable = false
[menu]
[[menu.main]]
    identifier = "blog"
    name = "Blog"
    url = "/ko/blog/"
    weight = 0
[[menu.main]]
    identifier = "obsidian"
    name = "Obsidian"
    url = "/ko/obsidian/"
    weight = 1
```

### 기본 설정
- `baseURL = "https://example.docsy.dev/"`
	- 사이트의 기본 URL을 설정합니다.
- `title = "Aprofl"`
	- 사이트의 제목을 설정합니다.
- `contentDir = "content"`
	- 컨텐츠 디렉토리의 경로를 설정합니다.
- `defaultContentLanguage = "ko"`
	- 기본 언어를 한국어로 설정합니다.
- `defaultContentLanguageInSubdir = true
	-  기본 언어를 하위 디렉토리에 포함시킵니다.
- `enableMissingTranslationPlaceholders = true`
	- 번역이 누락된 경우, 플레이스홀더를 표시합니다.
- `enableRobotsTXT = true`
	- `robots.txt` 파일을 생성하여 검색 엔진에 사이트 인덱싱 방법을 알려줍니다.
- `enableGitInfo = true`
	- 마지막 수정 시간 등의 Git 정보를 사용합니다.
### [분류 및 태그 설정](Taxonomies.md)
- `[taxonomies]`: 사이트의 분류 체계를 정의합니다.
    - `tag = "tags"`
    - `category = "categories"`
- `[params.taxonomy]`: 분류 관련 추가 설정입니다.
    - `taxonomyCloud = ["tags", "categories"]`
	    - 분류 클라우드를 설정합니다.
    - `taxonomyCloudTitle = ["Tag Cloud", "Categories"]`
	    - 분류 클라우드의 제목을 설정합니다.
    - `taxonomyPageHeader = ["tags", "categories"]`
	    - 페이지 헤더에 분류를 표시합니다.
### 코드 하이라이팅 설정
- `pygmentsCodeFences = true`
	- 코드 펜스를 사용할 때 Pygments를 사용하여 코드 블록을 하이라이팅합니다. 
	- Pygments는 다양한 프로그래밍 언어의 소스 코드를 구문 강조(문법 하이라이트)하기 위해 사용되는 오픈 소스 라이브러리입니다.
- `pygmentsUseClasses = false`
	- Pygments에서 직접 스타일을 적용하는 대신, CSS 클래스를 사용하도록 설정합니다. 
	- `false`로 설정하면 스타일이 직접 HTML 요소에 적용됩니다.
- `pygmentsUseClassic = false`
	- Pygments 대신 새로운 Chroma 하이라이터를 사용하도록 설정합니다. 
	- Chroma는 Go 언어로 작성된 하이라이팅 라이브러리로, 더 빠르고 간단한 설정을 제공합니다.
- `pygmentsStyle = "tango"`
	- 하이라이팅 스타일을 지정합니다. 
	- "tango"는 Pygments의 여러 스타일 중 하나입니다. 
	- 다른 스타일을 사용하고 싶다면 [Pygments 스타일 목록](https://help.farbox.com/pygments.html)을 참고하여 원하는 스타일을 설정할 수 있습니다.
### [URL 형식 설정](Permalinks.md)
- `[permalinks]`: 섹션별 URL 형식을 설정합니다.
    - `blog = "/:section/:year/:month/:day/:slug/"
	    - 블로그 게시물의 URL 형식을 설정합니다.
### [이미지 처리 설정](Imaging.md)
- `[imaging]`: 이미지 처리 설정입니다.
    - `resampleFilter = "CatmullRom"`: 이미지 리샘플링 필터를 설정합니다.
    - `quality = 75`: 이미지 품질을 설정합니다.
    - `anchor = "smart"`: 이미지 앵커를 스마트하게 설정합니다.
### 언어 설정
- `[languages]`: 다국어 지원을 설정합니다.
    - `[languages.ko]`: 한국어 설정입니다.
        - `languageName = "한국어"`: 언어 이름을 설정합니다.
        - `weight = 1`: 언어의 우선순위를 설정합니다.
        - `[languages.ko.params]`: 한국어 페이지의 추가 매개변수입니다.
            - `title = "Aprofl"`: 한국어 페이지의 제목을 설정합니다.
            - `description = "생산성을 위한 블로그"`: 한국어 페이지의 설명을 설정합니다.
    - `[languages.en]`: 영어 설정입니다.
        - `languageName = "English"`: 언어 이름을 설정합니다.
        - `weight = 2`: 언어의 우선순위를 설정합니다.
        - `[languages.en.params]`: 영어 페이지의 추가 매개변수입니다.
            - `title = "Aprofl"`: 영어 페이지의 제목을 설정합니다.
            - `description = "Blog for Productivity"`: 영어 페이지의 설명을 설정합니다.
### [마크업 설정](MarkUp.md)
- `[markup]`: 마크업 관련 설정입니다.
    - `[markup.goldmark]`: Goldmark 마크다운 렌더러 설정입니다.
        - `[markup.goldmark.parser.attribute]`: Goldmark 파서 설정입니다.
            - `block = true`: 블록 수준의 속성 파서를 활성화합니다.
        - `[markup.goldmark.renderer]`: Goldmark 렌더러 설정입니다.
            - `unsafe = true`: 안전하지 않은 HTML 렌더링을 허용합니다.
    - `[markup.highlight]`: 코드 하이라이팅 설정입니다.
        - `style = "tango"`: 하이라이팅 스타일을 `tango`로 설정합니다.
### 사이트 매개변수
- `[params]`: 사이트의 다양한 매개변수 설정입니다.
    - `privacy_policy = "https://policies.google.com/privacy"`: 개인정보 보호정책 URL을 설정합니다.
    - `version_menu = "Releases"`: 버전 선택 메뉴 제목을 설정합니다.
    - `archived_version = false`: 아카이브된 버전 배너 표시 여부를 설정합니다.
    - `version = "0.0"`: 현재 문서 세트의 버전 번호를 설정합니다.
    - `url_latest_version = "https://example.com"`: 최신 버전의 문서 링크를 설정합니다.
    - `github_repo = "https://github.com/google/docsy-example"`: GitHub 저장소 URL을 설정합니다.
    - `github_project_repo = "https://github.com/google/docsy"`: 관련 프로젝트 GitHub 저장소 URL을 설정합니다.
    - `github_branch= "main"`: GitHub 브랜치를 설정합니다.
    - `gcs_engine_id = "d72aa9b2712488cc3"`: Google Custom Search Engine ID를 설정합니다.
    - `offlineSearch = false`: Lunr.js 오프라인 검색을 활성화합니다.
    - `prism_syntax_highlighting = false`: Prism 구문 강조를 비활성화합니다.
- `[params.copyright]`: 저작권 정보를 설정합니다.
        - `authors = "Aprofl"`: 저작권 소유자를 설정합니다.
        - `from_year = 2024`: 저작권 시작 연도를 설정합니다.
- `[params.ui]`: 사용자 인터페이스 설정입니다.
	- `breadcrumb_disable = false`: 브레드크럼 내비게이션을 활성화합니다.
	- `navbar_logo = true`: 네비게이션 바에 로고를 표시합니다.
	- `navbar_translucent_over_cover_disable = false`: 커버 위의 네비게이션 바를 반투명하게 설정합니다.
	- `sidebar_menu_compact = false`: 사이드바 메뉴를 컴팩트하게 설정합니다.
	- `sidebar_search_disable = false`: 사이드바 검색을 비활성화합니다.
	- `disable_search = false`: 검색 기능을 활성화합니다.
- `[params.ui.feedback]`: 피드백 설정입니다.
		- `enable = false`: 피드백 기능을 비활성화합니다.
		- `yes = 'Glad to hear it! Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'`: 긍정 피드백 메시지를 설정합니다.
		- `no = 'Sorry to hear that. Please <a href="https://github.com/USERNAME/REPOSITORY/issues/new">tell us how we can improve</a>.'`: 부정 피드백 메시지를 설정합니다.
- `[params.ui.readingtime]`: 읽기 시간 설정입니다.
		- `enable = false`: 읽기 시간 표시를 비활성화합니다.
### 모듈 설정
- `[module]`: Hugo 모듈 설정입니다.
    - `[module.hugoVersion]`: Hugo 버전 설정입니다.
        - `extended = true`: Hugo Extended 버전을 사용합니다.
        - `min = "0.110.0"`: 최소 Hugo 버전을 설정합니다.
    - `[[module.imports]]`: 모듈을 가져옵니다.
        - `path = "github.com/google/docsy"`: Docsy 테마를 가져옵니다.
        - `disable = false`: 모듈을 비활성화합니다.
    - `[[module.imports]]`: 모듈을 가져옵니다.
        - `path = "../../Blog"`: 로컬 Blog 모듈을 가져옵니다.
        - `disable = false`: 모듈을 비활성화합니다.
### 메뉴 설정
- `[menu]`: 사이트 메뉴를 설정합니다.
    - `[[menu.main]]`: 메인 메뉴 항목을 추가합니다.
        - `identifier = "blog"`: 블로그 메뉴 항목의 식별자를 설정합니다.
        - `name = "Blog"`: 블로그 메뉴 항목의 이름을 설정합니다.
        - `url = "/ko/blog/"`: 블로그 메뉴 항목의 URL을 설정합니다.
        - `weight = 0`: 블로그 메뉴 항목의 정렬 가중치를 설정합니다.
    - `[[menu.main]]`: 메인 메뉴 항목을 추가합니다.
        - `identifier = "obsidian"`: Obsidian 메뉴 항목의 식별자를 설정합니다.
        - `name = "Obsidian"`: Obsidian 메뉴 항목의 이름을 설정합니다.
        - `url = "/ko/obsidian/"`: Obsidian 메뉴 항목의 URL을 설정합니다.
        - `weight = 1`: Obsidian 메뉴 항목의 정렬 가중치를 설정합니다.
/blog_docsy/
/blog_docsy/content/
/blog_docsy/content/ko
/blog_docsy/content/ko/Blog
/blog_docsy/content/ko/Blog/folder1/
/blog_docsy/content/ko/Blog/folder2/
/blog_docsy/content/ko/Obsidian/
/blog_docsy/content/ko/Obsidian/index.md
/blog_docsy/content/ko/Obsidian/folder1/
/blog_docsy/content/ko/Obsidian/folder2/
/blog_docsy/content/ko/StaticSite/
/blog_docsy/content/ko/StaticSite/folder1/
/blog_docsy/content/ko/StaticSite/folder2/
/blog_docsy/layouts/
/blog_docsy/layouts/section/
/blog_docsy/layouts/section/obsidian.html
/blog_docsy/hugo.toml
/blog_docsy/config.yaml
