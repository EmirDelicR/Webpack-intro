# Webpack-intro

How to use webpack - intro

```console
npm init
npm install --save-dev webpack@3 webpack-dev-server@2 webpack-cli
npm install --save react react-dom react-router-dom 
npm install --save-dev css-loader style-loader
npm install --save-dev postcss-loader
npm install --save-dev autoprefixer
npm install --save-dev url-loader
npm install --save-dev file-loader
npm install --save-dev html-webpack-plugin
```

Add this to package.json file
```javascript
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server",
    "build": "rimraf dist && webpack --config webpack.prod.config.js --progress --profile --color"
  },
```
Create a file webpack.config.js

```javascript
const path = require('path');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    /* Set port for server */
    devServer: {
        inline:true,
        port: 8008
    },
    /* Use this config always */
    devtool: 'cheap-module-eval-source-map',
    /* Entry point of apllication */
    entry: './src/index.js',
    /* Output the bundle file*/
    output: {
        /* Resolve root directory and create a dist folder*/
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        /* Adding extension to imported files */
        chunkFIlename: '[id].js',
        publicPath: ''
    },
    /* Resolve suport of extensions to add in inport /component/(Name.js) .js */
    resolve: {
        extensions: ['.js', '.jsx']
    },
    module: {
        /* Rules to bundle files exclude node files */
        rules: [
            {
                test: /\.js$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                exclude: /node_modules/,
                /* Webpack firs load css-loader then style-loader so this order must be
                from buttom to up */
                use: [
                    { loader: 'style-loader' },
                    { 
                        loader: 'css-loader',
                        options: {
                            importLoaders: 1,
                            modules: true,
                            localIdentName: '[name]__[local]__[hash:base64:5]'
                        } 
                    },
                    { 
                        loader: 'postcss-loader',
                        options: {
                            ident: 'postcss',
                            plugins: () => [
                               autoprefixer({
                                   browsers: [
                                       "> 1%",
                                       "last 2 versions"
                                   ]
                               }) 
                            ]
                        } 
                    }
                ]
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/,
                loader: 'url-loader?limit=8000&name=images/[name].[ext]'
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: __dirname + '/src/index.html',
            filename: 'index.html',
            inject: 'body'
        })
    ]
};
```

### Babel

```console
npm install --save-dev babel-loader babel-core babel-preset-react babel-preset-env
npm install --save-dev babel-plugin-syntax-dynamic-import
npm install --save-dev babel-preset-stage-2
npm install --save babel-polyfill 
```

Create a folder .babelrc

```javascript
{
    "presets": [
        ["env", {
            "targets": {
                "browsers": [
                    ">1 %",
                    "last 2 versions"
                ]
            },
            "useBuiltIns": "entry"
        }],
        "stage-2",
        "react"
    ],
    "plugins": [
        "syntax-dynamic-import"
    ]
}
```

To run all:
```console
npm start
```

### Optimaise production ./dist folder

To build for production run:
```console
npm run build
```

Create webpack.prod.config.js in root


rimraf give option to delete folders or files
```console
npm install --save-dev rimraf
```

webpack.prod.config.js file:
```javascript
const path = require('path');
const autoprefixer = require('autoprefixer');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

module.exports = {
    devtool: 'cheap-module-source-map',
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
        chunkFilename: '[id].js',
        publicPath: ''
    },
    resolve: {
        extensions: ['.js', '.jsx']
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                exclude: /node_modules/,
                use: [
                    { loader: 'style-loader' },
                    {
                        loader: 'css-loader',
                        options: {
                            importLoaders: 1,
                            modules: true,
                            localIdentName: '[name]__[local]__[hash:base64:5]'
                        }
                    },
                    {
                        loader: 'postcss-loader',
                        options: {
                            ident: 'postcss',
                            plugins: () => [
                                autoprefixer({
                                    browsers: [
                                        "> 1%",
                                        "last 2 versions"
                                    ]
                                })
                            ]
                        }
                    }
                ]
            },
            {
                test: /\.(png|jpe?g|gif|svg)$/,
                loader: 'url-loader?limit=8000&name=images/[name].[ext]'
            }
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: __dirname + '/src/index.html',
            filename: 'index.html',
            inject: 'body'
        }),
        new webpack.optimize.UglifyJsPlugin()
    ]
};
```


### Links

[Webpack Docs:](https://webpack.js.org/concepts/)

[More about Babel:](https://babeljs.io/)