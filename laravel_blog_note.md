# Laravel 5.1 基础教程

https://www.shiyanlou.com/courses/733

最后一个实验还将通过30分钟搭建一个迷你博客



**Laravel 5.1 中文文档** https://learnku.com/docs/laravel/5.1



## 实验 1 开启 Laravel 之旅以及环境配置


VitrualBox 是一款非常强大的免费虚拟机软件

Vagrant 是一个用于创建和部署虚拟化开发环境的工具，其依赖于 VirtualBox 虚拟机，致力于帮助开发者快速构建一个环境统一的虚拟系统。

Homestead 就是这样一个 Laravel 官方预装的适合 Laravel 开发的 Vagrant box 。


Homestead.yaml 

http://homestead.test

.app 在浏览器中无法打开（隐私设置错误?）



## 实验 2 HelloWorld — 我的第一行代码

| **文件夹** | **介绍**                                                |
| ---------- | ------------------------------------------------------- |
| app        | 网站的业务逻辑代码，例如：控制器/模型/路由等            |
| bootstrap  | 框架启动与自动加载设置相关的文件                        |
| config     | 网站的各种配置文件                                      |
| database   | 数据库操作相关的文件                                    |
| public     | 网站的对外文件夹，入口文件和静态资源（CSS，JS，图片等） |
| resources  | 前端视图文件和原始资源（CSS，JS，图片等）               |
| storage    | 编译后的视图、基于会话、文件缓存和其它框架生成的文件    |
| tests      | 自动化测试文件                                          |
| vendor     | Composer 依赖文件                                       |

除了上述文件夹，根目录下有些文件也比较常用：

| **文件**      | **介绍**                                                  |
| ------------- | --------------------------------------------------------- |
| .env          | 环境配置文件                                              |
| .env.example  | .env 文件的一个示例                                       |
| .gitignore    | git 的设置文件，制定哪些文件会被 git 忽略，不纳入文件管理 |
| composer.json | 网站所需的 composer 扩展包                                |
| composer.lock | 扩展包列表，确保这个网站的副本使用相同版本的扩展包        |
| gulpfile.js   | GULP 配置文件（ GULP 后边会学到）                         |



## 实验 3 初识强大的 Artisan 命令

Artisan 就相当于 Laravel 为我们独家定制的一个命令行工具，提供了很多实用的命令，可以用来快速生成 Laravel 开发中常用的一些文件并完成相关的配置。

**下面列出最常用的一些命令**

| **命令**                    | **说明**         |
| --------------------------- | ---------------- |
| php artisan key:generate    | 生成 App Key     |
| php artisan make:controller | 生成控制器       |
| php artisan make:model      | 生成模型         |
| php artisan make:policy     | 生成授权策略     |
| php artisan make:seeder     | 生成 Seeder 文件 |
| php artisan migrate         | 执行迁移         |

`php artisan route:list` 列出当前工程中的路由列表

| Domain | Method    | URI                      | Name             | Action                                          | Middleware |
| ------ | --------- | ------------------------ | ---------------- | ----------------------------------------------- | ---------- |
|        | GET\|HEAD | /                        |                  | Closure                                         |            |
|        | GET\|HEAD | articles                 | articles.index   | App\Http\Controllers\ArticlesController@index   |            |
|        | POST      | articles                 | articles.store   | App\Http\Controllers\ArticlesController@store   |            |
|        | GET\|HEAD | articles/create          | articles.create  | App\Http\Controllers\ArticlesController@create  |            |
|        | GET\|HEAD | articles/{articles}      | articles.show    | App\Http\Controllers\ArticlesController@show    |            |
|        | PUT       | articles/{articles}      | articles.update  | App\Http\Controllers\ArticlesController@update  |            |
|        | PATCH     | articles/{articles}      |                  | App\Http\Controllers\ArticlesController@update  |            |
|        | DELETE    | articles/{articles}      | articles.destroy | App\Http\Controllers\ArticlesController@destroy |            |
|        | GET\|HEAD | articles/{articles}/edit | articles.edit    | App\Http\Controllers\ArticlesController@edit    |            |

GET 获取资源 

POST 创建资源 

PUT 编辑/更新资源（需提交完整的资源字段）

PATCH 编辑/更新资源（可以提交需要更新的字段） 

DELETE 删除资源 



## 实验 4 路由

### 一、什么是路由呢？

路由系统会对用户输入的 URL 地址 进行解析，然后分配不同的工作

### 二、基本路由

网站的大多数路由都定义在 app/Http/routes.php 文件中，该文件将会被 App\Providers\RouteServiceProvider 类加载。

### 三、路由动作

我们知道，一个 url 请求可能有多种类型，除了常用的 GET，还可能有 POST、PUT、DELETE 等类型的请求。

还可以用 `match` 来同时处理多种类型的请求：

```php
Route::match(['get', 'post'],'/foo', function () {
    // 该路由将匹配 get 和 post 方法的 'foo' url
});
```

甚至，还可以使用 `any` 来同时处理所有类型的请求：

```php
Route::any('/foo', function() {
    // 该路由将匹配 所有 类型的 'foo' url
});
```

### 四、路由参数

#### 1.基础的路由参数

定义多个参数：

app/Http/routes.php

```php
<?php

Route::get('name/{name}/age/{age}', function ($name, $age) {
    return 'I`m '.$name.' ,and I`m '.$age;
});
```

如果访问 `localhost/name/Tom/age/28` 我们可以看到网页返回了对应的信息：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2484timestamp1483006622860.png)

#### 2.可选的路由参数

有时你需要指定可选的路由参数，可以通过在参数后面加上 `?` 来实现。

你可以为该可选参数设定一个默认值，当 url 未传参时，将显示默认值。

```php
<?php

Route::get('hello/{name?}', function ($name = 'Tom') {
    return 'Hello! '.$name;
});
```

这时访问 `localhost/hello` 将返回默认的信息，如下图：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2484timestamp1484284690286.png)

### 五、命名路由

所谓命名路由，就是给路由起个名字，这样我们就可以通过这个名字获取到该条路由的相关信息，也更利于后期维护。

建议在开发过程中给每个路由命名，使用下面两种方式都可以为一个路由命名。

```php
Route::get('foo', ['as' => 'foo', function () {
    //
}]);

Route::get('foo', function() {
    //
})->name('foo');
```

### 六、路由群组

路由群组允许你共用路由属性，例如：中间件、命名空间等，你可以利用路由群组统一为多个路由设置共同属性，而不需在每个路由上都设置一次。共用属性被指定为数组格式，当作 `Route::group` 方法的第一个参数。

### 八、查看路由

我们可以使用 `url('foo')` 函数来生成完整的 URL

我们还可以使用命名路由函数 `route('foo')` 来生成完整的 URL 信息。

### 九、正则表达式限制路由

你可以使用 where 方法来限制参数的格式。where 方法接受参数的名称和正则表达式。

app/Http/routes.php

```php
<?php

Route::get('hello/{name?}', function ($name = 'Tom') {
    return 'Hello! '.$name;
})->where('name','[A-Za-z]+');
```

然后如果访问 `localhost/hello/John` 这样的 url 就会正确显示。

如果访问 `localhost/hello/55` 这样的 url 将会报错，如下图：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2484timestamp1484286280022.png)

同样，也可对多个参数都设置正则验证：

app/Http/routes.php

```php
<?php

Route::get('name/{name}/age/{age}', function ($name, $age) {
    return 'I`m '.$name.' ,and I`m '.$age;
})->where(['name' => '[A-Za-z]+', 'age' => '[0-9]+']);
```

如果你想对所有的路由参数都加上某种限制，需要在 RouteServiceProvider 的 boot 方法里定义这些限制。

```php
/**
 * 定义你的路由模型绑定，模式过滤器等。
 *
 * @param  \Illuminate\Routing\Router  $router
 * @return void
 */
public function boot(Router $router)
{
    $router->pattern('id', '[0-9]+');

    parent::boot($router);
}
```



## 实验 5 控制器

### 二、基础控制器

控制器一般存放在 app/Http/Controllers 目录下

先来简单感受一下控制器文件的结构：

```php
<?php

namespace App\Http\Controllers;

use App\User;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * 显示指定用户的个人数据。
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
```

所有 Laravel 控制器都应继承基础控制器类，它包含在 Laravel 的默认安装中。该基础类提供了一些便捷的方法，例如 middleware 方法（middleware 是中间件 后边会讲解），该方法可以用来将中间件附加在控制器行为上。

然后你就可以通过一个路由把请求引入到这个控制器文件来，像下面这样：

```php
Route::get('user/{id}', 'UserController@show');
```

现在，当请求和此特定路由的 URI 相匹配时，UserController 类的 show 方法就会被运行。当然，路由的参数也会被传递至该方法。

现在，让我们动手实践来感受一下控制器。

首先用 artisan 命令创建一个新的控制器，打开命令行，进入代码根目录：

然后运行 artisan 命令生成控制器：

```
php artisan make:controller UserController
```

然后转到 app/Http/Controllers 目录下，可以看到刚刚创建的 UserController.php。

Laravel 为我们生成了一些默认的代码，仔细观察可以发现是 7 个空方法，分别是

`index()` `create()` `store()` `show()` `edit()` `update()` `destroy()`

### 三、控制器的命名空间

控制器文件中有这么一行代码：

```
namespace App\Http\Controllers;
```

这行代码就是说明了此控制器的命名空间。

这个目录也是控制器的默认目录，所以我们在 routes.php 文件中引入的时候可以直接使用

```
Route::get('/user/name', 'UserController@name');
```

现在假如你在 App\Http\Controller 目录下新建了一个 User 文件夹来存放 UserControllser.php。

你的 routes.php 文件中就需要这么写：

```
Route::get('/user/name', 'User\UserController@name');
```

相应的，控制器中的命名空间也要改变：

```
namespace App\Http\Controllers\User;
```

### 四、控制器的依赖注入

细心的你肯定发现，控制器中还有几行神奇的代码：

```
use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;
```

这几行代码说明了该控制器的依赖注入，简单来说，依赖注入就是将该控制器用到的依赖添加进来。



## 实验 6 中间件

### 一、什么是中间件?

我们知道一个请求可以通过路由分配到某个控制器上然后进行处理，如果我们想对请求加一个限制，只允许某些请求能够到达控制器，而过滤掉我们不想要的请求，这时候就可以使用 Laravel 的中间件。

Laravel 框架已经内置了一些中间件，包括维护、身份验证、CSRF 保护，等等。所有的中间件都放在 app/Http/Middleware 目录内。

除了 Laravel 内置的中间件以外，我们也可以根据自己的需要创建中间件。

### 二、创建中间件

下面我们通过一个简单的例子来体会一下 midddleware 的工作过程。

思路如下：

1. 创建一个带参路由，用来产生一个请求，并且附带一个 age 参数；
2. 创建一个简单的中间件，用来过滤掉 age > 25 的用户，来实现一个简单的中间件过滤；
3. 未通过中间件的请求将被重定向到主页；
4. 通过中间件的的请求将达到指定的控制器，实现相应动作。

首先创建带参路由：

app/Http/routes.php

```php
<?php

Route::get('/', function () {
    return view('welcome');
});

Route::get('/young/{age}','UserController@young');
```

然后创建中间件,我们使用 artisan 来创建：

```
php artisan make:middleware YoungMiddleware
```

此命令将会在 `app/Http/Middleware` 目录内创建一个名称为 YoungMiddleware 的中间件。

在这个中间件内我们只允许请求的年龄 age 变量小于 25 时才能访问路由，否则，我们会将用户重定向到首页。

加入我们的过滤代码：

app/Http/Middleware/YoungMiddleware.php

```php
<?php

namespace App\Http\Middleware;

use Closure;

class YoungMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        // 如果传入的参数 age 大于25，重定向到'/'
        if ($request->age > 25) {
            return redirect('/');
        }
        return $next($request);
    }
}
```

### 三、注册并添加中间件

中间件创建好后，还需要注册并添加到相应的路由上，才能工作。

注册中间件：

app/Http/Kernel.php

如果你希望每个 HTTP 请求都经过一个中间件，只要将该中间件的类加入到 app/Http/Kernel.php 的 $middleware 属性清单列表中。

如果你要指派中间件给特定路由，就要将该中间件的类加入到 app/Http/Kernel.php 的 $routeMiddleware 属性清单列表中，并给这个中间件起一个名字。

因为我们这里只需要对我们本节实验开始创建的路由添加这个中间件，所以我们修改代码如下：

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    /**
     * The application's global HTTP middleware stack.
     *
     * @var array
     */
    protected $middleware = [
        \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,

        //如果需要全局中间件在这里注册
    ];

    /**
     * The application's route middleware.
     *
     * @var array
     */
    protected $routeMiddleware = [
        'auth' => \App\Http\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        //在这里添加了一行
        'young' => \App\Http\Middleware\YoungMiddleware::class,
    ];
}
```

注册完成后，我们回到`routes.php`文件中为路由添加中间件。

app/Http/routes.php

```php
<?php

Route::get('/young/{age}', 'UserController@young')->middleware('young');
```

至此，一个简单的中间件就完成了！

此时，根据我们之前的思路：

- age > 25 的路由如 `网址/young/56` 应该会被重定向到 `'/'`
- age <= 25 的路由如 `网址/young/18` 应该会通过中间件，到达控制器`UserController@young`

最后，完成控制器部分的代码。

使用 artisan 命令创建一个控制器：

```
php artisan make:controller UserController  --plain
```

> 添加 --plain 后缀可以不生成默认的空方法。

打开`app/Http/Controllers/UserController.php`，添加`young()`方法：

app/Http/Controllers/UserController.php

```php 
<?php

class UserController extends Controller
{
    public function young(){
        return 'I`m young!';
    }
}
```

至此，所有的代码都完成了，打开浏览器测试一下效果。

访问 `localhost/young/18` 成功显示，如下图：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2498timestamp1483600610325.png)

访问 `localhost/young/56` 重定向到了 `'/'` 



## 实验 7 视图

### 二、视图基础

视图文件存放在 `resources/views` 目录下

我们可以像这样使用全局的辅助函数 view 来返回一个视图:

app/Http/routes.php

```php
Route::get('/', function ()    {
    return view('greeting', ['name' => 'James']);
});
```

`view()` 辅助函数的第一个参数会对应到 resources/views 目录内视图文件的名称。

`view()` 辅助函数的第二个参数是一个能够在视图内取用的数据数组。

视图文件也可以保存在 `resources/views` 目录下的子文件夹内，比如我们打开 `resources/views` 这个目录可以看到有一个 `errors` 文件夹，里边有一个 `503.blade.php` 文件 这个文件是 laravel 为我们生成的一个用来显示错误的页面

我们尝试给视图文件传一个参数,打开 `routes.php` 文件，修改代码：

app/Http/routes.php

```php
<?php

Route::get('errors', function () {
    return view('errors.503', ['message' => '503 ERROR']);
});
```

相应的，打开 `resources/views/errors/503.blade.php`，修改代码：

resources/views/errors/503.blade.php

```php
<!DOCTYPE html>
<html>
    <body>
        <div class="container">
            <div class="content">
                <div class="title">{{ $message }}</div>
            </div>
        </div>
    </body>
</html>
```

> 其中 {{ $message }} 是 blade 模板语法，意思是输出 $message 这个变量，下次实验会专门学习 blade 模板引擎。

然后访问 `localhost/errors` 可以看到参数成功被传到了视图：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2501timestamp1483608628183.png)



### 三、创建一个视图



## 实验 8 Blade 模板

### 一、什么是Blade模版？

Blade 视图文件使用 .blade.php 扩展名，一般被存放在 resources/views 目录。

### 二、模板继承

在 `resources/views` 目录下新建一个目录 `layouts` 然后在 `resources/views/layouts` 目录下新建一个视图文件 `app.blade.php`。

打开这个 `app.blade.php` 加入如下代码：

resources/views/layouts/app.blade.php

```php+HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Myweb</title>
  </head>
  <body>    
    @yield('content')
  </body>
</html>
```

然后修改 `home.blade.php` 和 `about.blade.php`：

resources/views/home.blade.php

```php
@extends('layouts.app')

@section('content')
<h1>Home</h1>
<p>Welcome to My web</p>
@endsection
```

resources/views/about.blade.php

```php
@extends('layouts.app')

@section('content')
<h1>About</h1>
<p>This is my first Laravel web</p>
<p>Author: SadCreeper</p>
<!-- 可以更改为你自己的信息 -->
@endsection
```

其中顶部代码：

```
@extends('layouts.app')
```

就是表示当前视图继承自 `layouts` 文件夹下的 `app.blade.php` 视图。

然后用下面这种方式就可以把 `section('content')` 中包含的内容放到被继承的视图 `app.blade.php` 中的 `yield('content')` 部分。

```
@section('content')
//
@endsection
```

使用模板继承后，你如果想对网站进行一些基础的公用代码的修改时，就可以修改 `app.blade.php`。

下面我们给这个基础视图添加一个非常简单的顶部导航栏和底部信息栏。

resources/views/layouts/app.blade.php

```php
<!DOCTYPE html>
<html>
  <head>
    <title>Myweb</title>
  </head>
  <body style="margin:0;padding:0;">
      <!-- header -->   
      <div style="padding:10px 50px 10px 50px;border-bottom: 1px solid #eeeeee;">
          <div style="display:inline-block;">
              <a href="{{ route('home') }}"><h2>Myweb</h2></a>
          </div>

          <div style="display:inline-block;margin-left:20px;">
              <a href="{{ route('about') }}">about</a>
          </div>
      </div> 

      <div style="text-align:center;">
          @yield('content')
      </div>


    <!-- footer -->
      <div style="padding:10px 50px 10px 50px;background-color:gray;">
          <p>contact me : 1234567</p>
      </div> 
  </body>
</html>
```

此时我们访问 `localhost` 和 `localhost/about` 都可以看到完整的包含导航栏和底部信息栏的视图。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2502timestamp1484540621164.png)

### 二、引入子视图

你可以使用 Blade 的 @include 命令来引入一个已存在的视图，所有在父视图的可用变量在被引入的视图中都是可用的。

尽管被引入的视图会继承父视图中的所有数据，你也可以通过传递额外的数组数据至被引入的页面：

```
@include('view.name', ['some' => 'data'])
```

下面我们创建一个子视图，用来显示网站作者信息，你可以在网站上任何想显示作者信息的地方引用这个视图。

在 `resources/views` 目录下新建一个文件夹 `shared` 然后在 `resources/views/shared` 目录下新建一个文件 `author.blade.php` 打开该文件。

resources/views/shared/author.blade.php

```php+HTML
<div style="width:200px;margin:20px auto 20px auto;padding:20px;border:1px solid black;">
    <h3>Author</h3>
    <p>name : SadCreeper</p>
    <p>age : 22</p>
    <p>Tel : 150-XXXX-XXXX</p>
</div>
```

然后在 `resources/views/about.blade.php` 中引用该子视图：

resources/views/about.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
<h1>About</h1>
<p>This is my first Laravel web</p>

@include('shared.author')

@endsection
```

然后访问`localhost/about` 看下效果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2502timestamp1484542266530.png)

此时如果你想在任何地方显示这个子视图，只需要在相应位置加入下面一行代码即可~

```
@include('shared.author')
```



### 三、数据显示

你可以使用 「中括号」 包住变量以显示传递至 Blade 视图的数据。如假设你有下面这样一个路由：

```php
Route::get('greeting', function () {
    return view('welcome', ['name' => 'Samantha']);
});
```

你在视图文件中可以像这样显示 name 变量的内容：

```
Hello, {{ $name }}.
```

在默认情况下，Blade 模板中的 {{ }} 表达式将会自动调用 PHP htmlentities 函数来转义数据以避免 XSS 的攻击。如果你不想你的数据被转义，你可以使用下面的语法：

```
Hello, {!! $name !!}.
```

### 四、控制输出

### 1.IF

你可以通过 `@if` , `@elseif` , `@else` 及 `@endif` 指令构建 if 表达式，这些命令的功能等同于在 PHP 中的语法：

```php
@if (count($records) === 1)
    我有一条记录！
@elseif (count($records) > 1)
    我有多条记录！
@else
    我没有任何记录！
@endif
```

为了方便，Blade 也提供了一个 @unless 命令：

```php
@unless (Auth::check())
    你尚未登录。
@endunless
```

### 2.循环

```php+HTML
@for ($i = 0; $i < 10; $i++)
    目前的值为 {{ $i }}
@endfor

@foreach ($users as $user)
    <p>此用户为 {{ $user->id }}</p>
@endforeach

@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>没有用户</p>
@endforelse

@while (true)
    <p>我永远都在跑循环。</p>
@endwhile
```

当使用循环时，你可能也需要一些结束循环或者跳出当前循环的命令：

```php+HTML
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{{ $user->name }}</li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
```

或者也可以简写：

```php+HTML
@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{{ $user->name }}</li>

    @break($user->number == 5)
@endforeach
```

当循环进行时，你可以使用 循环变量 `$loop` 来获取循环中有价值的信息

| **属性**         | **描述**                                              |
| ---------------- | ----------------------------------------------------- |
| $loop->index     | 当前循环所迭代的索引，起始为 0。                      |
| $loop->iteration | 当前迭代数，起始为 1。                                |
| $loop->remaining | 循环中迭代剩余的数量。                                |
| $loop->count     | 被迭代项的总数量。                                    |
| $loop->first     | 当前迭代是否是循环中的首次迭代。                      |
| $loop->last      | 当前迭代是否是循环中的最后一次迭代。                  |
| $loop->depth     | 当前循环的嵌套深度。                                  |
| $loop->parent    | 当在嵌套的循环内时，可以访问到父循环中的 $loop 变量。 |

### 五、注释

Blade 也允许在页面中定义注释，然而，跟 HTML 的注释不同的是，Blade 注释不会被包含在应用程序返回的 HTML 内：

```php
{{-- 此注释将不会出现在渲染后的 HTML --}}
```



## 实验 9 数据库 & 数据库迁移

### 一、数据库简介

Laravel 支持四种类型的数据库：

- MySQL
- Postgres
- SQLite
- SQL Server

> 本系列教程选用了 mysql

Laravel 应用程序的数据库配置文件放置在 config/database.php 文件中。

### 二、数据库配置

mysql

.env

### 三、数据库迁移

迁移文件存放在 `database/migrations` 文件夹内

一个迁移类会包含两个方法：`up()` 和 `down()` ：

- `up()` 方法可为数据库添加新的数据表、字段或索引，
- `down()` 方法则是 up 方法的逆操作。

你可以在这两个方法中使用 Laravel 数据库结构构造器来创建以及修改数据表。

比如上面创建 `users` 的代码：

```php
Schema::create('users', function (Blueprint $table) {
            $table->increments('id'); //创建递增字段‘id’
            $table->string('name'); //创建字符串字段‘name’
            $table->string('email')->unique(); //创建唯一字符串字段‘email’
            $table->string('password', 60); //创建字符串字段‘password’ 最大字符数60
            $table->rememberToken(); //创建记住密码字段
            $table->timestamps(); //创建时间戳
        });
```

### 四、运行迁移

一旦你写好了迁移文件，就可以通过一行命令来运行迁移。

首先进入项目位置：

运行迁移：

```
php artisan migrate
```

### 五、迁移回滚

进入项目代码，执行回滚：

```
php artisan migrate:rollback
```

### 六、生成迁移

在代码目录下使用 artisan 生成迁移：

```
php artisan make:migration create_articles_table
```

可以看到 `database/migrations` 目录下生成了对应的迁移文件

修改后执行迁移就可以对数据库进行想要的操作了。



## 实验 10 模型 & Eloquent

### 一、Eloquent简介

Laravel 的 Eloquent ORM 提供了漂亮、简洁的 ActiveRecord 实现来和数据库进行交互。

每个数据库表都有一个对应的「模型」可用来跟数据表进行交互。你可以通过模型查询数据表内的数据，以及将记录添加到数据表中。

### 二、定义模型

模型文件默认放在 `app` 目录下，打开该目录，可以看到 Laravel 默认生成的用户模型 `user.php` ，打开这个文件。

user.php

```php
<?php

namespace App;

use Illuminate\Auth\Authenticatable;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Auth\Passwords\CanResetPassword;
use Illuminate\Contracts\Auth\Authenticatable as AuthenticatableContract;
use Illuminate\Contracts\Auth\CanResetPassword as CanResetPasswordContract;

class User extends Model implements AuthenticatableContract, CanResetPasswordContract
{
    use Authenticatable, CanResetPassword;

    /**
     * The database table used by the model.
     *
     * @var string
     */
    protected $table = 'users';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['name', 'email', 'password'];

    /**
     * The attributes excluded from the model's JSON form.
     *
     * @var array
     */
    protected $hidden = ['password', 'remember_token'];
}
```

`protected $table = 'users';` 设置了数据表的名称。

`protected $fillable = ['name', 'email', 'password'];` 设置了可以更新的字段。

`protected $hidden = ['password', 'remember_token'];` 设置了哪些字段会被隐藏。

### 三、创建模型

使用 artisan 创建一个 Article 模型：

```
php artisan make:model Article
```

打开 `app` 目录即可看到我们创建的 Article 模型文件。

### 四、数据读取

一旦你创建并关联了一个模型到数据表上，那么你就可以从数据库中获取数据。

可以把每个 Eloquent 模型想像成强大的**查询构造器**，它让你可以流畅地查询与模型关联的数据表。

比如下面这段代码可以取出 `users` 数据表中的所有数据。

```php
<?php

use App\User;

$users = App\User::all();

foreach ($users as $user) {
    echo $user->name;
}
```

或者用下面这段代码来对数据进行筛选，来选出 `users` 表中 `active` 字段为 1，的数据并且按照 `age` 倒序排列，然后取出前十条。

```php
$users = App\User::where('active', 1)
               ->orderBy('age', 'desc')
               ->take(10)
               ->get();
```

当然了，你也可以通过 `find()` 和 `first()` 方法来取回单条记录。

```php
// 通过主键取回一个模型...
$flight = App\Flight::find(1);

// 取回符合查询限制的第一个模型 ...
$flight = App\Flight::where('active', 1)->first();
```

你也可以用主键的集合为参数调用 find 方法，它将返回符合条件的集合：

```php
$flights = App\Flight::find([1, 2, 3]);
```

有时你可能希望在找不到数据时抛出一个异常，这在路由或是控制器内特别有用。

`findOrFail()` 以及 `firstOrFail()` 方法会取回查询的第一个结果。如果没有找到相应结果，则会抛出一个异常：

```php
$model = App\Flight::findOrFail(1);

$model = App\Flight::where('legs', '>', 100)->firstOrFail();
```

### 五、数据添加

要在数据库中创建一条新记录，只需创建一个新模型实例，并在模型上设置属性和调用 save 方法即可：

```php
<?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class FlightController extends Controller
{
    /**
     * 创建一个新的用户实例。
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // 验证请求...

        $user = new User;

        $user->name = $request->name;

        $user->save();
    }
}
```

在这个例子中，我们把来自 HTTP 请求中的 `name` 参数简单地指定给 `App\User` 模型实例的 `name` 属性。

当我们调用 `save()` 方法，就会添加一条记录到数据库中。

当 `save()` 方法被调用时，`created_at` 以及 `updated_at` 时间戳将会被自动设置，因此我们不需要去手动设置它们。

### 六、数据更新

`save()` 方法也可以用于更新数据库中已经存在的模型。

要更新模型，则须先取回模型，再设置任何你希望更新的属性，接着调用 `save()` 方法就可以了。

```php 
$user = App\User::find(1);

$user->name = 'New User Name';

$user->save();
```

### 七、数据删除

在模型实例上调用 `delete()` 方法来删除数据：

```php
$user = App\User::find(1);

$user->delete();
```

在上面的例子中，我们在调用 delete 方法之前会先从数据库中取回了这个模型。

我们也可以不用取回直接删除它

- 直接删除，使用 `destroy()` 方法：

```php
App\User::destroy(1);

App\User::destroy([1, 2, 3]);

App\User::destroy(1, 2, 3);
```

- 通过查询来删除模型

当然了，你也可以删除某些满足条件的数据，例如我们删除所有被标为不活跃的用户：

```php
$deletedRows = App\User::where('active', 0)->delete();
```



### 八、Tinker

Laravel 内置了一个小工具可以用来快速调试数据库的数据 `php artisan tinker`。

Laravel artisan 的 tinker 是一个 REPL (read-eval-print-loop)，REPL 是指 交互式命令行界面，它可以让你输入一段代码去执行，并把执行结果直接打印到命令行界面里。

#### 3.使用 artisan 打开 tinker

```
php artisan tinker
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2510timestamp1484556043842.png)

#### 4.添加一个用户

```
$user = new App\User;
$user -> name = 'SadCreeper';
$user -> email = '12345@qq.com';
$user -> password = 'password';
$user -> save();
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2510timestamp1484556464166.png)

#### 5.查看用户信息

```
App\User::find(1);
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2510timestamp1484556553310.png)

#### 6.更新用户信息

```
$user = App\User::find(1);

$user->name = 'HappyCreeper';

$user->save();
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2510timestamp1484557177845.png)

#### 7.删除用户信息

```
App\User::destroy(1);
App\User::find(1);
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2510timestamp1484557268266.png)





## 实验 11 30分钟搭建迷你博客

### 二、设计与思路

在开始写第一行代码之前，一定要尽量从头到尾将我们要做的产品设计好，避免写完又改，多写不必要的代码。

- 需求分析：我们的迷你博客应该至少包含：新增/编辑/查看/删除文章，以及文章列表展示功能。
- 数据库分析：基于这个功能，我们只需要一张 Articles 数据表来存放文章即可。
- 页面结构分析：应该使用模板继承建立一张基础模板包含：导航栏/底部信息栏/作者信息等。

### 三、创建路由

完成这个博客大概需要以下几条路由：

| **路由**         | **功能**                         |
| ---------------- | -------------------------------- |
| 文章列表页面路由 | 返回文章列表页面                 |
| 新增文章页面路由 | 返回新增文章页面                 |
| 文章保存功能路由 | 将文章保存到数据库               |
| 查看文章页面路由 | 返回文章详情页面                 |
| 编辑文章页面路由 | 返回编辑文章页面                 |
| 编辑文章功能路由 | 将文章取出更新后重新保存到数据库 |
| 删除文章功能路由 | 将文章从数据库删除               |

可以看到几乎全部是对文章的数据操作路由，针对这种情况，Laravel 提供了非常方便的办法：RESTful 资源控制器和路由。

打开`routes.php`加入如下代码：

```
Route::resource('articles', 'ArticlesController');
```

只需要上面这样一行代码，就相当于创建了如下7条路由，且都是命名路由，我们可以使用类似`route('articles.show')` 这样的用法。

```php
Route::get('/articles', 'ArticlesController@index')->name('articles.index');
Route::get('/articles/{id}', 'ArticlesController@show')->name('articles.show');
Route::get('/articles/create', 'ArticlesController@create')->name('articles.create');
Route::post('/articles', 'ArticlesController@store')->name('articles.store');
Route::get('/articles/{id}/edit', 'ArticlesController@edit')->name('articles.edit');
Route::patch('/articles/{id}', 'ArticlesController@update')->name('articles.update');
Route::delete('/articles/{id}', 'ArticlesController@destroy')->name('articles.destroy');
```

app/Http/routes.php

```php
<?php

Route::get('/', function () {
    return view('welcome');
});

Route::resource('articles', 'ArticlesController');
```

### 四、创建控制器

利用 artisan 创建一个文章控制器：

```
php artisan make:controller ArticlesController
```

### 五、创建基础视图

在 `resources/views` 目录下新建一个文件夹 `layouts`， 然后在 `resources/views/layouts` 目录下新建一个文件 `app.blade.php`。

resources/views/layouts/app.blade.php

```php+HTML
<!DOCTYPE html>
<html>
  <head>
    <title>Myweb</title>

    <style type="text/css">
        body{margin: 0;padding: 0;background-color: #DEDEDE;}
        a{text-decoration:none;}
        .header{padding:10px 50px 10px 50px;border-bottom: 1px solid #eeeeee;}
        .header>.logo{display:inline-block;}
        .header>.menu{display:inline-block;margin-left:20px;}
        .content{}
        .left{background-color: white;margin: 25px 300px 25px 25px;padding: 25px;box-shadow: 1px 1px 2px 1px #848484;}
        .right{background-color: white;width: 200px;margin: 25px;padding: 25px;box-shadow: 1px 1px 2px 1px #848484;position: absolute;top: 92px;right: 0;}
        .footer{padding:10px 50px 10px 50px;background-color:gray;}
    </style>
  </head>

  <body>
      <!-- header -->   
      <div class="header">
          <div class="logo">
              <a href="#"><h2>Myweb</h2></a>
          </div>

          <div class="menu">
              <a href="{{ route('articles.index') }}">Articles</a>
          </div>
      </div> 

      <div class="content">
          <div class="left">
              @yield('content')
          </div>

          <div class="right">

          </div>    
      </div>


    <!-- footer -->
      <div class="footer">
          <p>contact me : 1234567</p>
      </div> 
  </body>
</html>
```

接下来的所有视图我们都将继承这个视图

### 六、新建文章表单

我们首先创建一个 文章列表展示页，作为我们的迷你博客首页。

在 `resources/views` 目录下新建一个文件夹 `articles` ，然后在 `resources/views/articles` 目录下新建一个文件 `index.blade.php`。

resources/views/articles/index.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
    <a href="{{ route('articles.create') }}" style="padding:5px;border:1px dashed gray;">
        + New Article
    </a>
@endsection
```

如果这时你点击新建文章，你会发现跳到了空白页，因为现在其实是跳到了 `ArticlesController` 的 `create()` 方法里，而这个方法现在是空白的，所以就出现了空白页，我们打开`ArticlesController.php` 来补全这个方法。

app/Http/Controllers/ArticlesController.php

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

class ArticlesController extends Controller
{
    .
    .
    .
    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        return view('articles.create');
    }
```

相应的，在`resources/views/articles` 目录下创建一个视图`create.blade.php`。

resources/views/articles/create.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
    <form action="{{ route('articles.store') }}" method="post">
        {{ csrf_field() }}
        <label>Title</label>
        <input type="text" name="title" style="width:100%;" value="{{ old('title') }}">
        <label>Content</label>
        <textarea name="content" rows="10" style="width:100%;">{{ old('content') }}</textarea>

        <button type="submit">OK</button>
    </form>
@endsection
```

> 注意上面代码中的 `{{ csrf_field() }}` ，这是 Laravel 的 CSRF 保护机制，你应该为你的每个表单添加这样一行代码，以使你的网站不受到跨网站请求伪造 攻击，详情参考[官方文档](https://laravel-china.org/docs/5.1/routing#csrf-protection)。

此时我们访问`localhost/articles` 点击新建文章就可以看到新建文章的表单了。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2512timestamp1484640982255.png)

### 七、文章存储

要实现文章存储，首先要配置数据库，创建数据表，创建模型，然后再完成存储逻辑代码。

#### 1.配置数据库

#### 2.创建数据表

利用 artisan 命令生成迁移：

```
php artisan make:migration create_articles_table
```

然后打开迁移文件：

database/migrations/XXXX_creat_articles_table.php

```php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateArticlesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->longText('content')->nullable();
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('articles');
    }
}
```

我们创建了一张 `articles` 表，包含递增的 `id` 字段，字符串`title`字段，长文本`content`字段，和时间戳。

执行数据库迁移：

```
php artisan migrate
```

出现以下提示，创建成功！

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2512timestamp1484641715030.png)

#### 3.创建模型

利用 artisan 命令创建模型：

```
php artisan make:model Article
```

打开模型文件，输入以下代码：

app/Article.php

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Article extends Model
{
    protected $table = 'articles';

    protected $fillable = [
        'title', 'content',
    ];
}
```

指定了数据表的名称和可写字段。

#### 4.存储逻辑代码

打开 `ArticlesController.php` 控制器，找到 `store()` 方法。

app/Http/Controllers/ArticlesController.php

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use App\Article;//  <---------注意这里

class ArticlesController extends Controller
{
    .
    .
    .

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $this->validate($request, [
            'title' => 'required|max:50',
        ]);

        $article = Article::create([
            'title' => $request->title,
            'content' => $request->content,
        ]);

        return redirect()->route('articles.index');
    }
```

因为这段代码中用到了Eloquent 模型的构造器用法，所以需要引入我们之前创建的模型。

```
use App\Article;
```

至此，新增文章功能就完成了，打开浏览器，访问 `localhost/articles` 点击新建文章，完成表单，点击添加。

### 八、文章列表展示

完成了添加文章功能后，就可以实现我们的文章列表展示页了。

打开 `ArticlesController.php` 找到 `index()` 方法，添加代码如下：

app/Http/Controllers/ArticlesController.php

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use App\Article;

class ArticlesController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $articles = Article::orderBy('created_at','desc')->get();

        return view('articles.index', compact('articles'));
    }
    .
    .
    .
```

取出了全部的文章并根据 `created_at` （创建时间）倒序排列，然后传给视图。

现在我们就可以在视图文件中读取全部的文章并显示了。

转到文章列表展示页：

resoureces/views/articles/index.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
    <a href="{{ route('articles.create') }}" style="padding:5px;border:1px dashed gray;">
        + New Article
    </a>

    @foreach ($articles as $article)
    <div style="border:1px solid gray;margin-top:20px;padding:20px;">
        <h2>{{ $article->title }}</h2>
        <p>{{ $article->content }}</p>
    </div>
    @endforeach
@endsection
```

### 九、编辑文章表单

首先我们在文章列表页的每个文章上添加一个编辑按钮：

resoureces/views/articles/index.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
    <a href="{{ route('articles.create') }}" style="padding:5px;border:1px dashed gray;">
        + New Article
    </a>

    @foreach ($articles as $article)
    <div style="border:1px solid gray;margin-top:20px;padding:20px;">
        <h2>{{ $article->title }}</h2>
        <p>{{ $article->content }}</p>
        <a href="{{ route('articles.edit', $article->id ) }}">Edit</a>
    </div>
    @endforeach
@endsection
```

然后打开 `ArticlesController.php` 添加相应的逻辑代码：

app/Http/Controllers/ArticlesController.php

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use App\Article;

class ArticlesController extends Controller
{

    .
    .
    .
    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        $article = Article::findOrFail($id);
        return view('articles.edit', compact('article'));
    }
    .
    .
    .
```

然后创建对应视图文件：

在 `resources/views/articles` 目录下新建一个文件 `edit.blade.php` 。

resources/views/articles/edit.blade.php

```php+HTML
@extends('layouts.app')

@section('content')
    <form action="{{ route('articles.update', $article->id) }}" method="post">
        {{ csrf_field() }}
        {{ method_field('PATCH') }}
        <label>Title</label>
        <input type="text" name="title" style="width:100%;" value="{{ $article->title }}">
        <label>Content</label>
        <textarea name="content" rows="10" style="width:100%;">{{ $article->content }}</textarea>

        <button type="submit">OK</button>
    </form>
@endsection
```

> 注意这段代码中的 `{{ method_field('PATCH') }}`，这是跨站方法伪造，HTML 表单没有支持 PUT、PATCH 或 DELETE 动作。所以在从 HTML 表单中调用被定义的 PUT、PATCH 或 DELETE 路由时，你将需要在表单中增加隐藏的 _method 字段来伪造该方法，详情参考 [官方文档](https://laravel-china.org/docs/5.1/routing#form-method-spoofing)。

这个表单将会把数据传到 `update()` 方法中，所以接下来我们补全 `update()` 方法。

app/Http/Controllers/ArticlesController.php

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use App\Article;

class ArticlesController extends Controller
{

    .
    .
    .
    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $this->validate($request, [
            'title' => 'required|max:50',
        ]);

        $article = Article::findOrFail($id);
        $article->update([
            'title' => $request->title,
            'content' => $request->content,
        ]);

        //return back();
        return redirect()->route('articles.index');

    }
    .
    .
    .
```

到此，编辑文章功能就完成了，点击编辑后将出现编辑表单，输入新的内容后点击 “OK” 就可以成功更新数据。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2512timestamp1484716165918.png)

### 十、删除文章

首先我们在文章列表页的每个文章上添加一个删除按钮：

resoureces/views/articles/index.blade.php

然后打开 `ArticlesController.php` 添加相应的逻辑代码：

app/Http/Controllers/ArticlesController.php

```php 
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

use App\Http\Requests;
use App\Http\Controllers\Controller;

use App\Article;

class ArticlesController extends Controller
{

    .
    .
    .
    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        $article = Article::findOrFail($id);
        $article->delete();
        return back();
    }
```

删除功能这样就做好了，现在访问文章列表展示页 `localhost/articles` 点击删除按钮，可以看到文章被删除。

### 十一、作者信息

最后，让我们添加作者信息。

打开基础视图：

resources/views/layouts/app.blade.php

```php+HTML
.
.
.      
<!-- header -->   
<div class="header">
  <div class="logo">
      <a href="#"><h2>Myweb</h2></a>
  </div>

  <div class="menu">
      <a href="{{ route('articles.index') }}">Articles</a>
  </div>
</div> 

<div class="content">
  <div class="left">
      @yield('content')
  </div>

  <div class="right">
    <!-- 这里 -->
    <div style="padding:20px;border:1px solid black;">
      <h3>Author</h3>
      <p>name : SadCreeper</p>
      <p>age : 22</p>
      <p>Tel : 150-XXXX-XXXX</p>
    </div>
    <!-- 这里 -->
  </div>    
</div>
```

全部完成了！

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid325811labid2512timestamp1484717151888.png/wm)









