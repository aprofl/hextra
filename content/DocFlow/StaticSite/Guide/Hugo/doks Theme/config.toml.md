doks 의 설정 파일은 `themes/doks/config/_default/Hugo.toml` 입니다.
Hugo는 프로젝트 루트의 `config.toml` 파일을 우선적으로 참조합니다. 만약 프로젝트 루트에 `config.toml` 파일을 새로 생성하면, Hugo는 이 파일을 주요 설정 파일로 사용합니다. `config.toml` 파일이 없는 경우에만 테마 디렉토리의 `_default` 설정 파일을 참조합니다.
따라서 프로젝트 루트에 `config.toml` 파일을 생성하면, `_default`에 있는 설정 파일을 참조하지 않고 새로 생성한 `config.toml` 파일을 사용하게 됩니다. 필요한 모든 설정을 이 파일에 포함시켜야 합니다.
하지만, 필요한 기본 설정을 프로젝트 루트의 `config.toml` 파일에 포함시키고, 추가 설정을 테마에서 가져오는 방식으로 사용할 수 있습니다. 이 경우, Hugo의 설정 병합 기능을 활용할 수 있습니다.

## 내용

### 기본 설정

```toml file:config.toml
title = "My Docs"                  # 사이트의 제목
baseurl = "http://localhost/"      # 사이트의 기본 URL
canonifyURLs = false               # 모든 URL을 절대 URL로 변환
disableAliases = true              # Hugo에서 자동 생성된 별칭 비활성화
disableHugoGeneratorInject = true  # Hugo generator 메타 태그 비활성화
enableEmoji = true                 # 이모지 지원 활성화
enableGitInfo = false              # Git 정보를 포함 비활성화
enableRobotsTXT = true             # robots.txt 파일 생성 활성화
languageCode = "en-US"             # 사이트의 기본 언어
paginate = 10                      # 페이지네이션 설정
rssLimit = 10                      # RSS 피드에 포함될 항목 수
summarylength = 20                 # 콘텐츠 요약의 길이
```

### 다국어 지원 설정

```toml file:config.toml
defaultContentLanguage = "en"               # 기본 콘텐츠 언어
disableLanguages = ["de", "nl"]             # 비활성화할 언어 목록
defaultContentLanguageInSubdir = false      # 기본 언어를 하위 디렉토리에 포함할지 여부
```

### 저작권 및 빌드 설정

```toml file:config.toml
copyRight = "Copyright (c) 2020-2024 Hyas"  # 저작권 정보
[build.buildStats]
  enable = true                             # 빌드 통계 활성화
```

### 출력 형식 설정

```toml file:config.toml
[outputs]
  home = ["HTML", "RSS", "searchIndex"]     # 홈 페이지 출력 형식
  section = ["HTML", "RSS", "SITEMAP"]      # 섹션 페이지 출력 형식
[outputFormats.searchIndex]
  mediaType = "application/json"
  baseName = "search-index"
  isPlainText = true
  notAlternative = true
[outputFormats.SITEMAP]
  mediaType = "application/xml"
  baseName = "sitemap"
  isHTML = false
  isPlainText = true
  noUgly = true
  rel  = "sitemap"
```

### 사이트맵 설정

```toml file:config.toml
[sitemap]
  changefreq = "monthly"      # 사이트맵의 기본 변경 빈도
  filename = "sitemap.xml"    # 사이트맵 파일 이름
  priority = 0.5              # 사이트맵의 기본 우선 순위
```

### 캐시설정

```toml file:config.toml
[caches]
  [caches.getjson]
    dir = ":cacheDir/:project"   # 캐시 디렉토리 설정
    maxAge = -1                  # 캐시의 최대 수명
```

### [텍소노미 설정](Taxonomies.md)

```toml file:config.toml
[taxonomies]
  contributor = "contributors"   # 기여자 분류
  category = "categories"        # 카테고리 분류
  tag = "tags"                   # 태그 분류
```

### [영구 링크 설정](Permalinks.md)

```toml file:config.toml
[permalinks]
  blog = "/blog/:slug/"            # 블로그 퍼머링크 구조
  docs = "/docs/:sections[1:]/:slug/" # 문서 퍼머링크 구조
```

### HTML 압축 설정

```toml file:config.toml
[minify.tdewolff.html]
  keepWhitespace = false          # HTML에서 공백을 유지하지 않음
```

### 관련 콘텐츠 설정

```toml file:config.toml
[related]
  threshold = 80                  # 관련 콘텐츠의 임계값
  includeNewer = true             # 새로운 관련 콘텐츠 포함 여부
  toLower = false                 # 소문자로 변환 여부
    [[related.indices]]
      name = "categories"         # 관련 콘텐츠 인덱스 - 카테고리
      weight = 100                # 가중치
    [[related.indices]]
      name = "tags"               # 관련 콘텐츠 인덱스 - 태그
      weight = 80                 # 가중치
    [[related.indices]]
      name = "date"               # 관련 콘텐츠 인덱스 - 날짜
      weight = 10                 # 가중치
```

### [이미지 설정](Imaging.md)

```toml file:config.toml
[imaging]
  anchor = "Center"               # 이미지 앵커 위치
  bgColor = "#ffffff"             # 이미지 배경색
  hint = "photo"                  # 이미지 힌트
  quality = 85                    # 이미지 품질
  resampleFilter = "Lanczos"      # 이미지 리샘플링 필터
```