
``vue create 项目名``

```js
Vue CLI v4.5.13
? Please pick a preset: (Use arrow keys)
> book-mall ([Vue 3] dart-sass, babel, router, vuex) 
  Default ([Vue 2] babel, eslint) 
  Default (Vue 3) ([Vue 3] babel, eslint) 
  Manually select features                                 //手动配置
```

```js
  Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: 
 (*) Choose Vue version                               //选择Vue版本
 (*) Babel                                           //将es6转成es5
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support             //渐进式Web应用程序（PWA）支持
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors                          //CSS预处理器，预处理器
>( ) Linter / Formatter                         //格式化程序
 ( ) Unit Testing                              //单元测试
 ( ) E2E Testing                              //端到端（end-to-end）

```

```js
  Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with (Use arrow keys)
> 2.x     //选择Vue版本
  3.x
```

```js
Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) //history模式应该选Y
```

```js
Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): (Use arrow keys)
> Sass/SCSS (with dart-sass)            //保存后编译  选这个不然会报错 
  Sass/SCSS (with node-sass)           //实时编译
  Less
  Stylus
```

```js
Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with node-sass)
? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
> In dedicated config files     // 独立文件放置
  In package.json              // 放package.json里  选这个
```


```js
Vue CLI v4.5.13
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router, Vuex, CSS Pre-processors
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with node-sass)
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N)   //不保存设置N
```

创建vue.config.js配置文件，设置路径

```js
module.exports = {
    configureWebpack: {
        resolve: {
            extensions: [],
            alias: {
                'assets': '@/assets',
                'common': '@/common',
                'components': '@/components',
                'network': '@/network',
                'views': '@/views',
            }
        }
    }
}
```