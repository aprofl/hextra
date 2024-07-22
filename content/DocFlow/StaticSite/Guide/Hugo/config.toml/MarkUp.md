`[markup]` 섹션은 마크다운과 코드 하이라이팅 설정을 정의합니다. 
이 섹션은 사이트의 콘텐츠를 어떻게 렌더링하고 스타일링할지를 설정하는 중요한 부분입니다.

## `[markup]` 섹션

#### `[markup.goldmark]`
- Goldmark 마크다운 렌더러 설정
- Goldmark는 Hugo에서 사용되는 마크다운 렌더러로, 빠르고 확장성이 뛰어나며 CommonMark 표준을 준수합니다.
#### `[markup.goldmark.parser.attribute]`
- Goldmark 파서 설정
- `block = true`
	- 블록 수준의 속성 파서를 활성화합니다.
	- 예를 들어, 특정 블록 요소에 ID나 클래스를 지정하여 스타일을 적용할 수 있습니다.
	- 다음과 같이, 마크다운 제목에 ID와 클래스를 추가할 수 있습니다.

```md
# 제목 {#custom-id .custom-class}
```

#### `[markup.goldmark.renderer]
- Goldmark 렌더러 설정
- `unsafe = true`
	- 안전하지 않은 HTML 렌더링을 허용합니다.
	- 마크다운 파일 내의 HTML 태그를 렌더링할 수 있도록 허용합니다. 
		- 보안상의 이유로 기본적으로 Hugo는 마크다운 내의 HTML 태그를 렌더링하지 않습니다. 
		- 이 설정을 활성화하면 사용자 정의 HTML을 마크다운 콘텐츠 내에서 사용할 수 있습니다.    
	- 다음과 같이, 마크다운 파일 내에 HTML 태그를 포함할 수 있습니다.

```md
<div class="custom-div">
  <p>이것은 사용자 정의 HTML 블록입니다.</p>
</div>
```

#### `[markup.highlight]`
- 코드 하이라이팅 설정
- 코드 하이라이팅은 프로그래밍 코드 블록을 시각적으로 강조하는 기능으로, 코드의 가독성을 높이고 이해하기 쉽게 만듭니다.
- `style = "tango"`
	- 하이라이팅 스타일을 `tango`로 설정합니다.
	- `tango`는 Pygments 라이브러리에서 제공하는 여러 스타일 중 하나입니다.
	- 이 스타일은 다양한 색상을 사용하여 코드의 각 부분을 시각적으로 구분합니다.
    
    #### 다른 스타일 예시    
    - `monokai`: 어두운 배경에 밝은 색상을 사용하는 인기 있는 스타일
    - `github`: GitHub에서 사용하는 코드 하이라이팅 스타일
    - `vs`: Visual Studio에서 사용하는 스타일
    
    각 스타일은 코드의 가독성을 높이고 다양한 환경에서 보기 좋게 만들기 위해 설계되었습니다.

## 예시 마크다운 파일

다음은 설정된 `[markup]` 옵션을 활용하는 예시 마크다운 파일입니다:

```md
# 마크다운 제목 {#custom-id .custom-class}
이것은 블록 수준의 속성을 가진 제목입니다.
```html

<div class="custom-div">
  <p>이것은 사용자 정의 HTML 블록입니다.</p>
</div>

```
위 HTML 블록은 `unsafe` 설정이 활성화되어 있기 때문에 렌더링됩니다.
