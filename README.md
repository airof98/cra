# cra

https://chanyeong.com/blog/post/7

## cra(create react app) 없이 react + typecript 셋팅하기
- react, typescript 설치
```
npm init -y
npm i react react-dom
npm i -D typescript @types/react @types/react-dom
```
- typescript 설정
```
node_modules/.bin/tsc --init
```
tcconfig.json
```
{
  "compilerOptions": {
    "sourceMap": true,
    "target": "es5",
    "lib": ["dom", "ES2015", "ES2016", "ES2017", "ES2018", "ES2019", "ES2020"],
    "allowJs": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "esModuleInterop": true,
    "module": "commonjs",
    "isolatedModules": true,
    "jsx": "preserve",
    "allowSyntheticDefaultImports": true,
    "baseUrl": "./",
    "outDir": "./dist",
    "moduleResolution": "node"
  },
  "exclude": ["node_modules"],
  "include": ["**/*.ts", "**/*.tsx"]
}
```
- index.tsx, App.tsx, index.html 생성
- Babel 설치
```
npm i -D babel-loader @babel/core @babel/preset-env
npm i -D @babel/preset-react @babel/preset-typescript
```
babel.config.js
```
module.exports = {
  presets: ['@babel/preset-react', '@babel/preset-env', '@babel/preset-typescript'],
};
```
- webpack 설치
```
npm i -D webpack webpack-cli webpack-dev-server
npm i -D html-webpack-plugin ts-loader
```
webpack.config.js
```
const HtmlWebpackPlugin = require('html-webpack-plugin');

const prod = process.env.NODE_ENV === 'production';

module.exports = {
  mode: prod ? 'production' : 'development',
  devtool: prod ? 'hidden-source-map' : 'eval',

  entry: './src/index.tsx',

  resolve: {
    extensions: ['.js', '.jsx', '.ts', '.tsx'],
  },

  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: ['babel-loader', 'ts-loader'],
      },
    ],
  },

	output: {
    path: path.join(__dirname, '/dist'),
    filename: 'bundle.js',
  },

  plugins: [
		new webpack.ProvidePlugin({
      React: 'react',
    }),
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ],
};
```
- webpack-dev-server  
webpack-config.js
```
const webpack = require('webpack');
const path = require('path');

...
module.exports = {...
	devServer: {
    historyApiFallback: true,
    inline: true,
    port: 3000,
    hot: true,
    publicPath: '/',
  },

	plugins: [
		...,
    new webpack.HotModuleReplacementPlugin(),
		...,
  ],
...
}
```
- srcipts   
package.json
```
"scripts": {
    "dev": "webpack-dev-server --mode development --open --hot",
    "build": "webpack --mode production",
    "prestart": "npm build",
    "start": "webpack --mode development"
},
```  