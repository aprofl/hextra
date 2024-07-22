## Electron 프로젝트 설치 

- Node.js 가 설치되어 있지 않다면, [Node.js 설치](Node.js%20설치.md)    
	
### 새로운 프로젝트 디렉터리를 생성하고 Electron 설치

```sh
> mkdir output 
> cd output 
> npm init -y 
> npm install electron --save-dev
```

![electron_install](/Resources/electron_install.png)

 ![electron_path_error](/Resources/electron_path_error.png)
 
 - 위와 같은 에러가 발생한다면 [Trouble Shooting] 참조

## Electron 설정

### `package.json`파일 수정`Electron 설정 추가

```json
{   
  "scripts": { 
    "start": "electron ." 
  }, 
}
```


![add_electron](/Resources/add_electron.png)

### main.js 파일 추가
- 프로젝트 디렉터리에 `main.js` 파일을 생성하고 아래 내용 추가

```js
const { app, BrowserWindow } = require('electron'); 
const path = require('path'); 
function createWindow() { 
  const mainWindow = new BrowserWindow({ 
    width: 800, 
    height: 600, 
    webPreferences: { 
      nodeIntegration: true 
    } 
  }); 
  
  //mainWindow.loadFile(path.join(__dirname, '../site/index.html')); 
  mainWindow.loadFile(path.join(__dirname, 'site', 'index.html')); 
} 
app.on('ready', createWindow); app.on('window-all-closed', () => { 
  if (process.platform !== 'darwin') { 
    app.quit(); 
  } 
}); 
app.on('activate', () => { 
  if (BrowserWindow.getAllWindows().length === 0) { 
    createWindow(); 
  } 
});
```

- 이 파일은 Electron 창을 생성하고, MkDocs 빌드된 HTML 파일을 로드
- ~~`main.js` 파일에서 `mainWindow.loadFile` 함수가 `site/index.html` 파일을 로드하도록 설정했음을 확인~~
- site 폴더를 상위 폴더로 설정하는 경우 경로가 정상적으로 설정되지 않는 경우가 있다.
	- site 폴더를 output 폴더 안으로 복사하고 사용하는 것으로 변경경
	- `site` 폴더는 MkDocs 빌드 파일이 있는 폴더

## Electron 실행

- output 폴더에서 다음 명령어를 실행하여 Electron 애플리케이션 시작

```sh
> npm start
```

![electron_run](/Resources/electron_run.png)

![electron_local](/Resources/electron_local.png)

## Electron 패키징

Electron 애플리케이션을 단일 실행 파일로 패키징하기 위해 `electron-packager` 또는 `electron-builder`와 같은 도구를 사용
### electron-builder를 사용하여 설치 버전 패키징`electron-builder` 설치

```sh
> npm install --save-dev electron-builder
```

![electron_builder](/Resources/electron_builder.png)

- package.json 파일 수정
	- `build` 스크립트와 `build` 설정을 `package.json` 파일에 추가

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
      //"../site/**/*" 
    ], 
    "directories": { 
      "output": "dist" 
    } 
  } 
}
```

- Electron 애플리케이션 빌드

```sh
> npm run build
```

![electron_build](/Resources/electron_build.png)

- `dist` 폴더에 실행 파일 생성 확인

![electron_output](/Resources/electron_output.png)- 

### electron-builder 를 사용하여 실행 파일 패키징
-  `package.json` 파일 설정
	- `build` 스크립트 추가
### electron-packager 사용
- `electron-packager` 설치

```sh
> npm install electron-packager --save-dev
```

- 다음 명령어를 실행하여 애플리케이션 빌드

```sh
> npx electron-packager . my-electron-app --platform=win32 --arch=x64 --out=dist --overwrite
```

- `dist` 디렉터리에 실행 가능한 파일 생성 확인
