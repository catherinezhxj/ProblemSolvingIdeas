http://gulpjs.com/plugins/  gulp插件库
http://www.qiqiboy.com/post/61 各种坑

###gulp-load-plugins:自动加载
1、它解决了：
当使用的插件过多，gulpfile.js 头部引入的文件过多，使得gulofile.js 变得非常冗长
不在最初就加载使用的所有插件，而是在使用到的时候再进行加载
2、使用方法：
安装：npm istall --save-dev gulp-load-plugins
引用：
```
   var gulp = require('gulp');
   var plugins = require('gulp-load-plugins')(); //加载后并立即执行
```
如何使用插件：
   plugins.插件名
   例如：plugins.rename 重命名插件、plugins.rubySaaS 解析saas插件
###gulp-rename:重命名
1、使用方法：
安装：npm install --save-dev gulp-rename
使用：rename('文件名')
2、解决的问题：
用 gulp.dest() 方法写入文件时，文件名使用的是 文件流中的文件名，所以就可以使用插件来改变文件流中的文件名。
3、例子：

![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp012.png?raw=true)

关于gulp-rename的更多强大的用法请参考https://www.npmjs.com/package/gulp-rename
###gulp-uglify:压缩js
技巧篇：http://www.cnblogs.com/2050/p/4198792.html
1、压缩多个js文件
var gulp = requite('gulp'),
uglify = require('gulp-uglify');
gulp.task('任务名',function(){
gulp.src(['路径1','路径2']) //多个文件，使用数组形式传入
.pipe(uglify())
.pipe(gulp.dest('压缩后保存路径'));
});
2、匹配符
src/js 的 0 个或多个子文件夹下，除了 test1.js 和 test2.js 的所有 js 文件
！src/js/**/{test1,test2}.js'
###gulp-clean-css:压缩css
安装：npm install --save-dev gulp-minify-css
使用： minifyHtmCss()
注意：gulp-minify-css 已经被弃用，用该包来替换。
###gulp-htmlmin:压缩html
安装：npm install --save-dev gulp-htmlmin
使用： htmlmin(options)
var options = {
removeComments:true, //清除HTML注释
collapseWhitespace:true, //压缩HTML
collapseBooleanAttributes:true, //省略布尔属性的值 <input checked='true'/> ==> <input />
removeEmptAttributes:true, //删除所有空格做属性值 <input id="" /> ==> <input />
removeStyleLinkTypeAttributes: true,//删除<style>和<link>的type="text/css"
minifyJS: true,//压缩页面JS
 minifyCSS: true//压缩页面CSS
ignoreCustomFragments:[正则1,正则2,正则3,正则4] // 压缩过程中匹配成功的部分不用压缩
}
注意：gulp-minify-html 包已经被弃用，使用该包来取代，该包更快、更全面
###gulp-jshint:js代码检查
安装：npm isntall jshint gulp-jshint --save-dev 
使用：jshint()
输出检查结果：jshint.reporter()
###gulp-concat:文件合并
安装：npm isntall --save-dev gulp-concat
使用：concat('文件名') //合并后并命名为此文件名
###gulp-imagemin:压缩 jpg/png/gif等
安装：npm install --save-dev gulp-imagemin

![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp013.png?raw=true)

###gulp-cache:只压缩修改的图片
使用：如果监控到图片改变，
###gulp-livereload:自动刷新
安装：npm isntall --save-dev gulp-livereload
作用：当代码发生变化时自动刷新页面，需要使用谷歌且安装 livereload chrome extension 插件

![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp014.png?raw=true)

###gulp-less/gulp-sass：编译less/sass
安装：npm install --save-dev gulp-less
   npm install --save-dev gulp-sass
使用：less()/sass()
###gulp-changed:只允许改变的文件通过管道
安装：npm install --save-dev gulp-changed
###gulp-filter:筛选 vinyl stream 中的文件
###gulp-sourcemaps:记录代码转换的位置
解决问题：
常见的转换情况：
压缩减小体积
多文件合并，减少HTTP请求数
其他语言编译成 javascript 。最常见的就是 CoffeeScript
以上转换情况，都使得实际运行的代码不同于开发的代码，因此出错时变得很困难。
而这个插件就是解决这个问题：Source map 文件记录了转换后代码的每一个位置，出错时出错工具将直接显示原始代码，而不是转换后的代码。

![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp015.png?raw=true)

###gulp-usemin:
使用：
browser-sync:网页文件变更时自动载入新的网页
使用：他是一款自动化测试辅助工具
