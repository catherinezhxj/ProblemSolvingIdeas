##SRC:
1、此方法用来获取流，保存的不是原始文件，而是 虚拟文件对象流（vinyl files）存储的是原始文件的路径、文件名、文件内容等信息
2、语法：
gulp.src(globs[,options])
globs 类正则表达式的文件匹配模式
options 可选参数，通常情况下不需要使用
3、glob文件匹配模式：
在 gulp 内部使用了 node-glob 模块来实现文件匹配功能。github地址：https://github.com/isaacs/node-glob
* 匹配 0 或多个字符，在一个单一路径中
? 匹配 1 个字符
[...] 匹配 中括号 中出现的 任意一个字符。如果第一个字符是 ! 或 ^ ，则不匹配中括号中的任意一个字符
！（模式1|模式2|模式3）匹配除这三种之外的
? (模式1|模式2|模式3）匹配括号中给定的任一模式 0 次或 1 次
+ (模式1|模式2|模式3） 匹配括号中给定的任一模式至少 1 次
* (模式1|模式2|模式3）匹配括号中给定的任一模式 0 次或多次
@ (模式1|模式2|模式3）匹配括号中给定的任一模式1 次，属于精确匹配
![匹配列表](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp006.png?raw=true)
4、glob文件展开模式：
a{b,c}d ==> abd，acd
a{b,}c ==> abc，ac
a{0..3}d ==> a0d , a1d , a2d , a3d
a{b,c{d,e}f}g ==> abg , acdfg , acefg
a{b,c}d{e,f}g ==> abdeg , abdfg , acdeg , acdfg
5、多种匹配模式使用数组：
gulp.src(['js/*.js' , 'css/*.css' ,' *.html' ])
6、注意：
不能再数组的第一位使用排除模式
gulp.src([*.js , '!b*.js'])  匹配排除以 b 开头的所有 js 文件
gulp.src(['!b*.js',*.js])   这种情况不会排除任何匹配文件

##TASK:
参考：http://www.cnblogs.com/2050/p/4198792.html
1、定义任务
2、语法：
gulp.task( name [, deps ] ,fn)
name ： 任务名
deps：任务依赖。当前任务会在 所有依赖 执行完毕 后才开始执行。可选。
fn：任务函数。当前任务执行的代码。可选。
3、入果有任务依赖，则先执行依赖中的任务。如果依赖中的任务是异步执行的（settimeout/ajax等）
则不会等待依赖完成再去执行当前任务。他遵循 javascript 的事件循环 event loops 机制。会将异步任务放入 事件队列，先去执行 栈中代码，执行完毕后 再去 任务队列 中读取那些异步任务执行。参考：http://www.ruanyifeng.com/blog/2014/10/event-loop.html
![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp007.png?raw=true)
先执行 two 在执行 one

##DEST:
原文章：http://www.cnblogs.com/2050/p/4198792.html
1、此 API 用于 写文件。
2、语法：
gulp.dest ( path [ , options ] )
path 写入文件路径
options 可选参数对象，通常不需要
3、使用流程：
1、通过 gulp.src() 获取要处理的文件流（导入）
2、通过 pipe 方法导入到 gulp 插件中，使用插件处理流（处理）
3、通过 pipe 方法导入到 gulp.desc() 中（导出）
gulp.desc() 是将流中的内容写入到文件中，而传入的是 路径参数，只能用来指定要生成的文件目录，而不指定生成的文件名，而他生成的文件的文件名使用的是导入他的文件流自身的文件名，所以生成的文件名是由导入的文件流决定，即使传入的是带文件名大的路径参数，他会把它当作是目录名来处理。
![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp008.png?raw=true)
4、输出的文件路径，是由 src 的文件名 + dest 的文件路径组合而成的。如果 src 中含有通配符，则 sec 的文件路径是取通配符匹配成功的部分。
![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp009.png?raw=true)
![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp0010.png?raw=true)
5、其实，通配符之前的路径 和 没有通配符情况下文件名前的路径 称为 base 路径，也就是说 生成的文件路径是 dest 中的路径替换掉 base 路径后组成的文件路径。所以，当我们改变 base 路径后，生成的文件路径也就会改变。
![](https://github.com/catherinezhxj/ProblemSolvingIdeas/blob/master/resources/gulp0011.png?raw=true)
##WATCH:
1、 gulp.watch() 用来监视文件变化，当文件变化后，利用它执行相应的任务。
2、语法：
gulp.watch( glob [,opts], tasks)
glop 需要监视的文件的匹配模式
opts 配置对象，可选，一般不用
tasks 文件变化后，要执行的任务
3、gulp.watch('js/**/*.js',[任务1，任务2]);
4、用法2：
gulp.watch(glob [ ,opts,cb])
当监视的文件发生变化的时候，就会调用 callbcak 函数，并且会给他传入一个对象 event ，该对象包含了 type （added 新增,deleted删除,changed改变）变化的类型属性 、 path（发生变化的文件的路径）属性等
gulp.watch('js/**/*.js',function(event){
coonsole.log(event.type);
console.log(event.path);
));
