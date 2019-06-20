### WHAT IS WEBPACK?

### HOW TO GET START

- yarn init
- yarn add webpack webpack-cli --dev
- touch index.js
- mrdir webpack
- touch ./webpack/webpack.config.js
- open package.json ->

```
"scripts": {
  "build": "webpack --config ./webpack/webpack.config.js",
  "dev": "webpack-dev-server --open --config ./webpack/webpack.dev.js",
  "start": "yarn build && yarn dev"
}
```

======== ======== ======== ======== ======== ========

### WEBPACK - WEBPACK-CI

========

```
yarn add webpack webpack-cli
```

or

```
yarn global add webpack webpack-cli
```

Nếu cài global có thể comman line chạy webpack luôn

```
webpack --watch ./file-name.js Chạy mode="xxx" webpack --watch --mode="development" ./file-name.js
```

File webpack.config.js

```
const path = require('path');
module.exports = {
  entry: {
    index: './src/index.js'
  },
  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: '[chunkhash].[name].js',
    publicPath: ''
  }
}
```

======== ======== ======== ======== ======== ========

### WEBPACK-DEV-SERVER

========

Sử dụng để chạy server local hoặc production, cũng tương tự browser-sync, nhưng có nhiều option hơn
https://webpack.js.org/configuration/dev-server/#devservercontentbase

```
yarn add webpack-dev-server --dev
webpack-dev-server --open --config ./webpack/webpack.dev.js
```

```
yarn add webpack-dev-server
```

File webpack.dev.js

```
const path = require('path')
const merge = require('webpack-merge')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const config = require('.webpack.config.js')

module.exports = merge(config, {
  devServer: {
    contentBase: '../dist',
    host: '0.0.0.0',
    port: 3000,
    publicPath: '/',
    historyApiFallback: true,
    hot: true
  },
  plugins: [
    new CleanWebpackPlugin()
  ]
})
```

======== ======== ======== ======== ======== ========

#### [hash], [chunkhash], [ext], [contenthash]

========

Link view: https://survivejs.com/webpack/optimizing/adding-hashes-to-filenames/

- hash: sử dụng cho các url tập tin file-loader, (img, fonts v.v...)
- chunkhash: sử dụng cho output xuất file của webpack
- contenthash: MiniCssExtractPlugin use [contenthash]

======== ======== ======== ======== ======== ========

#### .ENV Sử dụng biến môi trường enviroment

========

https://codeburst.io/process-env-what-it-is-and-why-when-how-to-use-it-effectively-505d0b2831e7

#### WHAT IS ENVIROMENT?

========

#### WHY TO USE IT?

========

#### WHEN TO USE IT?

========

- Phải phân biệt rõ ràng là các enviroment này được sử dụng ở đâu
- Sử dụng cho file config sẽ khác, webpack.config.js
- Sử dụng vào trong file làm việc sẽ khác .js, .jsx của Reactjs

Có nghĩa là nó sẽ đi tương ứng như thế này:

Tất cả các plugin sẽ có cơ chế cho bạn add enviroment, từ npm, node, webpack, docker ...

Ví dụ enviroment cho webpack:

```
plugins: [
  new webpack.DefinePlugin({
    'process.env': [
      ENV_POST: 3000
    ]
  })
]
```

Thì sau bạn chạy webpack, tất cả các file .js được run bằng webpack sẽ dùng được enviroment này.

Ví dụ dùng npm or yarn, thì setup thường được thấy nhất là ở package.json:

```
  scripts: {

  }
```

#### HOW TO USE IT EFFECTIVELY?

========

#### HOW TO USE IT WITH WEBPACK / REACTJS / DOCKER /?

========

#### .ENV + WEBPACK + REACTJS

========

#### Đối với react-scripts@0.x.x

Link chi tiết: https://facebook.github.io/create-react-app/docs/adding-custom-environment-variables

Cũng sẽ setup config cho file .env, nhưng mặc định luôn luôn bắt đầu với
`REACT_APP_xxx`, xxx phía sau là muốn custom gì thì custom

File config .env

```
REACT_APP_PORT=3000
```

Sử dụng vào trong file .js or .jsx: `process.env.REACT_APP_PORT`

#### Không dùng react-scripts@0.x.x

========

Có khá nhiều library, bạn có thể sử dụng tuỳ cái nào mong muốn:

- cross-env: https://www.npmjs.com/package/dotenv
- dotenv: https://www.npmjs.com/package/dotenv

Dùng cross-env tạo biến môi trường để chạy mode:'devlopment' or mode: 'production' khi dùng lệnh:

```
yarn dev
```

e.g:

```
"scripts": {
  "dev": "cross-env NODE_ENV=development webpack --config webpack.config.js"
}
```

Sử dụng tạo biến môi trường theo yêu cầu:
// Set cross-env NODE_ENV=production
// Get and use in file webpack.config.js const get = process.env.NODE_ENV

========
Còn với dotenv bạn cần tạo 1 file config => .env

```
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```

Gọi vào config webpack.config.js

```
require('dotenv').config()
```

Nếu là server nodejs cũng tương tự, và chỉ cần chạy server là xong không cần setup thêm trong package.json

======== ======== ======== ======== ======== ========

### MORE - TREE-SHAKING - OPTIMIZATION

========

Cả ba cũng có thể gọi nó là tree-shaking

```
module.exports = {
  mode: 'none',
  mode: 'development',
  mode: 'production'
};
```

- none : không làm gì cả
- development : tạo ra các file tương ứng để phục vụ cho dev
- production : devlop, nên sẽ tối ưu, min file v.v...

Thật ra tree-sharing nó chính là Optimization của webpack v4.0

```
module.exports = {
  optimization: {
    minimize: false, // developement
    minimize: true   // by default production
  }
}
```

Khi chúng ta có làm việc với các library trong node_modules thông qua package.json thì ta nên khai báo cho webpack biết là nên tree-sharing luôn những phần lib đó ta khai báo:

```
{
  "sideEffects": false,
  // or
  "sideEffects": ["*.css", "*.js"],
}
```

======== ======== ======== ======== ======== ========

### BABEL IJ

========

Hiện tại javascript mỗi ngày đều có nhiều function, method mới xuất hiện, mà các browser chưa kịp hỗ trợ, vai trò của babel là dịch ngược lại các phiên bản javascript cũ hơn để mọi browser có thể đọc được.

```
"babel-loader": "^8.0.6",
"@babel/core": "^7.4.5",
"@babel/preset-env": "^7.4.5",
"@babel/preset-react": "^7.0.0",
"plugin-syntax-dynamic-import": "^7.2.0"
```

- Webpack helper to transform your JavaScript dependencies (for example, when you import your components into other components) with Babel "babel-loader": "^7.1.4", "babel-loader": "^8.0.6",

- Transforms your ES6 code into ES5 "babel-core": "^6.26.3", "@babel/core": "^7.4.5",

- Determines which transformations/plugins to use and polyfills (provide modern functionality on older browsers that do not natively support it) based on the browser matrix you want to support "babel-preset-env": "^1.7.0", "@babel/preset-env": "^7.4.5",

- Babel preset for all React plugins, for example turning JSX into functions "babel-preset-react": "^6.24.1", "@babel/preset-react": "^7.0.0",

- Hỗ trợ sử dụng syntax trong khi làm việc với code spliting "babel-plugin-syntax-dynamic-import": "^6.18.0", "@babel/plugin-syntax-dynamic-import": "^7.2.0",

Install library & loader

```
yarn add babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/plugin-syntax-dynamic-import --dev
```

Setup file .babelrc, nên tạo file riêng để dễ maintain hơn.

```
{
  "presets" : [
    ['babel-preset-env', { modules: false }],
    'react',
  ],
  "plugins" : ["syntax-dynamic-import"]
}
```

Setup file webpack.config.js

```
module: {
  rules: [
    test: /\.js$/,
    exclude: /(node_modules)/,
    use : 'babel-loader'
  ]
}
```

Hoặc chỉ mỗi file webpack.config.js nếu không muốn tách ra .babelrc, xoá file này đi, để lại là báo lỗi đấy, vì nó ưu tiên đọc file này trước

```
module : {
  rules: [
    {
      // Tìm tất cả các file có đuôi .js
      test: /\.js$/,
      // Loại bỏ file or file không muốn sử dụng
      exclude: /(node_modules)/,
      // Sử dụng loader với các options của nó
      use: {
        loader: 'babel-loader',
        options: {
          // presets: ['es2015']
          presets: ['@babel/preset-env'] // just update
        }
      }
    },
  ]
}
```

Ngoài ta cần chú ý khi sử dụng với babel-loader file .babelrc

```
{
  "presets" : [
    ["@babel/preset-env", { modules: false }], // Fix error biên dịch Es6-Es5 mà vẫn giữ tree-shaking
    "react",
  ],
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```

======== ======== ======== ======== ======== ========

### CommonsChunkPlugin - Optimization - splitChunks

========

```
optimization: {
  minimize: false, // false | true => mode="production" default is false
  runtimeChunk: false, // true | false | single | default is false
  // true   : runtime~index.js, runtime~listing.js
  // false  : gôm chung vào từng file luôn index.js, listing.js
  // single : runtime.js dùng all pages, tên nào ra tên đó, nên dùng single
  // Default runtime.js -> tách nhỏ ra nhiều file để loading nhanh hơn, e.g: chrome 5 resquest 1 lần
  // runtimeChunk: {
  //   name: 'custom_name' -> custom_name.js
  // }
  splitChunks: {
    chunks: "async", // async | all | initial
    // async   : để y trong file index.css, listing.css
    // all     : gôm ra ngoài với tên tương ứng nếu có slipting index~listing.css ... (gôm cả đồng bộ và
    //           bất đồng bộ) dynamically imported modules
    // initial : gần giống với 'all' index~listing.css chỉ lấy các giá trị tĩnh import từ entry
    minSize: 30000, // Kích thước tối thiểu mà spliting sử dụng
    maxSize: 0, // Lỗi không setup dc
    // Qui định file modules được sử dụng chung trên 1,2,3 file trong project sẽ được splitChunks
    minChunks: 1,

    // Số request tối đa
    maxAsyncRequests: 5,
    // Số request tối thiểu
    maxInitialRequests: 3,

    // index~listing.js thay đổi cái '~' thành bất cứ cái gì mà bạn muốn
    automaticNameDelimiter: "~",

    name: true, // false nó sẽ xin ra theo 1.js 2.js, default is true
    // Tên sẽ là tên tương ứng của các file có dùng chung file modules với nhau
    // index~listing.css, vendors~index~listing.css
    // name: tuds, nó sẽ lệ thuộc vào minChunks để để cho ra file tuds.css, tuds.js tương ứng
    // không còn kiểu index~listing.css nữa mà là gôm lại những cái sử dụng chung vào tuds.css

    // CACHEGROUPS
    // Qui định thiết lập riêng cho từng file, và từng folder cụ thể,
    // Webpack default đã có sẵn 2 set sỉnh ra file vendors khi sử dụng các library trong node_modules
    // File name tương ứng cho modules custom sử dụng trên 2 file trong project
    // Để tắt các default có sẳn:
    // 1 là ta sử dụng default: false rồi setup
    // 2 là ta setup luôn, khi trùng tên nó sẽ tự động lấy Object mà ta setup
    // Bên trong mỗi item của cacheGroups,
    // tức là từng file cụ thể ta lại có thể setup riêng biệt về các qui định option cho riêng nó
    cacheGroups: {
      vendors: true, // true | false
      default: true, // true | false
      // true  : sử dụng cacheGroups options default, dùng 2 splitChunks: vendors, default
      // false : tắt hết default, vendors tự setup lại với config theo ý của mình

      // Làm việc với các file trong node_modules Default của webpack
      // nhận các giá trị bên ngoài từ các options của splitChunks
      vendors: {
        // name : '',
        // Không set name nên nó sẽ là vendors~index.js, vendors~listing.js nếu có sử dụng
        // Library của node_module
        test: /[\\/]node_modules[\\/]/, // sử dụng các file có trong node_modules
        priority: -10 // Độ ưu tiên khi được thêm vào DOM HTML <link> or <script>
        // Options của từng item bên trong sẽ kế thừa splitChunks bên ngoài
        // minChunks: 1,
        // minSize: 30000,
        // ...
      },
      // Làm việc với các tất cả các file trong project của chúng ta thông qua chunks: 'all' | 'inital'
      default: {
        // name : '',
        // Không set name -> nó sẽ là index~listing.js, index~listing~contact.js ... nếu các file được sử dụng ở nhiều pages
        // name : 'commons',
        // Có set name thì sẽ tự động tạo file tương ứng là commons.css or commons.js và gôm những modules sử dụng theo minChucks vào trong này.

        minChunks: 2, // file modulse được sử dụng chung trên 2 file trong project
        priority: -20, // Độ ưu tiên khi được thêm vào DOM HTML <link> or <script>
        reuseExistingChunk: true, // reuseExistingChunk nói cho SplitChunksPlugin sử dụng đoạn mã có sẵn thay vì tạo mới.
        // Ví dụ: nếu bất kỳ mô-đun nào được nhập bên trong mã chunk chung là một phần của đoạn mã khác, thì thay vì tạo đoạn mới cho nó, một mô-đun cũ được sử dụng lại thay thế.
        enforce: true // Với giá trị mặc định của kích thước file là 30kb thì chỉ cần set enforce: true là chung 2 file nó sẽ gôm lại.

        // #### ===== Hoạt động sau vendors
        // File nằm trong project + tính luôn file không thoả điều kiện ở trong node_modules,
        // Có kích thước lớn hơn 30kb, nhỏ hơn không chơi
        // Sử dụng tối đa ở 2 trang trở lên
        // Thì thoả điều kiện -> tạo ra file của các trang tương ứng (e.g index~listing.js)
      }
    }
  }
}
```

======== ======== ======== ======== ======== ========

### TEMPLATE HTML

========

```
yarn add html-webpack-plugin --dev
```

Link view: https://github.com/jantimon/html-webpack-plugin

```
mkdir html
touch index.html
```

File webpack.config.js nằm trong folder webpack

```
const HtmlWebpackPlugin = require('html-webpack-plugin');

module: {
  plugins: [
    new HtmlWebpackPlugin({
      file: 'index.html', // file name output
      template: path.resolve(__dirname, '../html/index.html')
    })
  ]
}
```

======== ======== ======== ======== ======== ========

### STYLE CSS

========

```
"style-loader":"0.23.1"   // Creates style nodes from JS strings
"css-loader": "^2.1.1",   // Translates CSS into CommonJS

"sass-loader": "^7.1.0",  // Compiles Sass to CSS, using Node Sass by default
"node-sass": "^4.12.0"   // It allows you to natively compile .scss files to css.
```

1. https://github.com/webpack-contrib/style-loader
2. https://github.com/webpack-contrib/css-loader
3. https://github.com/webpack-contrib/sass-loader
4. https://github.com/sass/node-sass
5. https://github.com/webpack-contrib/mini-css-extract-plugin
6. https://github.com/NMFR/optimize-css-assets-webpack-plugin

- Webpack cũng cung cấp 1 loader sass-loader, nguyên tắt hoạt động cũng tương ứng từ phải sáng trái, từ dưới lên trên.

```
style-loader <- css-loader <- sass-loader
```

1. Biên địch sass thành css
2. Biên dịch các css trong file js component
3. Chuyển tìm được trong file js component đưa vào trong style của file html

- Sử dụng mini-css-extract-plugin thay thế cho style-loader đứa css trong file main.js thành dạng link gắng vào file .html thay vì đổ vào <style></style> như style-loader vẫn làm.

```
module: {
  rules: [
    {
      test: /\.(sa|sc|c)ss$/,
      use: [
        {
          loader: MiniCssExtractPlugin.loader,
          options: {
            modules: true,
            sourceMap: true,
            importLoader: 2 // Dùng 2 loader, nếu dùng 3 thì sửa lại là 3
          }
        },
        {
          // Translates CSS into CommonJS
          loader: 'css-loader',
          options: {
            sourceMap: true
          }
        },
        {
          // Compiles Sass to CSS, using Node Sass by default
          loader: 'sass-loader',
          options: {
            sourceMap: true
          }
        }
      ]
    }
  ]
}
```

======== ======== ======== ======== ======== ========

### CSS OPTIMIZE - TREE SHAKING CSS

========

Link view: https://github.com/NMFR/optimize-css-assets-webpack-plugin

Với webpack 4 khi chúng ta thực hiện build ở môi trường mode="production":

- Không có hiêu quả với scss, css, nên ta sẽ phải tự viết hoặc sử dụng 1 plugin thứ 3
- Đó chính là https://github.com/NMFR/optimize-css-assets-webpack-plugin

Lợi ích sử dụng OptimizeCssAssetsPlugin:

- Xoá class double khi viết css
- Xoá khoản trắng, whitespace
- Cssnano, 10px 10px 5px 5px (auto) -> 10px 5px
- Min Css, nén file giảm dung lượng
- Plugins được tích hợp khá okay với MiniCssExtractPlugin

Install library

```
yarn add optimize-css-assets-webpack-plugin --dev
```

Options các giá trị mặc định của 'optimize-css-assets-webpack-plugin'

```
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')
// ...

plugins: [
  new OptimizeCssAssetsPlugin({
    assetNameRegExp: /\.css$/g,          // giống test của webpack, file có .css thì min
    cssProcessor: require('cssnano'),    // cssnano syntax, min, xoá khoản trắng, merge các giá trị vaule của css.
    cssProcessorPluginOptions: {         // truyền option
      preset: [
        'default',
        { discardComments: {
          removeAll: true
        } }
      ],
    },
    canPrint: true
  })
]
```

======== ======== ======== ======== ======== ========

### IMAGES - FONTS

========

======== ======== ======== ======== ======== ========

### REACTJS

========

```
yarn add react react-dom react-router-dom --dev
```

Create index.js file the same:

```
import React from 'react';
import ReactDOM from 'react-dom';
import '../sass/style.scss'
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

========

### REACTJS - @BABEL

========

File config .babelrc

```
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

========

### REACTJS - REACT-ROUTER-DOM

========

npm: https://www.npmjs.com/package/react-router-dom
Guides: https://reacttraining.com/react-router/web/guides/quick-start

```
import React from 'react';
import ReactDOM from 'react-dom';
import {
  BrowserRouter,
  Switch,
  Route,
  Link
} from 'react-router-dom';
class App extends React.Component {
  render() {
    return (
      <React.Fragment>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
        </ul>
        <Switch>
          <Route path={'/'} component={Home} />
        </Switch>
      </React.Fragment>
    );
  }
}
ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

BrowserRouter, // Giúp browser biết được là đang ở đâu path:
Link, // Tương tự tag <a>
Route, // Định tuyến show | hide component tương ứng
Switch, // Định nghĩa bên dưới kéo xuống đọc
Redirect // Đá qua trang mà

========

### Exact

========

Cho React biết đây là nội dung chính xác của từng trang

```
<Route exact path="/" component={HomeView} />
```

Nếu gọi 2 lần nó sẽ hiểu là tạo 2 component

```
<Route exact path="/" component={HomeView} />
<Route exact path="/" component={HomeView} />
```

Để khắc phục trường họp này chúng ta sẽ sử dụng <Switch>

```
<Switch>
  {/*Ưu tiên cho home được hiển thị là nội dung chính*/}
  <Route exact={true} path="/home" component={HomeView} />
  <Route path="/listing" component={ListingView} />
</Switch>
```

// Khi tự nhập (typing) route url vào browser cần lưu ý setup thêm ở webpack.dev.js

```
devServer: {
  historyApiFallback: true
}
```

========

### REACTJS - CODE_SPLITTING

========

Link view: https://itnext.io/react-router-and-webpack-v4-code-splitting-using-splitchunksplugin-f0a48f110312

```
"react-loadable": "^5.5.0",
"@babel/plugin-syntax-dynamic-import": "^7.2.0",
"webpack-async-chunk-names-plugin": "^0.1.1",
```

Đương nhiên phải có babel rồi:
"@babel/plugin-syntax-dynamic-import": "^7.2.0",

```
yarn add @babel/plugin-syntax-dynamic-import --dev
```

- Dùng library để có thể tạo code-splitting, dynamic
- Giúp tách các module js cần loading trước hoặc sau.
- Giúp giảm thời gian load timing size

"react-loadable": "^5.5.0",

```
yarn add react-loadable --dev
```

// Khi dynamic thường ra tên file là:
0.js
1.js
2.js
// Vì vậy để khắc phục ta có
"webpack-async-chunk-names-plugin": "^0.1.1",

```
yarn add webpack-async-chunk-names-plugin --dev
```

Tiếp theo setup tiếp trong .babelrc

```
{
  "plugins": ["@babel/plugin-syntax-dynamic-import"],
  // ...
}
```

======== ======== ======== ======== ======== ========

### DOCKER

========

- Docker là một open platform cung cấp cho người sử dụng những công cụ và service để người sử dụng có thể đóng gói và chạy chương trình của mình trên các môi trường khác nhau một cách nhanh nhất.

- Để setup 1 con web server thì cài đặt hơi nhiều thứ
  -> LEMP (Linux, Nginx, Mysql, PHP)
  -> Mysql Databases
  -> Php - phpMyAdmin
  -> Apache
  -> Nodejs
  ...
  E.g:
- mysql.server start
- php start
- npm start
- nginx -s start

Vây mỗi lần start 1 dự án thì cần start 4 thằng.

=> Docker giúp mix cách start của 4 thằng vào 1
=> docker-compose up --build
=> docker run -name container_name -p 8000:8000 image_name

=> Giúp đóng gói cài đặt 1 môi trường chung mà tắt cả các hệ điều hành điều có thể làm việc chung được.

#### Đối với 1 dev ta có 2 môi trường:

1. Môi trường dưới máy local - hay còn gọi là máy tính cá nhân.
2. Môi trường server - hosting - là con vps của amazon, linode, digitalocean ...

- Trường họp hay gặp phải là, cài đặt giả lập chạy ở local ngon, thơm bơ, nhưng khi lên con host - server thì dẹo, lỗi tèm lem
  -> Máy cá nhân dùng MacOs, Window
  -> Server dùng CenOS, Ubuntu
  -> Chung hệ điều hành mà lại khác version ruby2.3(ở local) - ruby 2.2.3 cũng dẹo như thường.

=> Giải quyết đống hỗn độn này với:
-> docker -- (build) --> image -- (run) --> container
-> build tạo file images từ -> Dockerfile
-> run tạo file container từ -> images

#### ĐÓNG GÓI LÀ GÌ?

=> Đóng gói ở đây tức là mang mysql, phpMyAdmin, mysql database ... vào 1 cái thùng, tức là không cần cài cho global máy tính, sau đó run, nó sẽ chỉ chạy trong folder này, không ảnh hương ra bên ngoài và cũng như bên ngoài không ảnh hương vào bên trong

=> Các tiến trình (process) trong một container bị cô lập với các tiến trình của các container khác trong cùng hệ thống tuy nhiên tất cả các container này đều chia sẻ kernel của host OS (dùng chung host OS).

#### Thành phần cơ bản của Docker:

- Image
- Container
- Docker Engine
- Docker Hub

1. Image

- Là file ảnh, file nền của một hệ điều hành, một nền tảng, một ngôn ngữ
  E.g: ubuntu image, ruby image, rails image, mysql image…

- Từ các image này, bạn sẽ dùng nó để tạo ra các container.
- Các image là dạng file-chỉ-đọc (read only file).
- Tự bạn cũng có thể tạo image cho riêng mình.
- Một image có thể được tạo từ nhiều image khác
  E.g: bạn tạo 1 image chạy ubuntu, có cài sẵn ruby 2.3 và rails 5, image này của bạn được tạo nên bởi 3 image khác.

2. Container

- Là một máy ảo, được cấu thành từ 1 image và được đắp thêm 1 lớp “trang trí” writable-file-layer. Các thay đổi trong máy ảo này (cài thêm phần mềm, tạo thêm file…) sẽ được lưu ở lớp trang trí này.

- Các container này sẽ dùng chung tài nguyên của hệ thống (RAM, Disk, Network…), chính nhờ vậy, những container của bạn sẽ rất nhẹ, việc khởi động, kết nối, tương tác sẽ rất nhanh gọn.

- Nếu ánh xạ sang hướng đối tượng, thì image chính là class, còn container chính là instance 1 thể hiện của class đó. Từ 1 class ta có thể tạo ra nhiều instance, tương tự, từ 1 image ta cũng có thể tạo ra được nhiều container hoàn toàn giống nhau.

3. Docker Engine

- Quản lý việc bạn tạo image, chạy container, dùng image có sẵn hay tải image chưa có về, kết nối vào container, thêm, sửa, xóa image và container, ....., vô vàn vô vàn

4. Docker Hub

- Là 1 trang chia sẻ các image, nó như kiểu github hay bitbucket vậy.

========

### Dockerfile

========

- Tất cả điều config điều tồn tại ở đây
- Ngay cả docker hub cũng chỉ là những image được viết sẳn cho chúng ta pull về sử dụng

Tức là:

1. Config Dockerfile

```
FROM nginx:1.15.2-alpine
COPY ./client /var/html/www
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
```

nginx.conf: tự config rồi copy vào nginx image

2. Khởi tạo image.

```
docker build -t image-name .
```

// or nếu muốn đưa lên docker hub để lưu trữ cũng dc

```
docker build -t nginx-proxy-image:latest .
```

3. Khởi tại container với lệnh `run` tương ứng từ Dockerfile, hoặc có thể nói là chạy cái image này (nginx-proxy-global:laster)

```
docker run --name nginx-proxy-global -p 80:80 -t nginx-proxy-image:latest
```

4. Các lệnh views xoá sửa.

// Lệch tạo
docker run --name container_name -d -p 80:80 -it image_name
// Hoặc
docker run --name container_name -d -p 80:80 -p 8000:3000 -it image_name

##### IMAGE

Xem

```
docker images
```

Xoá

```
docker image rm image_name -f
```

##### CONTAINER

Xem container nào đang chạy

```
docker ps
```

Xem tất cả container

```
docker ps -a
```

Xoá

```
docker rm <container_name> -f
// Xoá hết container
docker rm $(docker ps -a -q)
docker rm $(docker ps -aq) -f
```

5. Login host ảo của 1 container nào đó

Ví dụ: Login root container nginx `docker container exec -it ... bash`

```
docker container exec -it nginx-proxy-global bash
```

6. publish docker hub

// Tag the image
docker tag image username/repository:tag

// For example:
docker tag friendlyhello gordon/get-started:part2

// Publish the image
docker push username/repository:tag
// or
docker push username/repository

docker images
REPOSITORY TAG IMAGE ID CREATED SIZE

Lỡ đặt tên images không giống thì chuyển lại
// Tag the image
docker tag image username/repository:tag

image : tên của image muốn copy
username : tên user trên docker
repository : tên đầu tiên của docker images
tag : tên tag tự custom hoặc là version_1.0.0

=> docker tag hello tutruong/repository:tag

======== ======== ======== ======== ======== ========

### NGINX

========

https://viblo.asia/p/tu-xay-dung-cho-minh-ung-dung-tren-docker-RQqKLgDr57z
https://gist.github.com/xaviervia/6adea3ddba269cadb794

Setup
https://linuxize.com/post/nginx-reverse-proxy/

Full Config demo:
https://www.nginx.com/resources/wiki/start/topics/examples/full/

// envsubst: command not found on Mac OS X 10.8
brew install gettext

- Nginx là proxy server dùng để nhận các request từ phía client sau đó sẽ đưa tới rails server

Nếu tự config thì khá phức tạp đấy:

1. https://thachpham.com/linux-webserver/nginx/lemp-cho-vps-phan-3-toi-uu-nginx.html
2. https://vicloud.vn/community/cach-toi-uu-hoa-cau-hinh-nginx-491.html

Đầu tiên kiểm tra cấu hình VPS trước đã rồi hẳn setup
https://hocvps.com/lenh-kiem-tra-cac-thong-so-vps-linux/

Kiểm tra CPU:

```
cat /proc/cpuinfo
```

Kiểm tra phiên bản nhân Linux:

```
uname -a
```

Kiểm tra phiên bản hệ điều hành:

- Với CentOS: `cat /etc/redhat-release`
- Với Ubuntu: `lsb_release -a`

### Without docker-compose.yml

Nếu chưa có nginx thì cài nó vào VPS ubuntu:
https://vicloud.vn/community/cach-cai-dat-nginx-tren-ubuntu-1804-535.html

```
sudo apt update
sudo apt install nginx
```

- Ơ local MacOs thì các config của nginx sẽ nằm ở đây

```
cd /usr/local/etc/nginx
```

- Ubuntu thì các config của nginx sẽ nằm ở đây

```
cd /etc/nginx
```

#### worker_processes

```
worker_processes: 1
```

- Số lượng lỗi mà VPS cung cấp, để xem nó dùng lệnh `nproc`
- Tốt hơn hết, bạn nên thiết lập số lượng process ngang bằng với số lượng CPU mà bạn đang có.

#### worker_connections

- Còn worker_connections nghĩa là số lượng kết nối, resquest được phép sử dụng cho mỗi process.
- Bạn nên đặt con số này là khoảng từ 1024 đến 5000 là đẹp
- 1024 clients/giây
- Nhưng tốt hơn hết là xem VPS cho mình bao nhiêu `ulimit -n`

```
worker_connections: 1024
```

========

### Static HTML + nginx-proxy + docker-compose.yml

========

- nginx-proxy: Hứng request từ client gửi lên, sau đó phân bổ ra lại cho từng con server tương ứng trong vps của mình.

Ví dụ: nginx-proxy docker-compose.yml -> nginx_container -> nginx_images

File docker-compose.yml

```
nginx-proxy:
  container_name: nginx-proxy-global
  image: jwilder/nginx-proxy:alpine
  volumes:
    - /var/run/docker.sock:/tmp/docker.sock:ro
  ports:
    - "80:80"
```

Ví dụ: nginx static docker-compose.yml -> nginx_container -> nginx_images

```
nginx:
  image: nginx:1.15-alpine
  volumes:
    - ./dist:/usr/share/nginx/html:ro
  environment:
    VIRTUAL_HOST: sub.domain.tuds
  command: [nginx-debug, '-g', 'daemon off;']
```

Như ví dụ này, nó sẽ hứng tất cả request rồi sẽ trả lại cho 'sub.domain.tuds' với config nginx bên trong nữa, chạy static html thôi nha.

Mỗi docker-compose.yml có thể được setup tạo ra các container đọc lập: hoặc gôm chung tuỳ nhu cầu sử dụng.

Tốt nhất nên có 1 nginx-proxy hứng bên ngoài cùng default port 80:80, sau đó phân bổ chia chát vào bên trong là các nginx:alpine để lắng nghe.

docker-compose stop nginx-proxy ngoài cùng cũng tương tự là đống tất cả các request vào server tổng
https://drive.google.com/file/d/107r2hapgho4x_Te7waKxyFl5n5n-ZROu/view?usp=sharing

========

### Nginx + Nodejs + docker-composer.yml

========

- Tương tự, nếu con VPS chưa có nginx-proxy thì tạo 1 container duy nhất để hứng thôi
- Không cần tạo quá nhiều trong từng docker-compose.yml làm gì thêm mệt, trừ khi muốn dùng \_load balancing\*

- Load balancing: kiểu AB testing F5 1 cái chuyển qua lại server đã setup.

Tự set config, copy file của chúng ta đến thư mục của container nginx.

```
  volumes:
    - ./nginx.conf/nginx.conf:/etc/nginx/nginx.conf
```

File nginx.conf nếu tự setup, không thì xoá nó đi không cần làm gì.

```
worker_processes  1;
events {
  worker_connections  1024;
}
http {
  include       mime.types;
  default_type  application/octet-stream;

  sendfile        on;
  keepalive_timeout  65;
  # include /etc/nginx/conf.d/*;

  upstream my-app {
    server 172.17.0.1:9000 weight=1;
    #server 172.17.0.2:9000 weight=1;
  }

  server {
    listen 80;
    listen [::]:80;
    root /var/www/html;

    # index index.html index.htm index.nginx-debian.html;

    server_name sub.domain.tuds;

    location / {
      proxy_http_version 1.1;
      proxy_pass http://my-app;
    }
  }
}
```

Setup không cần nginx.conf, nginx-proxy sẽ hứng request thông qua

```
VIRTUAL_HOST: adminer.domain.tuds
```

File docker-compose.yml proxy

```
nginx-proxy:
  container_name: nginx_proxy_global
  image: jwilder/nginx-proxy:alpine
  volumes:
    - /var/run/docker.sock:/tmp/docker.sock:ro
  ports:
    - "80:80"
```

File docker-compose.yml nodejs server

```
node:
  container_name: node_your_name
  image: 'node:10.16-alpine'
  working_dir: /var/www/html
  volumes:
    - ./:/var/www/html
  ports:
    - 4001:4001
  environment:
    VIRTUAL_HOST: adminer.domain.tuds
  command: 'npm run server'
```
