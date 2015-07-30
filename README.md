# gulp-h5packer

移动H5页面打包工具

为了减少移动端H5页面的请求数量，往往会在发布前将页面依赖的JS, CSS文件嵌入html中，如果一些资源有全局资源包的支持，也会将其替换为CDN地址。

本插件可以根据link, script, img标签中的标记，选择将文件内容嵌入或者替换为CDN地址。

## 用法

首先，在待处理的html文件中加入标记。标记共有两种，一种为：


```html
	<link rel="stylesheet" type="text/css" href="something.css" data-replace="true">
	<script src="something.js" data-replace="true"></script>
```

这样会读取资源文件，把内容嵌入在标签的位置。script标签会删去src和data-replace属性，并在标签内嵌入内容。link标签会在后面产生一个style节点，并嵌入内容。标签本身会被删除。

另外一种引用格式为：

```html
	<link rel="stylesheet" type="text/css" href="local/amui.css" data-replace="amui">
	<script src="zepto.js" data-replace="zepto"></script>
```

在data-replace中写入全局资源包的名称，即可替换为相应的地址。

目前支持以下资源：

```js
{
  amui: 'https://a.alipayobjects.com/amui/native/9.0/amui.css',
  zepto: 'https://a.alipayobjects.com/amui/zepto/1.1.3/zepto.js',
  antBridge: 'https://a.alipayobjects.com/publichome-static/antBridge/antBridge.min.js',
  fastclick: 'https://a.alipayobjects.com/static/fastclick/1.0.6/fastclick.min.js',
  lazyload: 'https://a.alipayobjects.com/static/lazyload/2.0.3/lazyload.min.js'
}
```
具体的列表可以看[h5全局离线资源包](http://ux.alipay-inc.com/index.php/H5%E5%85%A8%E5%B1%80%E7%A6%BB%E7%BA%BF%E8%B5%84%E6%BA%90%E5%8C%85)

设置完HTML后，在gulpfile.js中引用：
```js
gulp.task('pack', function (){
  var replacer = require('@alipay/gulp-h5replacer');
  gulp.src('./build/*/*.html')
    .pipe(replacer())
    .pipe(gulp.dest('./pack'));
});
```

## Todo
1. 将文件引用的图片和无法嵌入的资源文件自动上传到蜻蜓上
2. 增加对网络引用文件的嵌入支持