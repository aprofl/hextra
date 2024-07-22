## 기본 전략

### [Obsidian](옵시디언이란.md)으로 글 작성 및 관리

- 마크다운 기반의 노트 작성 및 관리 애플리케이션을 사용하여 글을 작성하고 관리합니다.
- 이를 통해 모든 글을 체계적으로 관리하고 연결할 수 있습니다.
### [동기화](Obsidian/Guide/Setting/Sync/동기화%20방식)
- [NextCloud를 이용한 동기화](obsidian/guide/setting/sync/NextCloud를%20이용한%20동기화%20방법)
	- 작성한 글을 여러 디바이스에서 접근하고 편집할 수 있도록 NextCloud를 사용하여 동기화합니다.
###  [정적사이트](staticsite/about/StaticSite란)로 빌드
- Hugo: 고성능 정적 사이트 생성기를 사용하여 Obsidian에서 작성한 글을 정적 사이트로 빌드합니다.
### Git에 푸시
- 작성된 정적 사이트 파일을 Git 리포지토리에 푸시하여 버전 관리를 수행합니다.
    - Git을 사용하면 협업과 버전 관리를 쉽게 할 수 있습니다.`git add .`, `git commit -m "message"`, `git push origin main` 명령어를 사용하여 변경 사항을 푸시합니다.
### CloudFlare로 배포
- CloudFlare Pages: CloudFlare의 정적 사이트 호스팅 서비스를 사용하여 빌드된 사이트를 배포합니다.
    - Git 리포지토리와 연동하여 자동으로 사이트를 빌드하고 배포합니다.
    - CloudFlare 대시보드에서 Pages 설정을 통해 Git 리포지토리를 연결하고 빌드 설정을 완료합니다.