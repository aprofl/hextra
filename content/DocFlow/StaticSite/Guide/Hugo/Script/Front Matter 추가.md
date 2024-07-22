## 스크립트

```python
import os
import datetime
import re
import yaml

# 메타데이터 템플릿
metadata_template = {
    "title": "{title}",
    "description": "",
    "summary": "",
    "date": "{date}",
    "lastmod": "{lastmod}",
    "draft": False,
    "weight": 10,
    "categories": [],
    "tags": [],
    "contributors": [],
    "toc": True,
    "sidebar": {
        "collapsed": True,
    },
    "seo": {
        "title": "",  # custom title (optional)
        "description": "",  # custom description (recommended)
        "canonical": "",  # custom canonical URL (optional)
        "noindex": False  # 일반 md 파일의 경우 False
    }
}

# YAML 로드 및 덤프 도우미 함수
def load_yaml(content):
    return yaml.safe_load(content)

def dump_yaml(metadata):
    return yaml.dump(metadata, sort_keys=False, default_flow_style=False, allow_unicode=True)

# 파일 경로로부터 카테고리와 태그를 추출하는 함수
def extract_categories_and_tags(file_path, base_dir):
    # base_dir을 기준으로 상대 경로를 얻음
    relative_path = os.path.relpath(file_path, base_dir)
    # 상대 경로를 분할하여 폴더 구조를 얻음
    parts = relative_path.split(os.sep)
    
    # 최상위 폴더를 카테고리로 설정
    category = parts[0]
    # 모든 상위 폴더를 태그로 설정
    tags = parts[:-1]  # 파일 이름을 제외한 모든 폴더
    
    return category, tags

# 파일의 생성 시간과 수정 시간을 ISO 형식으로 반환하는 함수
def get_file_times(file_path):
    creation_time = os.path.getctime(file_path)
    modification_time = os.path.getmtime(file_path)
    creation_time_iso = datetime.datetime.utcfromtimestamp(creation_time).isoformat()
    modification_time_iso = datetime.datetime.utcfromtimestamp(modification_time).isoformat()
    return creation_time_iso, modification_time_iso

# 메타데이터를 업데이트하거나 추가하는 함수
def update_metadata(content, title, category, tags, date, lastmod):
    new_metadata = metadata_template.copy()
    new_metadata["title"] = title
    new_metadata["date"] = date
    new_metadata["lastmod"] = lastmod
    new_metadata["categories"] = [category]
    new_metadata["tags"] = tags

    # 기존 메타데이터가 있는지 확인
    metadata_match = re.match(r"---(.*?)---", content, re.DOTALL)
    if metadata_match:
        existing_metadata_str = metadata_match.group(1)
        existing_metadata = load_yaml(existing_metadata_str)
        original_metadata = existing_metadata.copy()  # 변경 전 메타데이터를 저장

        # 필요한 필드가 없으면 추가, 있으면 유지
        for key, value in metadata_template.items():
            if key not in existing_metadata:
                existing_metadata[key] = value
            elif isinstance(value, dict):
                for subkey, subvalue in value.items():
                    if subkey not in existing_metadata[key]:
                        existing_metadata[key][subkey] = subvalue

        # date 필드는 비어 있는 경우에만 업데이트
        if "date" not in existing_metadata or not existing_metadata["date"]:
            existing_metadata["date"] = date

        # lastmod 필드 업데이트
        if existing_metadata != original_metadata:  # 다른 메타데이터가 변경된 경우에만 lastmod 업데이트
            existing_metadata["lastmod"] = lastmod
        else:
            return content, 'unchanged'

        # 새로운 메타데이터 YAML 생성
        updated_metadata_str = dump_yaml(existing_metadata)
        new_metadata_block = f"---\n{updated_metadata_str}---\n"
        return content.replace(metadata_match.group(0), new_metadata_block).replace('\n\n', '\n'), 'updated'
    else:
        # 메타데이터가 없는 경우 새 메타데이터 추가
        new_metadata["date"] = date
        new_metadata_str = dump_yaml(new_metadata)
        new_metadata_block = f"---\n{new_metadata_str}---\n"
        return new_metadata_block + content.lstrip(), 'added'

# 특정 디렉토리 내의 모든 파일에 메타데이터 추가 또는 업데이트
def add_metadata_to_files(directory):
    total_files_count = 0
    updated_count = 0
    ignored_count = 0
    added_count = 0
    
    for root, dirs, files in os.walk(directory):
        for file in files:
            if file.endswith('.md') and file != "_index.md":
                total_files_count += 1
                file_path = os.path.join(root, file)
                try:
                    with open(file_path, 'r', encoding='utf-8') as f:
                        content = f.read()

                    # 파일 이름을 제목으로 사용 (확장자 제외)
                    title = os.path.splitext(file)[0]
                    # 카테고리와 태그 추출
                    category, tags = extract_categories_and_tags(file_path, directory)
                    # 파일의 생성 시간과 수정 시간 추출
                    date, lastmod = get_file_times(file_path)
                    updated_content, status = update_metadata(content, title, category, tags, date, lastmod)

                    if status == 'updated':
                        updated_count += 1
                    elif status == 'added':
                        added_count += 1

                    if status in ('updated', 'added'):
                        try:
                            with open(file_path, 'w', encoding='utf-8') as f:
                                f.write(updated_content)
                            print(f"Updated metadata in {file_path}")
                        except Exception as e:
                            print(f"Error writing to {file_path}: {e}")
                except Exception as e:
                    print(f"Error reading {file_path}: {e}")
                    ignored_count += 1

    return total_files_count, updated_count, ignored_count, added_count

# 디렉토리 경로를 설정하세요 (예: "D:/Obsidian/DocFlow")
directory_path = r"D:\Obsidian\DocFlow"
total_files_count, updated_count, ignored_count, added_count = add_metadata_to_files(directory_path)

# 로그 출력
print(f"Add Metadata")
print(f"Total files processed: {total_files_count}")
print(f"Metadata updated: {updated_count}")
print(f"Metadata ignored: {ignored_count}")
print(f"Metadata added: {added_count}")
print()
```

## 스크립트 설명

- 스크립트는 지정된 디렉토리(`directory_path`) 내의 모든 `.md` 파일을 검색합니다.
- 각 파일을 읽고, 메타데이터가 이미 있는지 확인합니다.
- 메타데이터가 없는 파일의 경우, 파일의 시작 부분에 메타데이터를 추가합니다.
- 폴더에 `_index.md` 파일이 없으면 추가합니다.
- 메타데이터 템플릿에서 `title`은 파일 이름을 사용합니다.`date`와 `lastmod`는 파일의 생성시간과 마지막 수정시간으로 업데이트합니다.
- 최상위 폴더는 Categories 에 추가하고, 경로상 모든 폴더를 Tags 로 추가합니다.
	- 기존에 내용이 있는 경우 유지하되 중복되지 않는 항목은 추가합니다.
- Contributors: 기존에 내용이 있는 경우 그대로 유지합니다.
- 메타데이터 필드: `title`, `date`, `lastmod`, `categories`, `tags` 필드 외의 항목은 기존에 있는 경우 그대로 유지하고, 없는 경우에만 추가합니다.
- 사이드바 접기 설정: 최상위 폴더는 `collapsed: False`, 나머지 폴더는 `collapsed: True`로 설정합니다.
- 빈 줄 제거: 메타데이터 블록과 내용 첫 줄 사이에 빈 줄이 없도록 합니다.