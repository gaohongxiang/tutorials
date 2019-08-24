重定向可以简单的理解为跳转

重定向响应是 `Illuminate\Http\RedirectResponse 类`的实例，包含了必要的头信息将用户重定向到另一个 URL


## 重定向到指定位置

#### user路由重定向到home路由

>全局辅助函数`redirect()`


`Route::get('user', function () {return redirect('home');});`

#### 重定向到命名路由 
>`redirect()->route()`

假设应用包含一个定义如下的路由

`Route::get('/post/{post}', function () {})->name('post.show');`

使用 route 辅助函数生成指向该路由的 URL
`return redirect()->route('post.show', ['post' => 1]);`

通常我们会使用 Eloquent 模型的主键来生成 URL，因此，可以传递 Eloquent 模型作为参数值，route 辅助函数会自动解析模型主键值，所以，上述方法还可以这么调用：
`return redirect()->route('post.show', ['post' => $post]);`


#### 重定向到控制器动作 
>`redirect()->action()`

只需传递控制器和动作名到 action 方法即可。

不需要指定控制器的完整命名空间，因为 Laravel 的 RouteServiceProvider 将会自动设置默认的控制器命名空间

`return redirect()->action('HomeController@index');`

如果控制器路由要求参数，将参数作为第二个参数传递给 action 方法

`return redirect()->action('UserController@profile', ['id'=>1]);`

#### 带一次性 Session 数据的重定向

>`redirect()->withInput()`

如果你经常需要一次性存储输入请求并返回到表单填写页，可以在 `redirect()` 之后调用 `withInput()` 方法实现这样的功能：

`return redirect('form')->withInput();`
`return redirect('form')->withInput(session()->flash('key', 'value'));`

一次性session数据参考[Laravel 之 session](https://app.yinxiang.com/shard/s66/sh/34db2d14-9f04-434b-b102-06a97febad23/cffc6060126baea3cafc723a6ba613be)

用户重定向到新页面之后，可以从 Session 中取出并显示这些一次性信息，使用 Blade 语法实现如下  `{{ session('key') }}`


#### 返回到前一个url

有时候你想要将用户重定向到上一个请求的位置，比如，表单提交后，验证不通过，你就可以使用辅助函数 back 返回到前一个 URL

>全局辅助函数`back`

返回前一个url
`return back();`
返回前一个url，并带请求时的参数
`return back()->withInput();`

注：由于该功能使用了 Session，使用该方法之前确保相关路由位于 web 中间件组或者应用了 Session 中间件