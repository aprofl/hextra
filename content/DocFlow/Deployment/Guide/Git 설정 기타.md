## Public 폴더 포함

Cloudflare Pages는 소스 코드를 GitHub 저장소에서 직접 가져와 빌드 및 배포를 수행합니다. `public` 폴더는 일반적으로 Hugo와 같은 정적 사이트 생성기에서 생성된 빌드 출력물로, 로컬 빌드 결과를 포함하고 있습니다. 그러나 Cloudflare Pages는 자체적으로 소스를 빌드하기 때문에, 일반적으로 `public` 폴더를 GitHub에 푸시할 필요가 없습니다.

### Public 폴더 제외
#### `.gitignore` 파일 생성 또는 수정

- 터미널(예: Command Prompt, PowerShell) 또는 파일 탐색기에서 로컬 리포지토리 디렉토리로 이동합니다.
- **`.gitignore` 파일 열기 또는 생성:**
	- 텍스트 편집기(예: Notepad)를 사용하여 `.gitignore` 파일을 엽니다. 파일이 없는 경우 새로 생성합니다.
	- 확장자는 없습니다.

```sh
notepad .gitignore
```

- **`public` 폴더 추가:**
	- `.gitignore` 파일에 `public/`을 추가하여 `public` 폴더와 그 안의 모든 파일을 무시하도록 설정합니다.

```txt
public/
```

- 변경 사항을 저장하고 파일을 닫습니다.

#### 이미 추적 중인 `public` 폴더 무시하기
만약 `public` 폴더가 이미 Git에 의해 추적되고 있는 경우, `.gitignore` 파일을 수정하는 것만으로는 충분하지 않습니다. 기존의 추적 정보를 제거해야 합니다.

```sh
# `public` 폴더 캐시에서 제거:
git rm -r --cached public/

# 변경 사항 커밋
git add .gitignore
git commit -m "Update .gitignore to exclude public folder"

# 원격 리포지토리에 푸시
git push origin master
```