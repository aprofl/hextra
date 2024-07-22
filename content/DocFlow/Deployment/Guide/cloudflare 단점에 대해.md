## 요약

단순 블로그 배포 시에는 서버리스 기능이 많이 필요하지 않습니다. Cloudflare Pages는 정적 사이트를 배포하는 데 매우 적합하며, 필요한 경우 Cloudflare Workers를 사용하여 서버리스 기능을 확장할 수 있습니다. 로그인 기능이 없고, 댓글 시스템을 고려하고 있다면 외부 서비스나 간단한 Cloudflare Workers 기능으로 충분히 구현할 수 있습니다.
따라서, Cloudflare Pages를 통해 블로그를 배포하는 것은 매우 효율적이고, 추가 서버리스 기능이 필요할 때는 Cloudflare Workers를 통해 보완할 수 있습니다.

## Cloudflare와 서버리스 기능

Cloudflare는 기본적으로 정적 사이트 배포에 최적화되어 있습니다. 서버리스 기능이 부족하다는 점이 언급되었지만, Cloudflare Workers를 통해 서버리스 기능을 추가할 수 있습니다.
### Cloudflare Workers
#### 기능
- Cloudflare Workers는 Cloudflare의 서버리스 컴퓨팅 플랫폼으로, JavaScript, Rust, C, C++, Python 등의 언어를 사용하여 서버리스 코드를 작성할 수 있습니다.
#### 사용
- 간단한 API를 만들거나, 댓글 기능, 데이터 처리 등 다양한 서버리스 작업을 처리할 수 있습니다.
#### 통합
- Cloudflare Pages와 쉽게 통합되어 추가 서버리스 기능을 제공할 수 있습니다.

## 서버리스 기능 사용 예시

### 댓글 시스템
댓글 시스템을 구현하는 방법에는 여러 가지가 있습니다:
- **외부 서비스 사용**: Disqus, Commento, Staticman 같은 외부 댓글 시스템을 사용하면 별도의 서버리스 기능이 필요 없습니다. 이들은 위젯을 통해 쉽게 블로그에 통합할 수 있습니다.
- **Custom Serverless Function**: Cloudflare Workers를 사용하여 자체 댓글 시스템을 구현할 수도 있습니다. 이 경우, Workers KV 등을 이용하여 데이터를 저장하고 처리할 수 있습니다.
### 폼 처리
독자들이 이메일 구독, 연락처 폼 등을 제출할 수 있게 하려면 서버리스 함수가 필요합니다.
- **폼 서비스**: Formspree, Netlify Forms 등의 외부 서비스를 사용하여 폼 데이터를 처리할 수 있습니다.
- **Cloudflare Workers**: 폼 데이터를 받아 처리하는 서버리스 함수를 작성하여 Cloudflare Workers에 배포할 수 있습니다.
