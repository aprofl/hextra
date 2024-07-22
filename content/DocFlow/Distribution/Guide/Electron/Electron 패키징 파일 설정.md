## 이름 변경

```json
{
  "name": "output",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "build": "electron-builder"
  },
  "description": "",
  "devDependencies": {
    "electron": "^30.0.7",
    "electron-builder": "^24.13.3"
  },
  "build": {
    "appId": "com.example.mkdocs",
    "files": [
      "main.js",
      "site/**/*"
    ],
    "directories": {
      "output": "dist"
    },
    "compression": "maximum",
    "extraResources": "site/**/*",    
    "extraMetadata": {
      "main": "main.js"
    }
  }
}

```

- 출력 파일 이름 : **output Setup 1.0.0.exe**
### productName 설정

```json
  "build": {
    "appId": "com.example.mkdocs",
    "productName": "KCT Manual",
  }

```


- 출력 파일 이름 : **KCT Manual Setup 1.0.0.exe**
### artifactName 설정

```json

  "build": {
    "appId": "com.example.mkdocs",
    "productName": "KCT Manual",
    "artifactName": "${productName}-${version}.${ext}",    
  }

```


- 출력 파일 이름 : **KCT Manual_1.0.0.exe**

## 설치 화면 비활성

![electron_install_screen](/Resources/electron_install_screen.png)

`package.json` 파일에 `nsis` 설정을 추가하여 `oneClick` 설치 활성화
- NSIS 설치
	- https://nsis.sourceforge.io/Download

```json
  "build": {
    "appId": "com.example.mkdocs",
    "files": [
      "main.js",
      "site/**/*"
    ],
    "directories": {
      "output": "dist"
    },
    "compression": "maximum",
    "extraResources": "site/**/*",    
    "extraMetadata": {
      "main": "main.js"
    },
    "nsis": {
      "oneClick": true,
      "perMachine": true,
      "allowToChangeInstallationDirectory": false,
      "deleteAppDataOnUninstall": true,      
    }
  }

```

- nsis 설정만으로는 설치 진행 창이 사라지지 않음

```json
"nsis": {
      "oneClick": true,
      "perMachine": true,
      "allowToChangeInstallationDirectory": false,
      "deleteAppDataOnUninstall": true,
      "include": "installer.nsh"
    }
```


```installer.nsh

!macro customHeader
  SilentInstall silent
  SilentUnInstall silent
!macroend
!macro customInit
  SetSilent silent
!macroend
Section "MainSection"
  ; 실제 설치 작업을 여기에 추가
SectionEnd

```


- install.nsh 파일을 만들어서 include 시키면 빌드 버전에서 실행이 안됨
- npm run 으로는 실행됨
- 원인 분석 필요

## site 폴더 참조

- 폴더 구조
	- KCT/Site
	- KCT/Output
- Output 폴더의 Pakage.json 파일에서 Site 를 참조해야 하므로 경로를 "../Site" 로 설정하면 될거 같은데 안됨.
- 상대 경로 설정이 제대로 안되는 듯
- 빌드할때마다 Site 폴더를 복사하는 건 귀찮으니까 prebuild script 추가
- 프로젝트 루트 디렉토리에 `copy-files.js` 파일을 생성하고 다음 내용 추가

```js
const fs = require('fs-extra');
const path = require('path');
// 상위 폴더의 site 디렉토리 경로
const sourceDir = path.join(__dirname, '..', 'site');
// 현재 프로젝트의 build 디렉토리 경로
const destDir = path.join(__dirname, 'site');
// 디렉토리 복사
fs.copy(sourceDir, destDir, err => {
  if (err) {
    console.error('Error while copying the site directory:', err);
  } else {
    console.log('Site directory copied successfully!');
  }
});
```

`package.json` 수정

```json
{
  "name": "output",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "prebuild": "node copy-files.js",
    "start": "electron .",
    "build": "electron-builder"
  },
  "devDependencies": {
    "electron": "^30.0.7",
    "electron-builder": "^24.13.3",
    "fs-extra": "^10.0.0"
  },
  "build": {
    "appId": "com.example.mkdocs",
    "productName": "KCT_Manual",
    "artifactName": "${productName}_${version}.${ext}",
    "files": [
      "main.js",
      "site/**/*"
    ],
    "directories": {
      "output": "dist"
    },
    "compression": "maximum",
    "extraResources": [
      {
        "from": "site",
        "to": "site"
      }
    ],    
    "extraMetadata": {
      "main": "main.js"
    },
    "nsis": {
      "oneClick": true,
      "allowElevation": true,
      "allowToChangeInstallationDirectory": false,
      "deleteAppDataOnUninstall": true,      
      "removeDefaultUninstallWelcomePage": true
    }
  }
}

```

- 빌드 시 site 폴더 복사 여부 확인
