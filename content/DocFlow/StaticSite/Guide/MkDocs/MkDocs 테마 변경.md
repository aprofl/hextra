## 기본테마

- 기본 테마는 `mkdocs`, `readthedocs` 등
- 'mkdocs.yml' 파일 수정

```yaml
site_name: KCT_Manual
theme:
  name: readthedocs
```

## 커스텀 테마

### Meterial 테마 
- 테마 설치

```sh
> pip install mkdocs-material
```

-  'mkdocs.yml' 파일 수정

```yaml
site_name: My Docs
theme:
  name: material
```

- 추가 설정

```yml
site_name: My Docs
theme:
  name: material
  palette:
    - scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
  font:
    text: Roboto
    code: Roboto Mono
  features:
    - navigation.instant
    - navigation.top
```

- **`scheme`**: `default`는 라이트 모드, `slate`는 다크 모드
- **`primary`**: 기본 색상
- **`accent`**: 강조 색상
- **`toggle`**: 테마 전환 버튼 설정
    - **`icon`**: 아이콘 설정. Material Icons 사용
    - **`name`**: 전환 버튼의 이름
- **`features`**: Material 테마의 고급 기능 활성화
    - **`navigation.instant`**: 즉각적인 네비게이션 활성화
    - **`navigation.top`**: 사이드바(nav)를 상단에 고정.
1. FindProcDLL 플러그인 다운로드
2. KillProcDLL 플러그인 다운로드