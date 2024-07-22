대부분의 Theme는 모든 폴더에 `_index.md` 파일을 요구합니다.
metadata_template 내용은 테마에 따라 달라지며, 다음 예시는 doks theme 용입니다.

## 스크립트

```python file:add_index_file.py
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
        "noindex": True  # false (default) or true
    }
}

# YAML 로드 및 덤프 도우미 함수
def load_yaml(content):
    return yaml.safe_load(content)

def dump_yaml(metadata):
    return yaml.dump(metadata, sort_keys=False, default_flow_style=False, allow_unicode=True)

# 파일의 생성 시간과 수정 시간을 ISO 형식으로 반환하는 함수
def get_file_times(file_path):
    creation_time = os.path.getctime(file_path)
    modification_time = os.path.getmtime(file_path)
    creation_time_iso = datetime.datetime.utcfromtimestamp(creation_time).isoformat()
    modification_time_iso = datetime.datetime.utcfromtimestamp(modification_time).isoformat()
    return creation_time_iso, modification_time_iso

# 메타데이터를 업데이트하거나 추가하는 함수
def update_metadata(content, title, date, lastmod):
    new_metadata = metadata_template.copy()
    new_metadata["title"] = title
    new_metadata["date"] = date
    new_metadata["lastmod"] = lastmod

    # 기존 메타데이터가 있는지 확인
    metadata_match = re.match(r"---(.*?)---", content, re.DOTALL)
    if metadata_match:
        existing_metadata_str = metadata_match.group(1)
        existing_metadata = load_yaml(existing_metadata_str)
        original_metadata = existing_metadata.copy()

        # 필요한 필드가 없으면 추가, 있으면 유지
        for key, value in metadata_template.items():
            if key not in existing_metadata:
                existing_metadata[key] = value
            elif isinstance(value, dict):
                for subkey, subvalue in value.items():
                    if subkey not in existing_metadata[key]:
                        existing_metadata[key][subkey] = subvalue

        # date 필드는 원래 값 유지
        if not existing_metadata.get("date"):
            existing_metadata["date"] = date

        # lastmod 필드 업데이트 (다른 사항이 수정된 경우에만)
        if existing_metadata != original_metadata:
            existing_metadata["lastmod"] = lastmod

        # 새로운 메타데이터 YAML 생성
        updated_metadata_str = dump_yaml(existing_metadata)
        new_metadata_block = f"---\n{updated_metadata_str}\n---\n"
        
        if updated_metadata_str.strip() == existing_metadata_str.strip():
            return content, 'unchanged'
        else:
            return content.replace(metadata_match.group(0), new_metadata_block), 'updated'
    else:
        # 메타데이터가 없는 경우 새 메타데이터 추가
        new_metadata_str = dump_yaml(new_metadata)
        new_metadata_block = f"---\n{new_metadata_str}\n---\n"
        return new_metadata_block + content.lstrip(), 'added'

# 특정 디렉토리 내의 모든 폴더에 _index.md 파일 생성
def create_index_files(directory):
    total_index_files_count = 0
    index_added_count = 0
    index_updated_count = 0

    for root, dirs, files in os.walk(directory):
        for dir in dirs:
            folder_path = os.path.join(root, dir)
            index_file_path = os.path.join(folder_path, "_index.md")

            # .md 파일이나 서브 폴더가 있는지 확인
            has_md_files = any(file.endswith('.md') for file in os.listdir(folder_path) if os.path.isfile(os.path.join(folder_path, file)))
            has_sub_folders = any(os.path.isdir(os.path.join(folder_path, sub)) for sub in os.listdir(folder_path))

            if not has_md_files and not has_sub_folders:
                continue

            total_index_files_count += 1

            # _index.md 파일이 이미 있는지 확인
            if os.path.exists(index_file_path):
                # 이미 존재하는 경우 업데이트
                try:
                    with open(index_file_path, 'r', encoding='utf-8') as f:
                        content = f.read()

                    title = os.path.basename(folder_path)
                    _, lastmod = get_file_times(index_file_path)
                    
                    updated_content, status = update_metadata(content, title, None, lastmod)
                    if status == 'updated':
                        with open(index_file_path, 'w', encoding='utf-8') as f:
                            f.write(updated_content)
                        index_updated_count += 1
                        print(f"Updated _index.md in {folder_path}")
                except Exception as e:
                    print(f"Error updating {index_file_path}: {e}")
            else:
                # 없는 경우 새로 생성
                try:
                    title = os.path.basename(folder_path)
                    current_time = datetime.datetime.now(datetime.timezone.utc).isoformat()

                    new_metadata = metadata_template.copy()
                    new_metadata["title"] = title
                    new_metadata["date"] = current_time
                    new_metadata["lastmod"] = current_time
                    new_metadata_str = dump_yaml(new_metadata)
                    new_metadata_block = f"---\n{new_metadata_str}\n---\n"
                    
                    with open(index_file_path, 'w', encoding='utf-8') as f:
                        f.write(new_metadata_block)
                    index_added_count += 1
                    print(f"Created _index.md in {folder_path}")
                except Exception as e:
                    print(f"Error creating {index_file_path}: {e}")

    return total_index_files_count, index_added_count, index_updated_count

def is_markdown_file(file):
    return file.endswith('.md')

# 디렉토리 경로를 설정하세요 (예: "D:/Obsidian/DocFlow")
directory_path = r"D:\Obsidian\DocFlow"
total_index_files_count, index_added_count, index_updated_count = create_index_files(directory_path)

# 로그 출력
print(f"Add file : _index.md")
print(f"Total index files processed: {total_index_files_count}")
print(f"_index.md added: {index_added_count}")
print(f"_index.md updated: {index_updated_count}")
print()
```

## 스크립트 설명

- 스크립트는 지정된 디렉토리(`directory_path`) 내의 모든 폴더를 검색합니다.
- 각 폴더에서 `_index.md` 파일과 서브디렉토리가 없고 파일이 한 개만 있는 경우, 메타데이터가 포함된 `_index.md` 파일을 생성합니다.
- 메타데이터 템플릿에서 `title`은 폴더 이름을 사용하며, `date`와 `lastmod`는 현재 날짜와 시간을 사용합니다.
