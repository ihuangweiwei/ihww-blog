title: Grunt学习笔记
date: 2014-12-04 09:39:20
tags:
    -前端构建
    -nodejs
---

##一、安装grunt

    npm install -g grunt-cli

安装grunt-cli并不等于安装了Grunt，`允许你在同一个系统上同时安装多个版本的 Grunt`

##二、task配置实例

**插件安装**

    npm install grunt-contrib-uglify --save-dev


**配置实例**

参考：<https://github.com/ihuangweiwei/aiting>

```
module.exports = function (grunt) {

    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        date: grunt.template.today("yyyy-mm-dd"),
        //压缩js，同样可以设置合并js
        uglify: {
            //参考UglifyJS2
            options: {
                compress: {
                    drop_console: true,
                    warnings: false,
                    join_vars: true,
                    drop_debugger: true
                },
                beautify: {
                    ascii_only: true
                },
                banner: '/*! <%= pkg.name %> <%= date %> */'
            },
            my_target: {
                //Compiling all files in a folder dynamically
                files: [
                    //模拟使用usemin
                    {"dest/js/jquery/j.js": ['src/js/jquery/jquery-2.1.1.js', 'src/js/jplayer/jquery.jplayer.js']},
                    {"dest/js/aiting.js": 'src/js/aiting.js'}
                ]
            }
        },
        //压缩css
        cssmin: {
            options: {
                banner: '/* minified css file */'
            },
            my_target: {
                //Compiling all files in a folder dynamically
                files: [{
                    expand: true,
                    cwd: 'src/css',
                    src: '**/*.css',
                    dest: 'dest/css'
                }]
            }
        },
        copy: {
            options: {
                timestamp: true
            },
            my_target: {
                files: [
                    {
                        expand: true,
                        cwd: 'src/static',
                        src: "**",
                        dest: "dest/static"
                    },
                    {"dest/CNAME": './CNAME'},
                    {"dest/index.html": 'src/index.html'}
                ]
            }
        },
        //清除打包的文件夹
        clean: {
            folder: "dest"
        },
        htmlmin: {
            //https://github.com/kangax/html-minifier#options-quick-reference
            options: {
                removeComments: true,
                collapseWhitespace: true,
                removeEmptyAttributes: true,
                minifyJS: true,
                minifyCSS: true
            },
            my_target: {
                files: {
                    'dest/index.html': 'dest/index.html'
                }
            }
        },
        //一键发布github
        'gh-pages': {
            options: {
                base: 'dest',
                branch: 'gh-pages'
            },
            src: ['**']
        },
        useminPrepare: {
            html: 'dest/index.html'
        },
        usemin: {
            html: 'dest/index.html',
            options: {
                blockReplacements: {
                    //简单做个测试，可以自定义静态文件后缀，避免缓存
                    js: function (block) {
                        return '<script type="text/javascript" src="' + block.dest + '?' + grunt.template.today("yyyymmdd") + '"></script>'
                    }
                }
            }

        }
    })


    grunt.loadNpmTasks('grunt-contrib-uglify');
    grunt.loadNpmTasks('grunt-contrib-cssmin');
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-contrib-htmlmin');
    grunt.loadNpmTasks('grunt-contrib-copy');
    grunt.loadNpmTasks('grunt-gh-pages');
    grunt.loadNpmTasks('grunt-usemin');


    grunt.registerTask("build", ['clean', 'cssmin', 'uglify', 'copy','usemin', 'htmlmin'])
    grunt.registerTask("deploy", ['build', 'gh-pages'])


};
```