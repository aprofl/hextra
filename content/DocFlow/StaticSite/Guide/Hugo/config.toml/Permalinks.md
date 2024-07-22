`[permalinks]` 는 사이트의 각 섹션에 대해 URL 구조를 정의하는 데 사용되는 설정으로,
이를 통해 더 일관되고 사용자 친화적인 URL을 만들 수 있습니다. 
각 섹션별로 다른 URL 패턴을 지정할 수 있으며, 이를 통해 SEO 최적화와 사용자 경험을 개선할 수 있습니다. 

## Permalinks 설정

Hugo에서 `permalinks` 섹션은 각 섹션의 URL 패턴을 정의합니다. 
여기서 섹션은 콘텐츠 디렉토리 내의 폴더를 의미합니다. 
예를 들어, `content/blog` 폴더는 `blog` 섹션이 됩니다.
#### 예제 설정

```toml
[permalinks]
blog = "/:section/:year/:month/:day/:slug/"
```

위 설정은 `blog` 섹션의 콘텐츠 URL 형식을 정의합니다.
### URL 패턴의 각 요소 설명
- `/:section/`: 콘텐츠가 속한 섹션 이름입니다. 예를 들어, `content/blog` 섹션은 `blog`가 됩니다.
- `/:year/`: 콘텐츠의 연도입니다. 콘텐츠가 게시된 연도를 포함합니다.
- `/:month/`: 콘텐츠의 월입니다. 콘텐츠가 게시된 월을 포함합니다.
- `/:day/`: 콘텐츠의 일입니다. 콘텐츠가 게시된 날짜를 포함합니다.
- `/:slug/`: 콘텐츠의 슬러그입니다. 일반적으로 콘텐츠의 제목을 기반으로 하며, URL에 사용할 수 있는 형태로 변환됩니다.
### 예제 콘텐츠
예를 들어, `content/blog/hello-world.md` 파일이 있다면, 이 파일의 메타데이터는 다음과 같습니다:

이 설정에 따르면, 이 블로그 게시물의 URL은 다음과 같이 생성됩니다:

```sh
/blog/2024/06/13/hello-world/
```

### 다른 섹션에 대한 Permalinks 설정
각 섹션별로 다른 URL 패턴을 정의할 수 있습니다. 
예를 들어, `content/obsidian` 섹션에 대해 다른 URL 패턴을 설정하고 싶다면 다음과 같이 설정할 수 있습니다

```toml
[permalinks]
blog = "/:section/:year/:month/:day/:slug/"
obsidian = "/:section/:title/"
```

위 설정에서는 `obsidian` 섹션의 URL 패턴을 `/obsidian/제목/` 형식으로 정의합니다.
Hugo에서 여러 섹션에 대해 동일한 permalink 설정을 적용하려면, 각 섹션별로 명시적으로 설정을 정의해야 합니다. 여러 섹션에 동일한 permalink 설정을 한 번에 적용하는 기능은 제공하지 않습니다.
예를 들어, 블로그와 옵시디언 섹션이 있고, 두 섹션 모두 동일한 permalink 패턴을 사용하고 싶다면 다음과 같이 설정합니다:

```toml
[permalinks]
blog = "/:section/:year/:month/:day/:slug/"
obsidian = "/:section/:year/:month/:day/:slug/"
```

## 사용 가능한 Permalink 변수

Hugo에서는 여러 가지 변수를 사용하여 URL 패턴을 정의할 수 있습니다. 주요 변수들은 다음과 같습니다:
- `:year`: 콘텐츠가 게시된 연도 (예: 2024)
- `:month`: 콘텐츠가 게시된 월 (예: 06)
- `:day`: 콘텐츠가 게시된 일 (예: 13)
- `:title`: 콘텐츠의 제목 (예: Hello World)
- `:slug`: 콘텐츠의 슬러그 (예: hello-world)
- `:section`: 콘텐츠가 속한 섹션 (예: blog)
- `:filename`: 콘텐츠 파일의 이름 (확장자 제외)
