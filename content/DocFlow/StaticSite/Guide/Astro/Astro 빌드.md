Obsidian 볼트 `obsidian/blog`를 Astro로 빌드하여 정적 사이트를 만드는 과정을 단계별로 설명하겠습니다. 폴더 구조, 설정 파일 및 명령어를 포함한 전체적인 과정을 다룹니다.
### 1. 환경 설정 및 프로젝트 준비
#### 1.1. 프로젝트 폴더 구조
다음과 같은 구조로 프로젝트를 구성합니다:

```sh
obsidian/
├── astro-project/
│   ├── public/
│   │   ├── attachments/  # 이미지 파일 복사 위치
│   │   ├── copyCode.js
│   │   └── styles.css
│   ├── src/
│   │   ├── layouts/
│   │   │   └── DefaultLayout.astro
│   │   └── pages/
│   │       ├── note1/
│   │       │   └── note1.md
│   │       ├── note2/
│   │       │   └── note2.md
│   │       └── note3/
│   │           └── note3.md
│   ├── astro.config.mjs
│   └── package.json
└── blog/
    ├── note1/
    │   └── note1.md
    ├── note2/
    │   └── note2.md
    ├── note3/
    │   └── note3.md
    └── attachments/  # 이미지 및 기타 첨부 파일 폴더
```

### 2. Astro 프로젝트 생성
#### 2.1. Astro 프로젝트 생성

```sh
mkdir astro
cd astro
npm create astro@latest
npm install
```

### 3. Obsidian 콘텐츠 가져오기
#### 3.1. Obsidian 콘텐츠 복사

`obsidian/blog/notes` 폴더의 Markdown 파일을 `astro-project/src/pages` 폴더로 복사합니다.
#### 3.2. 이미지 파일 복사

`obsidian/blog/attachments` 폴더의 이미지를 `astro-project/public/images` 폴더로 복사합니다.
### 4. Astro 설정 파일 구성
#### 4.1. `astro.config.mjs` 파일

```js file:astro.config.mjs
import { defineConfig } from 'astro/config';
export default defineConfig({
  site: 'https://yourdomain.com',  // 사이트 도메인
  markdown: {
    remarkPlugins: [],
    rehypePlugins: [
      [
        'rehype-rewrite',
        {
          rewrite: (node) => {
            if (node.tagName === 'a' && node.properties.href) {
              node.properties.href = node.properties.href.replace('.md', '.html');
            }
          },
        },
      ],
    ],
  },
});
```

### 5. 빌드 및 배포
#### 5.1. 빌드 명령어 실행
Astro 프로젝트를 빌드합니다.

```sh
npm run build
```

### 6. 폴더 구조 요약
최종 프로젝트 폴더 구조는 다음과 같습니다:

```sh
obsidian/
├── astro-project/
│   ├── public/
│   │   ├── attachments -> ../blog/attachments  # 심볼릭 링크
│   │   ├── copyCode.js
│   │   └── styles.css
│   ├── src/
│   │   ├── layouts/
│   │   │   └── DefaultLayout.astro
│   │   └── pages/
│   │       ├── note1 -> ../../blog/note1  # 심볼릭 링크
│   │       ├── note2 -> ../../blog/note2  # 심볼릭 링크
│   │       ├── note3 -> ../../blog/note3  # 심볼릭 링크
└── blog/
    ├── note1/
    │   └── note1.md
    ├── note2/
    │   └── note2.md
    ├── note3/
    │   └── note3.md
    └── attachments/  # 이미지 및 기타 첨부 파일 폴더
```

## 폴더 복사 생략

Obsidian 폴더를 그대로 사용하여 Astro로 빌드하고, 파일 복사 과정을 생략하는 방법을 설명드리겠습니다. 이를 위해 심볼릭 링크를 사용하거나, Astro의 설정을 통해 Obsidian 폴더를 직접 참조하도록 구성할 수 있습니다.
### 1. 심볼릭 링크 사용
심볼릭 링크를 사용하면 Obsidian 폴더를 Astro 프로젝트에서 바로 참조할 수 있습니다.
#### 1.1. 프로젝트 폴더 구조

```sh
obsidian/
├── work/
└── blog/
    ├── notes/        # Obsidian 노트 폴더
    └── attachments/  # 이미지 및 기타 첨부 파일 폴더
astro-project/
├── public/
│   └── images/      # 이미지 파일에 대한 심볼릭 링크
├── src/
│   └── pages/       # 노트 폴더에 대한 심볼릭 링크
└── astro.config.mjs
└── package.json
```

1.2. 심볼릭 링크 생성

```sh
cd astro-project
ln -s ../../obsidian/blog/notes src/pages
ln -s ../../obsidian/blog/attachments public/images
```

### 2. Astro 프로젝트 설정
#### 2.1. Astro 프로젝트 생성

```sh
mkdir astro-project
cd astro-project
npm create astro@latest
npm install
```

### 3. 빌드 및 배포
#### 3.1. 빌드 명령어 실행
Astro 프로젝트를 빌드합니다.

```sh
npm run build
```

#### 3.2. Cloudflare Pages 설정
1. **Cloudflare 계정 생성 및 로그인**: [Cloudflare](https://www.cloudflare.com/)에 가입하고 로그인합니다.
2. **새 프로젝트 추가**:
    - Cloudflare Pages 대시보드에서 "Create a project" 버튼을 클릭합니다.
    - GitHub 또는 GitLab 리포지토리를 연결합니다.
3. **배포 설정**:
    - 빌드 명령어: `npm run build`
    - 빌드 출력 디렉토리: `dist`
4. **배포 시작**: 프로젝트 설정이 완료되면 자동으로 빌드가 시작되고 배포됩니다.
### 최종 프로젝트 폴더 구조 요약

```sh
astro-project/
├── public/
│   └── images -> ../../obsidian/blog/attachments  # 심볼릭 링크
├── src/
│   └── pages -> ../../obsidian/blog/notes  # 심볼릭 링크
├── astro.config.mjs
└── package.json
```