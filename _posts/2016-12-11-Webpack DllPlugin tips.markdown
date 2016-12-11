---
layout: post
title: Webpack DllPlugin打包小技巧
categories:
- Web Developer
---

前端项目构建过程中，为了提高构建效率，用上cache，往往会将第三方库和自己的业务逻辑代码分开打包，在`Webpack`里有两个插件可以完成这项任务，`CommonsChunk`和`DLL & DllReference`

### CommonsChunk

可以将相同的模块提取出来单独打包，进而减小 rebuild 时的性能消耗，但是每次构建时都需要执行一次，显然很浪费时间

### DLL & DllReference

相比于前者，通过前置这些依赖包的构建，来提高真正的 build 和 rebuild 的构建效率。也就是说只要第三方库没有变化，之后的每次build都只需要去打包自己的业务代码。这个思路应该来源于Windows的`动态链接库 (DLL) `

如何使用`DLL & DllReference`在这里不再敖述，网上资料还是比较多的，可以参考<a href="https://segmentfault.com/a/1190000005770042#articleHeader11">https://segmentfault.com/a/1190000005770042#articleHeader11</a>

### 出现的问题

使用`DLL & DllReference`后，第三方库的确前置构建了，但是如何让打包出来的bundle文件在index.html中引用呢？如果output.fileName写死名字，在index.html中也写死，就没有了强缓存，如果output.fileName="[name].[hash].js"，就得找到一个往index.html添加js的办法

### assets-webpack-plugin

> When working with Webpack you might want to generate your bundles with a generated hash in them (for cache busting).
> This plug-in outputs a json file with the paths of the generated assets so you can find them from somewhere else.

有了这个插件，看起来就行得通了，在打包第三库时使用`assets-webpack-plugin`将bundle的文件名输出，保存成json，在打包业务代码时配合`html-webpack-plugin`插件，将bundle添加到index.html中

webpack.dll.config.js

```
const webpack = require('webpack');
const AssetsPlugin = require('assets-webpack-plugin');

module.exports = {
    entry: {
        bundle: [
            'history',
            'react', 
            'react-router', 
            'react-dom', 
            'react-redux', 
            'react-router', 
            'react-router-redux', 
            'redux',
            'redux-thunk'
            ]
    },
    output: {
    	path: '/',
        filename: '[name].[hash].js',
        library: '[name]_library'
    },
    plugins: [
        new webpack.DllPlugin({
            path: '/',      
            name: '[name]_library'
        }),
        new AssetsPlugin({
        	filename: 'bundle-config.json', 
        	path: '/'
        }),
    ]
};
```

webpack.config.js

```
const webpack = require('webpack');
const bundleConfig = require("/bundle-config.json");
module.exports = {
    entry: {
        app: ['./app.tsx']
    },
    output: {
    	path: '/',
        publicPath: '',
        filename: '[name]-[hash].js',
    	chunkFilename: '[name]-[chunkhash].js'
    },
    plugins: [
        new HtmlwebpackPlugin({
            filename: 'index.html',
            chunks: ['app'],
            template: 'app.html',
            bundleName: bundleConfig.bundle.js,
            minify: __DEV__ ? false : {
                collapseWhitespace: true,
                collapseInlineTagWhitespace: true,
                removeRedundantAttributes: true,
                removeEmptyAttributes: true,
                removeScriptTypeAttributes: true,
                removeStyleLinkTypeAttributes: true,
                removeComments: true
            }
        }),
        new webpack.DllReferencePlugin({
            context: '.',
            manifest: require('/bundle-manifest.json') 
        })
    ]
};
```

app.html里需要用到这样的模版 <%= htmlWebpackPlugin.options.bundleName %>

到这里，整个构建过程就比较舒服了，不重复打第三方库的包，也用上了强缓存

更详细的代码可以参考我搭的这个脚手架 <a href="https://github.com/bccsafe/React-WebApp-Boilerplate">https://github.com/bccsafe/React-WebApp-Boilerplate</a>

### 参考资料

* <a href="https://segmentfault.com/a/1190000005770042#articleHeader11">https://segmentfault.com/a/1190000005770042#articleHeader11</a>
* <a href="http://engineering.invisionapp.com/post/optimizing-webpack/">http://engineering.invisionapp.com/post/optimizing-webpack/</a>
* <a href="https://github.com/ampedandwired/html-webpack-plugin/issues/194">https://github.com/ampedandwired/html-webpack-plugin/issues/194</a>
* <a href="https://github.com/kossnocorp/assets-webpack-plugin">https://github.com/kossnocorp/assets-webpack-plugin</a>





