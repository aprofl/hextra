첫 배포시 baseURL 문제로 page 참조가 안될 수 있습니다.
 `config/_default/hugo.toml` 파일의 다음 설정을 확인합니다.
 
```toml
baseURL = "http://localhost"
```

위 항목을 CloudFlare page 주소로 변경합니다.

## 동적 설정

`baseURL` 설정이 로컬과 배포 환경에서 다르면, 각각의 환경에서 URL을 다르게 설정해야 하는 문제가 발생합니다. 이를 해결하기 위해 Hugo는 여러 환경에 맞춰 `baseURL`을 동적으로 설정할 수 있는 방법을 제공합니다.

###  1. 로컬 서버 실행 시 baseURL 설정

`config/_default/hugo.toml` 파일의 다음 설정을 확인합니다.
 
```toml
baseURL = "https://aprofl.pages.dev/"
```

#### 로컬 빌드

- 로컬빌드가 되지 않는 경우, 다음과 같이 baseURL 을 지정하여 실행합니다.

```sh
hugo server --baseURL="localhost:1313" --ignoreCache
```

#### 배포 빌드
배포빌드 시에는 baseURL의 설정값을 따라갑니다.

### 방법 2. 환경 변수 사용

Hugo는 환경 변수를 사용하여 `baseURL`을 설정할 수 있습니다. 이렇게 하면 로컬 환경과 배포 환경에서 다른 `baseURL`을 사용할 수 있습니다.

#### `config.toml` 파일 수정

`config.toml` 파일을 다음과 같이 수정합니다:

```toml
baseURL = "{{ .Env.BASE_URL | default 'http://localhost:1313/' }}"
```

이제 로컬에서 Hugo를 실행할 때는 기본값이 `http://localhost:1313/`가 됩니다.

#### 로컬 빌드

로컬에서 Hugo를 빌드하거나 서버를 실행할 때는 환경 변수를 설정하지 않고 다음과 같이 실행합니다

```sh
hugo server
```

#### 배포 빌드

Cloudflare Pages에서 빌드할 때는 환경 변수를 설정합니다. Cloudflare Pages 대시보드에서 환경 변수를 설정할 수 있습니다:

- **Variable name**: `BASE_URL`
- **Value**: `https://aprofl.pages.dev/`

이렇게 하면 Cloudflare Pages에서 빌드할 때 `baseURL`이 `https://aprofl.pages.dev/`로 설정됩니다.

![cloud_baseurl](/Resources/cloud_baseurl.png)

### 방법 3: Hugo의 구성 파일 분리

Hugo는 여러 구성 파일을 지원하므로, 환경별로 다른 구성 파일을 사용할 수 있습니다. 예를 들어, `config.toml` 외에 `config.production.toml` 파일을 추가할 수 있습니다.

#### `config.production

```toml
baseURL = "https://aprofl.pages.dev/"
```

#### 로컬 빌드

로컬에서 Hugo를 빌드하거나 서버를 실행할 때는 기본 구성 파일을 사용합니다:

```sh
hugo server
```

#### 배포 빌드

Cloudflare Pages에서 배포할 때는 `HUGO_ENV` 환경 변수를 설정합니다:

- **Variable name**: `HUGO_ENV`
- **Value**: `production`

Cloudflare Pages는 `production` 환경에서 자동으로 `config.production.toml` 파일을 사용하게 됩니다.

### 방법 4: Hugo 플래그 사용

Hugo를 실행할 때 `--baseURL` 플래그를 사용하여 동적으로 `baseURL`을 설정할 수도 있습니다.

#### 로컬 빌드

로컬에서 Hugo 서버를 실행할 때 기본값을 사용합니다:

```sh
hugo server
```

#### 배포 빌드

Cloudflare Pages에서 빌드 명령어를 다음과 같이 설정합니다:

```sh
hugo --baseURL https://aprofl.pages.dev/
```

이 방법은 Cloudflare Pages 대시보드에서 **Build command**를 수정하여 설정할 수 있습니다.