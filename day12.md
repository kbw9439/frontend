### 1. node-sass 명령어
#### 1.1 embed  
' $ node-sass -w -r sass/ -o css/ --output-style expanded --source-map-embed '
- ' --source-map-embed' :  출력되는 css파일내에 source-map 정보를 내제함.  

#### 1.2 npm init  / npm init -y
##### 1.2.1 npm init
'$ npm init' : 해당 디렉토리내에 package.jason 파일 생성을 도와 주는 명령어

`$ npm init`
```
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
```
interactive mode로 필요한 정보를 입력
```
name: (day_03-1)          : 디렉토리명 기본값
version: (ㅞㅡ )          : 애플리케이션의 버전
description:              : 프로젝트 설명
entry point: (index.js)   : 메인으로 사용하는 js파일
test command:             : 명령어 등록
git repository:           : git repository 설정     
keywords:                 : 키워드
author:                   : 작성자
license: (ISC)
About to write to C:\Users\kbw94\Desktop\frontend\yamoo\day_03-1\package.json:
```
- 각 항목에 값을 입력하지 않고 엔터를 치면 기본값이 된다.
```
{
  "name": "day_03-1",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}

  Is this ok? (yes) yes

```
-  yes입력 후 enter를 치면 프로젝트 폴더 하위에 `package.json` 파일이 생성됨.

##### 1.2.2 npm init -y
- 각 항목 입력없이 기본값으만 설정하여 package.json파일 바로 생성됨
- 에디터에서 해당디렉토리 내 package.json파일을 열어 수정처리함.


### 2. node-sass 명령어 등록
- package.json파일 내 scripts 항목에 sass 명령어 등록()단, 해당명령어는 해당 디렉토리 내에서만 실행됨.)
```
"scripts": {
  "sass": "node-sass -w -r sass/ -o css/ --output-style expanded --source-map-embed",
  "sass-build": "node-sass -r sass/ -o css/ --output-style compressed"
}
```

#### 2.1 등록한 node-sass 명령어 실행
   - $ npm run (등록한 명령어)
```
   $ npm run sass
   > sass-basic-study@0.0.1 sass C:\Users\kbw94\Desktop\frontend\yamoo\day_03
   > node-sass -w -r sass/ -o css/ --output-style expanded --source-map-embed

   => changed: C:\Users\kbw94\Desktop\frontend\yamoo\day_03\sass\sass-scripts.sass
   Rendering Complete, saving .css file...
   Wrote CSS to C:\Users\kbw94\Desktop\frontend\yamoo\day_03\css\sass-scripts.css
```

### 3. Placeholder Selector(대체 선택자)
- 대체 선택자(%): %기호가 붙으면 css는 해석하지 않는다.
```
CSS코드 변경시, 출력되지 않음
%reset-mp {
  margin: 0;
  padding: 0; }
```
```  
#app {
  @extend %reset-mp; }
#design {
  @extend %reset-mp; }
```
