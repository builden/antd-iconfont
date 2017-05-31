# antd-iconfont
antd离线字体

## 要解决的问题
1. antd默认iconfont指向的是阿里在公网CDN上部署的url
2. 项目需要在本地进行部署，特别是打包在Electron中运行的时候，使用的是本地文件的访问方式，希望能离线使用

## 在create-react-app中的配置方法
1. 安装依赖
```bash
$ yarn add antd-iconfont
$ yarn add babel-plugin-import -D
$ yarn add less less-loader -D
```

2. 配置babel插件
```json
// 注意style要设置成true，使用less而非css
{
  "babel": {
    "plugins": [
      [
        "import",
        [
          {
            "libraryName": "antd",
            "style": true
          }
        ]
      ]
    ]
  }
}
```

3. 配置webpack.xxx.config.js
```js
// module => rules中增加项
// 通过修改modifyVars中的值修改默认antd.less的默认参数
{
  test: /\.less$/,
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        ident: 'postcss', // https://webpack.js.org/guides/migrating/#complex-options
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
    {
      loader: require.resolve('less-loader'),
      options: {
        modifyVars: {
          '@icon-url': '"~antd-iconfont/iconfont"',
        },
      },
    },
  ],
},
```


## 参考
* [iconfont原始下载地址] (https://ant.design/docs/resource/download)