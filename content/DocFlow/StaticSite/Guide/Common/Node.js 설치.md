## Node.js & npm 설치

-  [Node.js 공식 사이트](https://nodejs.org/)에서 다운로드 및 설치
	- LTS(Long Term Support) 버전을 다운로드하는 것이 안정적입니다. 
	- 현재 권장 버전을 다운로드합니다.
 ![Node.js_Setup](/Resources/nodejs_setup.png)
- npm은 Node.js 패키지 관리자로, Node.js 설치 시 함께 설치됩니다

## 설치 확인

설치가 완료된 후, 명령 프롬프트(CMD)나 PowerShell을 열고 다음 명령어를 입력하여 Node.js와 npm이 제대로 설치되었는지 확인합니다.

```sh
> node -v
> npm -v
```

```sh
C:\>node -v
v20.15.0

C:\>npm -v
10.8.1

C:\>
```

## nvm 을 이용한 버전 관리 (옵션)

### nvm-windows 다운로드 및 설치
- [nvm-windows Releases 페이지](https://github.com/coreybutler/nvm-windows/releases)로 이동합니다.
- 최신 릴리스의 `nvm-setup.zip` 파일을 다운로드합니다.
- 다운로드한 `nvm-setup.zip` 파일을 압축 해제한 후, `nvm-setup.exe` 파일을 실행합니다. 
- 설치 마법사에 따라 설치를 진행합니다.
- 명령 프롬프트(CMD)나 PowerShell 이 실행중이라면 종료 후 재실행합니다.

### nvm으로 Node.js 설치
- 설치 가능한 Node.js 버전 확인

```sh
> nvm list available
|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    22.2.0    |   20.14.0    |   0.12.18    |   0.11.16    |
|    22.1.0    |   20.13.1    |   0.12.17    |   0.11.15    |
|    22.0.0    |   20.13.0    |   0.12.16    |   0.11.14    |
|    21.7.3    |   20.12.2    |   0.12.15    |   0.11.13    |
|    21.7.2    |   20.12.1    |   0.12.14    |   0.11.12    |
|    21.7.1    |   20.12.0    |   0.12.13    |   0.11.11    |
|    21.7.0    |   20.11.1    |   0.12.12    |   0.11.10    |
|    21.6.2    |   20.11.0    |   0.12.11    |    0.11.9    |
|    21.6.1    |   20.10.0    |   0.12.10    |    0.11.8    |
|    21.6.0    |    20.9.0    |    0.12.9    |    0.11.7    |
|    21.5.0    |   18.20.3    |    0.12.8    |    0.11.6    |
|    21.4.0    |   18.20.2    |    0.12.7    |    0.11.5    |
|    21.3.0    |   18.20.1    |    0.12.6    |    0.11.4    |
|    21.2.0    |   18.20.0    |    0.12.5    |    0.11.3    |
|    21.1.0    |   18.19.1    |    0.12.4    |    0.11.2    |
|    21.0.0    |   18.19.0    |    0.12.3    |    0.11.1    |
|    20.8.1    |   18.18.2    |    0.12.2    |    0.11.0    |
|    20.8.0    |   18.18.1    |    0.12.1    |    0.9.12    |
|    20.7.0    |   18.18.0    |    0.12.0    |    0.9.11    |
|    20.6.1    |   18.17.1    |   0.10.48    |    0.9.10    |
This is a partial list. For a complete list, visit https://nodejs.org/en/download/releases
>
```

- Node.js 설치
	- 원하는 버전의 Node.js를 설치합니다. 
	
```sh
> nvm install node   # 최신 버전 설치
> nvm install lts  # LTS 버전 설치
```

- Node.js 사용 설정
	- 설치한 Node.js 버전을 사용 설정합니다.

```sh
> nvm use 14.17.0 # 14.17.0 버전 사용
> nvm use lts # LTS 버전 사
```

```sh
C:\>nvm use lts
node v20.15.1 (64-bit) is not installed.

C:\>nvm install lts
Downloading node.js version 20.15.1 (64-bit)...
Extracting node and npm...
Complete
npm v10.7.0 installed successfully.


Installation complete. If you want to use this version, type

nvm use 20.15.1

C:\>nvm use lts
Now using node v20.15.1 (64-bit)
```

- 설치 확인
	- Node.js와 npm이 제대로 설치되었는지 확인합니다.

```sh
> node -v
> npm -v
```

## NPM 버전 관리

`nvm` (Node Version Manager)은 주로 Node.js 버전을 관리하는 도구입니다. 따라서, `nvm`을 사용하여 npm 버전만 별도로 변경할 수는 없습니다. 하지만, Node.js 버전과는 별도로 npm을 업데이트하거나 다운그레이드할 수 있습니다.

npm 버전을 변경하는 방법은 다음과 같습니다:

###  npm 업데이트
- 최신 버전으로 업데이트

```sh
npm install -g npm@latest
```

- 특정 버전으로 업데이트

```sh
npm install -g npm@{버전번호}
```

``