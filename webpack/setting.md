## 웹팩 설정하기
웹팩 설정하기가 생각보다 쉽지 않았다. 일단 공통적으로 해야하는 일은
1. webpack 설치
2. loader 및 필요한 파일들을 -D 옵션으로 설치
- "css-loader": "^0.28.7",
-  "extract-text-webpack-plugin": "^3.0.2",
-  "html-webpack-plugin": "^2.30.1",
-  "style-loader": "^0.19.1",
-  "webpack": "^3.10.0",
-  "webpack-dev-server": "^2.9.7"
3. wepack.config.js 파일 생성
- entry : 제일 메인 file
- output : output file name과 그 파일들이 위치할 장소
	- path.resolve('dist')하면 실제로 dist 폴더가 생기지는 않고 가상으로 만드는 것 같다.
- module
	- rule {
        test: /\.css/,
        exclude: /node_modules/,
        use: ExtractTextPlugin.extract({
          fallback: 'style-loader',
          use: 'css-loader'
        })
      }
      // css와 같은 파일을 로드하는 loader를 설정한다.
      // ExtractTextPlugin을 사용하면 css파일을 output 폴더에 생성한다.
- plugins
	- 추가적으로 사용할 플러그인을 등록한다.
- devServer
	- 가상 서버에 띄울 code가 담겨있는 폴더의 path를 입력하면
	- webpack-dev-server --open 라고 입력했을 때 브라우저가 뜨면서 index.html파일이 실행된다.
4. npm scripts
- "build": "webpack --config webpack.config.js",
- "start": "webpack-dev-server --open",
- "watch": "webpack --watch",
- "test": "echo \"Error: no test specified\" && exit 1"
