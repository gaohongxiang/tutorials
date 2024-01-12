Laravel中所有路由定义在`/routes`目录中。默认提供了四个路由文件用于给不同的入口使用：`web.php`、`api.php`、 `console.php` 和 `channels.php`。

`web.php` 文件包含的路由都位于 RouteServiceProvider 所定义的 web 中间件组约束之内，因而支持 Session、CSRF 保护以及 Cookie 加密功能，如果应用无需提供无状态的、RESTful 风格的 API，那么路由基本上都要定义在 web.php 文件中。

`api.php` 文件包含的路由位于 api 中间件组约束之内，支持频率限制功能，这些路由是无状态的，所以请求通过这些路由进入应用需要通过 token 进行认证并且不能访问 Session 状态。

`console.php` 文件用于定义所有基于闭包的控制台命令，每个闭包都被绑定到一个控制台命令并且允许与命令行 IO 方法进行交互，尽管这个文件并不定义 HTTP 路由，但是它定义了基于控制台的应用入口（路由）。

`channels.php` 文件用于注册应用支持的所有事件广播频道。

## 路由的请求方式

项目的路由都写在web.php文件中。默认定义了应用的首页路由。
```
Route::get('/', function () {
    return view('welcome');
});
```
这段代码的意思是：当访问应用首页的时候，返回`/resources/views/welcome.blade.php`视图中的内容并渲染到浏览器页面中。

#### 常规路由请求方式

##### get|post|put|patch|delete|options

`Route::get|post|put|patch|delete|options($uri, $callback);`
`Route::get|post|put|patch|delete|options($uri, 控制器@方法);`

例：

`Route::get('/hello',function(){return "Hello Laravel!";});`
`Route::post('/users',UsersController@index);`

##### match

可以使用match来匹配请求的方式

`Route::match(['get','post'],'/hello',UsersController@index);`
   
##### any

可以使用any来匹配所有的请求的方式

`Route::any('/hello',function(){ return "Hello Laravel!";});`
   
#### 资源路由请求方式

>`Route::resource($uri, 控制器]);`

这个路由声明包含了处理文章资源对应动作的多个路由。

以`Route::resource('/article', 'ArticleController']);`为例

- 请求方式：`get` URI路径：`/article` 控制器方法：`index`
- 请求方式：`get` URI路径：`/article/create` 控制器方法：`create`
- 请求方式：`post` URI路径：`/article` 控制器方法：`store`
- 请求方式：`get` URI路径：`/article/{参数}` 控制器方法：`show`
- 请求方式：`get` URI路径：`/article/{参数}/edit` 控制器方法：`edit`
- 请求方式：`put/patch` URI路径：`/article/{参数}` 控制器方法：`update`
- 请求方式：`delete` URI路径：`/article/{参数}` 控制器方法：`destroy`

对应的控制器会包含index、create、store、show、edit、update、destroy七种方法。

控制器生成方法`php artisan make:controller ArticleController --resource`

##### 只用其中某些方法

若只想用资源控制器的某些方法，可以在资源路由里用`only`来指定

`Route::resource('/article', 'ArticleController', ['only' => ['show', 'update', 'edit']]);`


## 路由参数

有时我们需要在路由中捕获 URI 片段（比如，要从 URL 中捕获用户 ID），就需要定义路由参数。

路由参数总是通过花括号进行包裹，这些参数在路由被执行时会被传递到路由的闭包。

#### 一个参数
`Route::get('user/{id}', function ($id) {return 'User '.$id});`

#### 多个参数
`Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {});`

#### 可选参数
有时候可能需要指定可选的路由参数，这可以通过在参数名后加一个 ? 标记来实现，这种情况下需要给相应的变量指定默认值。

`Route::get('user/{name?}',function ($name = null) {});`

#### 正则约束

可以使用路由实例上的where方法来约束路由参数的格式。where方法接收参数名和一个正则表达式来定义该参数如何被约束。

```
Route::get('user/{id}/{name}', function ($id, $name) {
...
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```
多个参数时where里用数组来写。一个参数直接写即可

#### 全局约束

如果想要路由参数在全局范围内被给定正则表达式约束，可以使用pattern方法。在`/app/Providers/RouteServiceProvider.php`的`boot`方法中定义约束模式。
```
public function boot(){
   Route::pattern('id', '[0-9]+');
    parent::boot();
}
```
一旦模式被定义，将会自动应用到所有包含该参数名的路由中，例如：
```
Route::get('user/{id}', function ($id) {
    // 只有当 {id} 是数字时才会被调用
});
```

## 路由命名

相当于给路由起了一个别名，然后别的地方能很方便的使用

#### 语法

##### 方式一：使用数组键 as 指定路由名称
`Route::get('user/profile', ['as' => 'profile', function () {}]);`

##### 方式二：路由定义之后使用 name 方法链的方式来实现
`Route::get('user/profile', 'UserController@showProfile')->name('profile');`

此外，还可以为控制器动作指定路由名称
`Route::get('user/profile', ['as' => 'profile', 'uses' => 'UserController@showProfile']);`


## 路由分组

路由分组的目的是让我们在多个路由中共享相同的路由属性，比如中间件和命名空间等，这样的话我们定义了大量的路由时就不必为每一个路由单独定义属性。共享属性以数组的形式作为第一个参数被传递给 Route::group 方法。

#### 前后台分离
```
Route::group(['namespace'=>'Home'],function (){
    //前台路由
});
Route::group(['namespace'=>'admin'],function (){
    //后台路由
});
```

#### 中间件

要给某个路由分组中定义的所有路由分配中间件，可以在定义分组之前使用 `middleware` 方法。中间件将会按照数组中定义的顺序依次执行。
```
Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });

    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});
```
#### 命名空间

路由分组另一个通用的例子是使用 namespace 方法分配同一个 PHP 命名空间给该分组下的多个控制器
```
Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\Http\Controllers\Admin" Namespace
});
```
默认情况下，RouteServiceProvider 在一个命名空间分组下引入所有路由文件，并指定所有控制器类所在的默认命名空间是 `App\Http\Controllers`，因此，我们在定义控制器的时候只需要指定命名空间 `App\Http\Controllers` 之后的部分即可。

#### 子域名路由

路由分组还可以被用于处理子域名路由，子域名可以像 URI 一样被分配给路由参数，从而允许捕获子域名的部分用于路由或者控制器，子域名可以在定义分组之前调用 domain 方法来指定。
```
Route::domain('{account}.blog.dev')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        return 'This is ' . $account . ' page of User ' . $id;
    });
});
```
比如我们设置会员子域名为 account.blog.dev，那么就可以通过 http://account.blog.dev/user/1 访问用户ID为 1 的会员信息了。

#### 路由前缀

prefix 方法可以用来为分组中每个路由添加一个给定 URI 前缀，例如，你可以为分组中所有路由 URI 添加 admin 前缀 
```
Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});
```
这样我们就可以通过 http://blog.dev/admin/users 访问路由了。


## 全局辅助函数route

##### 定义重定向（跳转）

`return redirect()->route('profile');`

##### 生成 URL

`$url = route('profile');`

如果命名路由定义了参数，可以将该参数作为第二个参数传递给 route 函数。给定的路由参数将会自动插入到 URL 中.

` $url = route('profile', ['id' => $id]);`

##### 检查当前路由

判断当前请求是否被路由到给定命名路由，可以使用 Route 实例上的 named 方法。

例如：你可以从路由中间件中检查当前路由名称：
```
public function handle($request, Closure $next)
{
    if ($request->route()->named('profile')) {
        //
    }
    return $next($request);
}
```