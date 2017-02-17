##使用gulp进行文件的压缩合并，他是基于node.js的前端构建工具，所以需要先保证电脑上有node.js。
###1.全局安装gulp:   npm install -g gulp
  ![gulp安装](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp001.png?raw=true)  
  ![gulp安装](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp002.png?raw=true)  
###2.项目根目录安装本地gulp：npm install gulp --save-dev
  #### glup的 API 只有四个：
    * task 创建任务，在命令行下使用 gulp 任务名称 来执行任务
    * watch 监听任务
    * src 设置需要处理的文件路径，可以是数组的形式 或者正则表达的形式
     [ test1.js , test2.js ]     或者  /**/*.js
    * dest 设置生成文件的路径，一个任务可以有多个路径，一个可以输出未压缩的版本，一个输出压缩后的版本
###3.gulp本身不具有压缩合并功能，因此需要在项目的根目录安装相关插件：
  npm install gulp-clean-css gulp-uglify gulp-concat gulp-rename --save-dev
  npm install jshint gulp-jshint --save-dev
  
  [参考gulp入门1](https://segmentfault.com/a/1190000002698606)
###4.在项目更目录创建gulpfile.js并配置相关内容：
```ruby
  var gulp = require('gulp'),
    cleanCSS = require('gulp-clean-css'), //css压缩
    concat = require('gulp-concat'),//js合并
    uglify = require('gulp-uglify'),//js压缩
    rename = require('gulp-rename'),//重命名
    jshint=require('gulp-jshint');//js代码检测
    //语法检查
    gulp.task('jshint', function () {
      return gulp.src('js/*.js')//文件位置
            .pipe(jshint())
            .pipe(jshint.reporter('default'));
            });
    //压缩css
    gulp.task('minifycss', function() {
        return gulp.src('css/*.css')    //需要操作的文件
            .pipe(rename({suffix: '.min'}))   //rename压缩后的文件名
            .pipe(cleanCSS({compatibility: 'ie7'}))   //执行压缩
            .pipe(gulp.dest('Css'));   //输出文件夹
    });
    //压缩,合并 js
    gulp.task('minifyjs', function() {
        return gulp.src('js/*.js')      //需要操作的文件
            .pipe(concat('main.js'))    //合并所有js到main.js
            .pipe(gulp.dest('js'))       //输出压缩前的文件到文件夹
            .pipe(rename({suffix: '.min'}))   //rename压缩后的文件名
            .pipe(uglify())    //压缩
            .pipe(gulp.dest('Js'));  //输出压缩后的文件到文件夹
    });
　　//默认命令，在cmd中输入gulp后，执行的就是这个任务(压缩js需要在检查js之后操作)
    gulp.task('default',['jshint'],function() {
        gulp.start('minifycss','minifyjs'); //
});
```
###5.在项目根目录的命令窗口执行gulp命令，查看cmd报错信息，并更正
  使用：
  
  ![gulp安装](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp003.png?raw=true)  
  ![gulp安装](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp004.png?raw=true)  
  ![gulp安装](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp005.png?raw=true)  
###6.使用 npm 卸载插件：npm uninstall <name> [-g] [--save-dev]
删除全部插件：npm uninstall <name1> <name2> ......
借助 rimraf 删除插件：npm install rimraf -g 用法：rimraf node_modules
###7.更新插件：npm update <name> [-g] [--save-dev]
更新全部插件：npm update [--save-dev]
###8.查看帮助：npm help
###9.当前目录已安装插件：npm list
