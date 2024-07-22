## 마운트 기능 소개

Hugo의 마운트 기능은 프로젝트의 파일 및 디렉토리 구조를 유연하게 관리할 수 있게 해주는 기능입니다. 이 기능을 통해 외부 디렉토리나 파일을 Hugo 프로젝트의 특정 위치에 연결할 수 있습니다. 마운트 기능을 사용하면 코드 재사용이 쉬워지고, 프로젝트 구조를 보다 모듈화하여 유지보수가 용이해집니다.

### Hugo 마운트 기능 개요

Hugo의 마운트 기능은 `config.toml` 파일에서 설정할 수 있으며, `module.mounts` 섹션을 사용하여 디렉토리 또는 파일을 원하는 위치에 마운트할 수 있습니다. 이를 통해 외부 콘텐츠나 테마 요소를 프로젝트의 특정 경로에 연결하여 사용할 수 있습니다. 옵시디언으로 글을 작성하고 관리하는 경우, 옵시디언의 볼트를 content 폴더에 마운트 시킴으로서 별도의 처리 과정 없이 소스로 사용할 수 있습니다.

## 사용법

### 기본 설정

Hugo 프로젝트의 `config.toml` 파일을 열고, `mounts` 섹션을 추가합니다. 예를 들어, 다음과 같이 외부 콘텐츠 디렉토리를 마운트할 수 있습니다:

```toml
[module]
  [[module.mounts]]
    source = "../DockFlow"
    target = "content/docs"
```

여기서 `source`는 외부 디렉토리의 경로이고, `target`은 Hugo 프로젝트 내에서 연결할 가상 경로입니다.
프로젝트 폴더 구조는 다음과 같습니다:

```sh
obsidian/
├── DocFlow/
│   ├── note1/
│   │   └── note1.md
│   ├── note2/
│   │   └── note2.md
│   ├── note3/
│   │   └── note3.md
│   └── attachments/
└── DocFlow_Hugo/
    ├── config.toml
    ├── content/
    ├── themes/
    │   └── doks/
    └── ...
```

이 예시에서는 DockFlow 폴더 정체를 `DocFlow_Hugo/content/docs` 폴더로 연결합니다.

### 여러 마운트 설정

여러 디렉토리를 마운트할 수 있습니다. 예를 들어, 테마의 일부를 프로젝트에 포함시키고 싶은 경우 다음과 같이 설정할 수 있습니다:

```toml
[module]
  [[module.mounts]]
    source = "my-theme/assets"
    target = "assets"

  [[module.mounts]]
    source = "my-theme/static"
    target = "static"

  [[module.mounts]]
    source = "my-theme/layouts"
    target = "layouts"
```


이 설정은 `my-theme` 디렉토리의 `assets`, `static`, `layouts` 디렉토리를 각각 Hugo 프로젝트의 해당 디렉토리에 마운트합니다.

### 외부 Git 리포지토리 마운트

Hugo 모듈을 사용하여 외부 Git 리포지토리를 마운트할 수도 있습니다. 예를 들어, 다음과 같이 설정합니다:

```toml
[[module.imports]]
  path = "github.com/username/repository"
  mounts = [
    { source = "content", target = "content/external" }
  ]
```

이 설정은 GitHub의 `repository` 리포지토리에서 `content` 디렉토리를 가져와 Hugo 프로젝트의 `content/external` 디렉토리에 마운트합니다.

## 사용 예시

- Doks 테마에서는 `Module` 섹션을 별도의 파일로 분리하여 관리합니다.
- `config/_default/module.toml` 파일을 수정하여 마운트를 추가할 수 있습니다.

```toml file:config.toml
[[mounts]]
  source = "../DocFlow"
  target = "content/docs"
```

- Hugo 서버를 실행해 보면, Pages 등이 마운트한 폴더의 파일 수만큼 증가한 것을 볼 수 있습니다.

```sh
D:\Obsidian\DocFlow_Hugo>hugo server
Watching for changes in D:\Obsidian\{DocFlow,DocFlow_Hugo}
Watching for config changes in D:\Obsidian\DocFlow_Hugo\config\_default, D:\Obsidian\DocFlow_Hugo\config\_default\menus
Start building sites …
hugo v0.127.0-74e0f3bd63c51f3c7a0f07a7c779eec9e922957e+extended windows/amd64 BuildDate=2024-06-05T10:27:59Z VendorInfo=gohugoio


                   | EN
-------------------+------
  Pages            | 338
  Paginator pages  |  25
  Non-page files   |  47
  Static files     |  13
  Processed images |   4
  Aliases          |  64
  Cleaned          |   0

Built in 564 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

