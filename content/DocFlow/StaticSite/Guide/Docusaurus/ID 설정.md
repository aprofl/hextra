### 옵시디언의 링크 방식

옵시디언은 마크다운 기반의 메모 및 문서 관리 애플리케이션으로, 내부 링크를 생성하는 데 사용되는 방식은 매우 직관적입니다. 옵시디언에서는 문서 제목을 그대로 링크로 사용합니다.

예시:

```md
[[문서 제목]]
```

옵시디언에서 문서 제목이 "테스트 진행"인 경우, 내부 링크는 다음과 같이 작성됩니다:

```md
[[테스트 진행]]
```

### 독사우루스의 링크 방식

독사우루스는 정적 사이트 생성기로, 링크를 생성할 때 `id`를 사용하여 문서를 참조합니다. 이는 URL 경로로 사용되므로 URL 인코딩 문제를 방지하기 위해 영어와 숫자, 하이픈(-)만 사용하는 것이 좋습니다.

예시:

```md
[링크 텍스트](/docs/문서-id)
```

독사우루스에서 "테스트 진행" 문서를 참조하려면, 먼저 해당 문서에 `id`를 설정해야 합니다. 예를 들어, `id`를 `test-progress`로 설정한 경우, 링크는 다음과 같이 작성됩니다:

```md
[테스트 진행](/docs/test-progress)
```

## 프론트 매터 없이 제목을 아이디로 사용하는 경우

프론트 매터를 사용하지 않고 제목을 아이디로 사용할 경우, 한글과 띄어쓰기, 특수문자 등이 URL 인코딩 문제를 일으킬 수 있습니다. 이는 브라우저와 서버 간의 통신에서 문제가 될 수 있습니다.

예시:

옵시디언에서 "테스트 진행" 문서를 참조하는 링크:

```md
[테스트 진행](테스트%20진행)
```

이는 독사우루스에서도 기본적으로 작동할 수 있지만, URL 인코딩 문제를 방지하기 위해 아이디를 변경하는 것이 좋습니다.

## 스크립트를 사용하여 URL 인코딩 문제 해결

### 문제점

1. **한글과 띄어쓰기 문제**: 제목에 한글과 띄어쓰기가 포함된 경우 URL 인코딩 문제가 발생할 수 있습니다.
2. **링크 정확성**: 옵시디언의 내부 링크를 독사우루스에서 참조하도록 변환해야 합니다.

### 해결 방법

1. **아이디 생성**: 문서 제목을 로마자로 변환하고, 띄어쓰기를 하이픈(-)으로 대체하여 URL 안전한 아이디를 생성합니다.
2. **프론트 매터 추가**: 각 문서에 프론트 매터를 추가하여 `id`와 `title`을 설정합니다.
3. **내부 링크 변환**: 옵시디언의 내부 링크를 독사우루스 형식으로 변환합니다.

#### `unidecode` 설치
`unidecode` 모듈은 텍스트를 로마자로 변환하는데 사용되며, 이를 설치하기 위해 다음 명령어를 사용해야 합니다.

```sh
pip install unidecode
```

### 스크립트 예시

```python
import os
import re
from unidecode import unidecode

def generate_id(title):
    id = unidecode(title)  # 한글을 로마자로 변환
    id = re.sub(r'\s+', '-', id)  # 띄어쓰기를 하이픈으로 대체
    id = re.sub(r'[^\w\-]', '', id)  # 특수문자 제거
    id = id.strip('-').lower()  # 양쪽 끝의 하이픈 제거 및 소문자 변환
    return id

def extract_keywords_and_tags(file_path):
    # 파일 경로에서 폴더 이름 추출
    path_parts = os.path.normpath(file_path).split(os.sep)
    folder_names = path_parts[:-1]  # 마지막 부분은 파일 이름이므로 제외
    return folder_names

def add_or_update_front_matter(file_path):
    with open(file_path, 'r', encoding='utf-8') as f:
        lines = f.readlines()

    # 파일이 비어 있는지 확인
    if not lines:
        print(f"File is empty: {file_path}")
        return None, None

    # 코드 블록 안에 있는지 확인하는 함수
    def is_within_code_block(index):
        in_code_block = False
        for i in range(index):
            if lines[i].strip().startswith("```"):
                in_code_block = not in_code_block
        return in_code_block

    # 기존 프론트 매터가 있는지 확인
    if lines[0].strip() == '---' and not is_within_code_block(0):
        front_matter_end = 1
        while front_matter_end < len(lines) and lines[front_matter_end].strip() != '---':
            front_matter_end += 1
        front_matter_end += 1

        front_matter = lines[:front_matter_end]
        content = lines[front_matter_end:]
    else:
        front_matter = []
        content = lines

    # 프론트 매터 정보 추출 및 기본 값 설정
    title = content[0].strip('# ').strip()
    id = generate_id(title)
    keywords_and_tags = extract_keywords_and_tags(file_path)
    keywords = ', '.join(keywords_and_tags)
    tags = ', '.join(keywords_and_tags)

    front_matter_dict = {
        'id': id,
        'title': title,
        'description': '',
        'keywords': keywords,
        'tags': tags,
        'sidebar_position': '1'
    }

    # 기존 프론트 매터가 있으면 비어있는 항목만 채우기
    for line in front_matter:
        match = re.match(r'(\w+):\s*(.*)', line.strip())
        if match:
            key, value = match.groups()
            if value:
                front_matter_dict[key] = value

    # 새로운 프론트 매터 생성
    new_front_matter = ['---\n']
    for key, value in front_matter_dict.items():
        new_front_matter.append(f'{key}: {value}\n')
    new_front_matter.append('---\n')

    # 파일에 새 프론트 매터와 기존 내용 결합하여 쓰기
    with open(file_path, 'w', encoding='utf-8') as f:
        f.writelines(new_front_matter + content)

    return title, id

def convert_internal_links(file_path, title_id_map):
    with open(file_path, 'r', encoding='utf-8') as f:
        content = f.read()

    # 코드 블록 안에 있는 링크를 무시하는 함수
    def is_within_code_block(content, start_index):
        in_code_block = False
        for i in range(start_index):
            if content[i:i+3] == "```":
                in_code_block = not in_code_block
        return in_code_block

    def replace_link(match):
        link_title = match.group(1)
        link_id = title_id_map.get(link_title, generate_id(link_title))
        return f'[{link_title}](/docs/{link_id})'

    # 링크를 변환하면서 코드 블록을 무시
    new_content = []
    index = 0
    while index < len(content):
        if content[index] == '[' and not is_within_code_block(content, index):
            match = re.match(r'\[\[([^\]]+)\]\]', content[index:])
            if match:
                new_content.append(replace_link(match))
                index += len(match.group(0))
                continue
        new_content.append(content[index])
        index += 1
    
    new_content = ''.join(new_content)

    with open(file_path, 'w', encoding='utf-8') as f:
        f.write(new_content)

def process_markdown_files(directory):
    title_id_map = {}
    for root, _, files in os.walk(directory):
        for filename in files:
            if filename.endswith('.md'):
                file_path = os.path.join(root, filename)
                title, id = add_or_update_front_matter(file_path)
                if title and id:
                    title_id_map[title] = id

    for root, _, files in os.walk(directory):
        for filename in files:
            if filename.endswith('.md'):
                file_path = os.path.join(root, filename)
                convert_internal_links(file_path, title_id_map)

# 예시 디렉토리 경로
markdown_directory = r"D:\Obsidian\aprofl_docu\docs"
process_markdown_files(markdown_directory)

```

### 설명

1. **generate_id(title)**: 제목을 로마자로 변환하고, 띄어쓰기를 하이픈으로 대체하여 `id`를 생성합니다.
2. **extract_keywords_and_tags(file_path)**: 파일 경로에서 폴더 이름을 추출하여 `keywords`와 `tags`로 사용합니다.
3. **add_or_update_front_matter(file_path)**: 기존 프론트 매터가 있는지 확인하고, 필요한 경우 항목을 추가하거나 수정합니다. 기본 항목으로 `id`, `title`, `description`, `keywords`, `tags`, `sidebar_position`을 설정합니다.
4. **convert_internal_links(file_path, title_id_map)**: 옵시디언의 내부 링크를 독사우루스 형식으로 변환합니다.
5. **process_markdown_files(directory)**: 지정된 디렉토리 내의 모든 마크다운 파일을 처리하여 프론트 매터를 추가하거나 수정하고, 내부 링크를 변환합니다.