다크모드를 기본으로 바꾸는 법

```yml
theme:
  name: material
  palette:
    - scheme: slate # 다크 모드를 기본으로 설정
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
    - scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
  font:
    text: Roboto
    code: Roboto Mono  
  extra_css:
    - css/custom.css
```
