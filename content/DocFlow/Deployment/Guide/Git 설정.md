## 로컬 Git 저장소 설정

Hugo 사이트 디렉토리에서 Git 저장소를 초기화하고 첫 번째 커밋을 만듭니다.

```sh
# Git 저장소 초기화
git init

# 모든 파일을 스테이지에 추가
git add .

# 초기 커밋
git commit -m "Initial commit"
```

## GitHub 리포지토리 생성 및 Hugo 사이트 푸시

GitHub에 새 리포지토리를 생성합니다. 그런 다음, 로컬 Hugo 사이트를 해당 리포지토리에 푸시합니다.

```sh
# 원격 저장소 추가
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git

# 원격 저장소로 푸시
git push -u origin master
```

## 인증

```sh
git push -u origin master
warning: could not find UI helper 'GitHub.UI'
Select an authentication method for 'https://github.com/':
  1. Web browser (default)
  2. Device code
  3. Personal access token
option (enter for default):
```

이 경고 메시지는 GitHub에서 인증을 요구하는 경우 발생합니다. 아래 단계에 따라 인증을 완료할 수 있습니다:

1. **Web Browser (기본 옵션)**:    
    - 기본 옵션은 웹 브라우저를 통해 인증하는 것입니다. 
    - 브라우저가 열리면 GitHub 계정에 로그인하고 인증 절차를 따르세요.
    - 인증이 완료되면 GitHub로부터 터미널로 돌아와서 푸시가 완료됩니다.
2. **Device Code**:    
    - GitHub는 인증에 사용할 기기 코드를 제공합니다.
    - 브라우저에서 GitHub 기기 코드 인증 페이지(예: [https://github.com/login/device)로](https://github.com/login/device)%EB%A1%9C) 이동하여 기기 코드를 입력하고 인증 절차를 따르세요.
    - 인증이 완료되면 터미널로 돌아와서 푸시가 완료됩니다.
3. **Personal Access Token**:    
    - GitHub에서 생성한 개인 액세스 토큰(Personal Access Token)을 입력하세요. 
    - 토큰을 생성하려면 GitHub 설정에서 Developer settings > Personal access tokens으로 이동하세요.
    - 새로운 토큰을 생성하고, `repo` 권한을 포함한 필요한 권한을 부여하세요.
    - 생성된 토큰을 복사하여 터미널에 입력하고 Enter 키를 누르세요.