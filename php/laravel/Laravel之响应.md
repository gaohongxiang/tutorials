响应，顾名思义，就是一种反馈。

所有路由和控制器处理完业务逻辑之后都会返回一个发送到用户浏览器的响应

>在控制器、路由闭包中，使用 echo 输出内容和使用 return 输出内容有什么区别？

>控制器和路由闭包中返回（return）的数据，最终会交由 laravel 的 HTTP 组件的 Response（响应）类处理，而直接输出（echo等）是由 php 引擎处理，php 会以默认的文件格式、响应头输出，除非使用 header 函数改变。因此与其自己去调取 header() 调整响应头还是其他，都不如 laravel 的 Response 来的简洁实惠。所以，`laravel直接return数据`

Laravel 提供了多种不同的方式来返回响应

最基本的响应就是从路由或控制器返回一个简单的字符串或数组，框架会自动将这个字符串或数组转化为一个完整的 HTTP 响应或JSON响应
`Route::get('/', function () {return 'Hello World';});`
`Route::get('/', function () {return [1, 2, 3];});`

## Response响应

通常，并不只是从路由动作简单返回字符串和数组，大多数情况下，都会返回一个完整的 `Illuminate\Http\Response 实例`或`视图`。

Response 实例继承自 `Symfony\Component\HttpFoundation\Response 基类`，该类提供了一系列方法用于创建 HTTP 响应。


#### Response对象的两种使用方法

方法1：`use Illuminate\Http\Response`基类，使用的时候new一个实例出来
貌似响应不能像请求一样依赖注入。
```
use Illuminate\Http\Response;

Route::get('home', function () {
  return new Response('fibdden','403');         
});
```
方法2：
>laravel提供了全局函数`response()`，直接就可以使用，简洁方便

`Route::get('resp',function(){return response('welcome',200);});`


#### 添加响应头

>大部分响应方法都可以以方法链的形式调用，从而可以流式构建响应（流接口模式）

在发送响应给用户前可以添加一系列响应头

方法1：使用 `header` 方法来添加单条响应头
```
return response($content)
    ->header('Content-Type', $type)
    ->header('X-Header-One', 'Header Value')
    ...
```
方法2：使用 `withHeaders` 方法来指定头信息数组添加到响应
```
return response($content)
    ->withHeaders([
        'Content-Type' => $type,
        'X-Header-One' => 'Header Value',
        ...
    ]);
```

#### 添加cookie到响应

方法1：使用响应实例上的 cookie 方法可以轻松添加 Cookie 到响应

```
return response($content)
    ->header('Content-Type', $type)
    ->cookie('name', 'value', $minutes);
```

方法2：使用 Cookie 门面以”队列”形式将 Cookie 添加到响应。

queue 方法接收 Cookie 实例或创建 Cookie 所必要的参数作为参数，这些 Cookie 会在响应被发送到浏览器之前添加到响应
```
Route::get('cookie/response', function() {
    Cookie::queue(Cookie::make('site', 'Laravel',1));
    Cookie::queue('author', '小明', 1);
    return response('Hello Laravel', 200)
        ->header('Content-Type', 'text/plain');
});
```

>默认情况下，Laravel 框架生成的 Cookie 都经过了加密和签名，以免在客户端被篡改。

如果想让特定的 Cookie 子集在生成时取消加密，可以通过 `App\Http\Middleware` 目录下的中间件 `EncryptCookies` 提供的 `$except` 属性来排除这些 Cookie
```
protected $except = [
    'cookie_name',//例：author
];
```
## 重定向响应(RedirectResponse)

重定向可以简单的理解为跳转


## 其他响应

除了 Response 和 RedirectResponse 两种响应类型，我们还可以通过辅助函数 response 很方便地生成其他类型的响应实例，`当无参数调用 response` 时会返回 Illuminate\Contracts\Routing\ResponseFactory 契约的一个实现，该契约提供了一些有用的方法来生成各种响应，如视图响应、JSON 响应，文件下载、流响应等等。

### 视图响应

如果需要控制响应状态和响应头，并且还需要返回一个视图作为响应内容，可以使用 view 方法：
```
return response()
        ->view('hello', $data, 200)
        ->header('Content-Type', $type);
```

>如不需要传递自定义的 HTTP 状态码和头信息，只需要简单使用全局辅助函数 `view()` 即可

`Route::get('view/response', function() {return view('hello');});`
   
### json响应

json 方法会自动将 Content-Type 头设置为 application/json，并使用 PHP 函数 json_encode 方法将给定数组转化为 JSON 格式数据
```
return response()->json([
        'name' => 'Abigail', 
        'state' => 'CA'
]);
```
#### jsonp响应

方法1：在 json 方法之后调用 withCallback 方法
```
return response()
        ->json(['name' => 'Abigail', 'state' => 'CA'])
        ->withCallback($request->input('callback'));
```
方法2：直接使用 jsonp 方法
```
return response()
        ->jsonp($request->input('callback'), ['name' => 'Abigail', 'state' => 'CA']);
```

### 文件响应

##### 文件显示 `file()`

file 方法可用于直接在用户浏览器显示文件

`return response()->file($pathToFile);`
`return response()->file($pathToFile, $headers);`

参数1：文件路径
参数2：可选，HTTP头信息

例：
```
Route::get('download', function() {
  return response()->file(storage_path('app\photo\test.jpg'));
});
```
##### 文件下载 `download()`

download 方法用于生成用户浏览器下载给定路径文件的响应

`return response()->download($pathToFile);`
`return response()->download($pathToFile, $name, $headers);`
`return response()->download($pathToFile)->deleteFileAfterSend(true);`

参数1：文件路径
参数2：可选，自定义的下载文件名
参数3：可选，HTTP头信息

例：
```
Route::get('download/response', function() {
    return response()->download(storage_path('app\photo\test.jpg'), '测试图片.jpg');
});
```
>注：管理文件下载的 Symfony HttpFoundation 类要求被下载文件有一个 ASCII 文件名，这意味着被下载文件名不能是中文。



## 响应宏

响应宏：定义一个自定义的可以在多个路由和控制器中复用的响应。即共享响应。

使用 Response 门面上的 macro 方法。

例如，在某个服务提供者的 boot 方法中编写代码如下：
```
<?php
namespace App\Providers;
use Illuminate\Support\Facades\Response;
use Illuminate\Support\ServiceProvider;
class ResponseMacroServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Response::macro('caps', function ($value) {
            return Response::make(strtoupper($value));
        });
    }
}
```
macro 方法接收响应名称作为第一个参数，闭包函数作为第二个参数，响应宏的闭包在 ResponseFactory 实现类或辅助函数 response 中调用宏名称的时候被执行：
```
Route::get('macro/response', function() {
    return response()->caps('Laravel');
});
```
在浏览器中访问 http://blog.dev/macro/response，输出如下：laravel