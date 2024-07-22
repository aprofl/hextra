## Cloudflare Pages 설정

1. **Cloudflare 계정에 로그인**:    
    - Cloudflare 계정에 로그인합니다. 계정이 없으면 여기서 계정을 만드세요.
2. **Cloudflare Pages로 이동**:    
    - Cloudflare 대시보드에 로그인한 후, 왼쪽 사이드바에서 "Workers & Pages"를 클릭합니다.
    - Pages 탭을 선택합니다.
3. **Git 저장소 연결**:    
    - "Connect to Git" 버튼을 클릭하여 GitHub 또는 GitLab 계정에 연결합니다. 
    - 이 과정에서 Cloudflare가 저장소에 접근할 수 있도록 권한을 부여해야 합니다.

![cloudflare_gitconnect.png](/Resources/cloudflare_gitconnect.png)

1. **저장소 선택**:    
    - 연결된 Git 계정에서 배포할 저장소를 선택합니다.
    - 해당 저장소를 선택하면 Cloudflare Pages가 자동으로 프로젝트를 설정합니다.
### 배포 설정

![cloudflare_hugo_settting](/Resources/cloudflare_hugo_settting.png)

1. **브랜치 선택**:
    
    - 배포할 브랜치를 선택합니다. 기본적으로 `main` 또는 `master` 브랜치를 선택할 수 있습니다.
2. **빌드 설정**:
    
    - 빌드 설정을 구성합니다. 이는 프로젝트의 빌드 명령어와 배포 디렉토리(예: `npm run build` 및 `public` 디렉토리)를 포함합니다.
    - 일반적인 설정 예시는 다음과 같습습니다.
        - **Build command**: `npm run build` 또는 프로젝트의 빌드 명령어를 입력합니다.
        - **Build output directory**: `public` 또는 빌드된 파일이 위치할 디렉토리를 입력합니다.
3. **환경 변수 설정 (선택 사항)**:
    
    - 필요한 경우, 환경 변수를 설정합니다. 예를 들어, API 키나 기타 설정 값을 추가할 수 있습니다.
4. **배포 시작**:
    
    - 설정이 완료되면 "Save and Deploy" 버튼을 클릭하여 배포를 시작합니다.

## 도메인 설정

Cloudflare에서 도메인을 설정하여 맞춤 도메인을 사용할 수 있습니다.

1. **Cloudflare 계정에 도메인 추가:** Cloudflare 계정에 로그인하여 도메인을 추가하고, 네임서버를 Cloudflare 네임서버로 변경합니다.
2. **DNS 설정:** Cloudflare 대시보드에서 DNS 설정으로 이동하여 새로운 CNAME 레코드를 추가합니다. 이름은 `www`로, 값은 Cloudflare Pages의 서브도메인 (예: `your-site.pages.dev`)으로 설정합니다.
3. **SSL/TLS 설정:** SSL/TLS 설정에서 "Full" 또는 "Full (strict)"을 선택하여 사이트가 HTTPS로 제공되도록 설정합니다.

## 5. GitHub Actions 설정 (선택 사항)

매번 Hugo 사이트를 변경할 때 자동으로 Cloudflare Pages에 배포되도록 GitHub Actions를 설정할 수 있습니다.

`.github/workflows/deploy.yml` 파일을 생성합니다.

```yaml
name: Deploy Hugo site to Cloudflare Pages

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Install Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: 'latest'

    - name: Build site
      run: hugo

    - name: Deploy to Cloudflare Pages
      uses: cloudflare/pages-action@v1
      with:
        apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
        accountId: ${{ secrets.CLOUDFLARE_ACCOUNT_ID }}
        projectName: your-project-name
        directory: ./public
```

이 설정 파일은 GitHub Actions가 Hugo 사이트를 빌드하고, Cloudflare Pages에 배포합니다. Cloudflare API 토큰, 계정 ID, 프로젝트 이름을 GitHub Secrets에 저장해야 합니다.

## GitHub Secrets 설정

GitHub 리포지토리 설정에서 Secrets를 추가합니다.

- `CLOUDFLARE_API_TOKEN`: Cloudflare에서 생성한 API 토큰
- `CLOUDFLARE_ACCOUNT_ID`: Cloudflare 계정 ID
- `CLOUDFLARE_PROJECT_NAME`: Cloudflare Pages 프로젝트 이름

이 설정을 완료하면 GitHub Actions가 자동으로 Hugo 사이트를 빌드하고 Cloudflare Pages에 배포합니다.

모든 설정이 완료되면, Hugo 사이트가 GitHub과 연동되어 Cloudflare에 성공적으로 게시됩니다.