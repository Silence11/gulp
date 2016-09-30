## gulp
gulp构建前端项目

###环境安装(都是安装在本地的)
* npm install --save-dev gulp  
* npm install --save-dev gulp-html           //html压缩
* npm install --save-dev gulp-uglify         //js压缩
* npm install --save-dev gulp-notify         //提示信息
* npm install --save-dev gulp-minify-css     //压缩css文件
* npm install --save-dev gulp-imagemin       //图片压缩
* npm install --save-dev imagemin-pngcrush
* npm install --save-dev gulp-rename         //文件重命名
* npm install --save-dev gulp-concat     //文件合并

###在项目的根目录下新建gulpfile.js文件
###配置如下
```
var gulp = require('gulp');
var htmlmin = require('gulp-htmlmin'); //html压缩
var uglify = require('gulp-uglify');//js压缩
var notify = require('gulp-notify');//提示信息
var minifycss = require('gulp-minify-css');//压缩css
var imagemin = require('gulp-imagemin');//图片压缩
var pngcrush = require('imagemin-pngcrush');
//压缩html
gulp.task('html',function(){
    return gulp.src('html/*.html')//配置将要压缩的文件入口
        .pipe(htmlmin({
            collapseWhitespace: true,
            removeComments: true,
            minifyJS: true,  //压缩页面JS
            minifyCSS: true  //压缩页面CSS
        }))
        .pipe(gulp.dest('../dist'))//将压缩的文件输出到此文件夹下面
        .pipe(notify({ message: 'html task ok' }));//打印信息

})
////压缩js文件
gulp.task('js', function() {
    return gulp.src(['build/*.js'])
        .pipe(concat('all.js)) //合并为一个js
        .pipe(gulp.dest('../dist/build'))
        .pipe(rename({suffix:'.min'})//将压缩的js重命名
        .pipe(uglify().on('error', function(e){
            console.log(e);
        }))
        .pipe(gulp.dest('../dist/build'))
        .pipe(notify({ message: 'js task ok' }));
});
//压缩
gulp.task('css', function() {
    return gulp.src(['css/*.css'])
        //.pipe(concat('main.css'))
          .pipe(gulp.dest('../dist/css'))
        //.pipe(rename({ suffix: '.min' }))
        .pipe(minifycss())
        .pipe(gulp.dest('../dist/css'))
        .pipe(notify({ message: 'css task ok' }));
});
// 压缩图片
gulp.task('img', function() {
    return gulp.src('images/*')
        .pipe(imagemin({
            progressive: true,
            svgoPlugins: [{removeViewBox: false}],
            use: [pngcrush()]
        }))
        .pipe(gulp.dest('./dist/images/'))
        .pipe(notify({ message: 'img task ok' }));
});
//执行任务
gulp.task('build', function(){
    gulp.run('js', 'html','css','img');//执行
    //watch 监听变化
    gulp.watch('html/*.html', function(){
        gulp.run('html');
    });
    gulp.watch(['build/*.js'], ['js']);
    gulp.watch(['css/*.css'], ['css']);
    gulp.watch('images/*', ['img']);
});
```
###运行命令
* gulp build


