由于 HTTP 协议本身是无状态的，上一个请求与下一个请求无任何关联，为此我们引入 Session 来存储用户请求信息以解决特定场景下无状态导致的问题（比如登录、购物）。

Laravel 通过简洁的 API 统一处理后端各种 Session 驱动，目前开箱支持的流行后端驱动包括 Memcached、Redis 和数据库。

注：`Laravel 并没有使用 PHP 内置的 Session 功能，而且自己实现了一套更加灵活更加强大的 Session 机制`。核心逻辑请参考 Illuminate\Session\Middleware\StartSession 这个中间件，因此在 Laravel 应用中不要试图通过 $_SESSION 方式去获取应用的 Session 值，这是徒劳的。

另外，在 Laravel 的控制器构造函数中无法获取应用 Session 数据，这是因为 Laravel 的 Session 通过 StartSession 中间件启动，既然是中间件就会在服务容器注册所有服务之后执行，而控制器们的构造函数都是在容器注册服务的时候执行的，所以这个时候 Session 尚未启动，又何来的获取数据呢？`解决办法是将获取 Session 数据逻辑后置或者在构造函数中引入在 StartSession 之后执行的中间件。`

## Session驱动

Session 驱动用于定义请求的 Session 数据存放在哪里
Laravel 可以处理多种类型的驱动（存储信息的方式和位置）

- `file` Session 数据存储在 `storage/framework/sessions` 目录下
- `cookie` Session 数据存储在经过安全加密的 Cookie 中
- `database` Session 数据存储在数据库中
- `memcached/redis` Session 数据存储在Memcached/Redis缓存中,访问速度最快
- `array` Session 数据存储在PHP 数组中,在多个请求之间是非持久化的。数组驱动通常用于运行测试以避免 Session 数据持久化。

>默认情况下，Laravel 使用的 Session 驱动为 file 驱动。
>Session 配置文件为 `config/session.php`。默认Session驱动代码如下` 'driver' => env('SESSION_DRIVER', 'file'),`

#### 更改session驱动

laravel默认的session驱动使用file驱动，如有需要可以更改session驱动

##### database驱动
当使用 database 作为 Session 驱动时，需要设置表包含 Session 字段
可以使用 Artisan 命令 session:table 在数据库中创建这张表

##### memcached / redis驱动

在生产环境中，可能考虑使用 memcached 或者 redis 驱动以便获取更佳的 Session 性能，尤其是线上同一个应用部署到多台机器的时候，这是最佳实践。

在 Laravel 中使用 Redis 作为 Session 驱动前，需要通过 Composer 安装 `predis/predis`包。可以在 database 配置文件中配置 Redis 连接，在 Session 配置文件中，connection 选项用于指定 Session 使用哪一个 Redis 连接。

放在哪理论上都没问题，但必须要注册，能加载的到



>在 Laravel 中主要有两种方式处理 Session 数据
- `全局辅助函数 session()`
- `通过 Request 实例`（请求实例的 session 属性）

>request实例的依赖注入参考(Laravel之请求)[]

## 存储session数据

请求传过来的数据存到session。
key/value：把传过来的value存到session里，起名叫key

#### 常规存储session数据

>方法1：request实例`$request->session()->put()`
方法2：全局辅助函数`session()`

request实例session属性的put方法存储session数据

`$request->session()->put('key', 'value');`一条
`$request->session()->put(['key', 'value'],['key', 'value']);`多条

session全局辅助函数用 key/value 键值对数组的方式来存储session数据

`session(['key' => 'value']);`一条
`session(['key' => 'value','key' => 'value']);`多条

#### 存储一次性session数据

存储的 Session 数据只在随后的 HTTP 请求中有效，然后将会被删除

>方法1：request实例`$request->session()->flash()`
方法2：全局辅助函数`session()->flash()`


`session()->flash('key', 'value');`
`$request->session()->flash('key', 'value');`



指定数据存入一次性session

>request实例`$request->session()->flashOnly()|falshExcept()`

`$request->flashOnly(['key1', 'key2']);`（只有）
`$request->flashExcept('key');`（除了）

>获取用全局辅助函数old()

这些方法在 Session 之外保存敏感信息时很有用，该功能适用于登录密码填写错误的场景


如果需要在更多请求中保持该一次性数据，可以使用 reflash 方法，该方法将所有一次性数据保留到下一个请求
`session()->reflash('key', 'value');`
`$request->session()->reflash('key', 'value');`

如果只是想要保存特定一次性数据，可以使用 keep 方法
`session()->keep(['key', 'value']);`
`$request->session()->keep(['key', 'value']);`


#### 推送数据到session数组 

push 方法可用于推送数据到值为数组的 Session，例如，如果 user.teams 键包含团队名数组，可以像这样推送新值到该数组。用于团队协作

`$request->session()->push('user.teams', 'developers');`


#### 重新生成 Session ID

重新生成 Session ID 经常用于阻止恶意用户对应用进行 session fixation 攻击
参考：http://www.360doc.com/content/11/1028/16/1542811_159889635.shtml

如果使用内置的 LoginController 的话，Laravel 会在认证期间自动重新生成 session ID

如果需要手动重新生成 session ID，可以使用 regenerate 方法
`session()->regenerate();`
`$request->session()->regenerate();`


## 获取session数据

#### 获取一条 session 数据

>方法1：request实例 `$request->session()->get()`
方法2：全局辅助函数`session()`

request实例方式
`$data = $request->session()->get('key');`
`$data = $request->session()->get('key','default');`

session全局辅助函数方式
`$data = session('key');`
`$data = session('key','default');`
 
get 方法可以接受第二个参数作为session默认值，默认值在指定键在 Session 中不存在时返回。（默认值也支持回调函数）

#### 获取所有 session 数据


>方法1：request实例`$request->session()->all()`
方法2：全局辅助函数`session()->all()`

`$data = session()->all();`

`$data = $request->session()->all();`

#### 获取后并删除数据

pull 方法将会通过一条语句从 Session 获取并删除数据

`$value = $request->session()->pull('key', 'default');`


#### 判断 Session 中是否存在指定项

has 方法。存在且不为null则返回true
`session()->has('users');`
`$request->session()->has('users')`


exists 方法。存在即返回true
`session()->exists('users');`
`$request->session()->exists('users')`


## 删除数据

#### 删除所有session数据

flush方法
`session()->flush();`
`$request->session()->flush();`

#### 删除指定session数据  

forget 方法
`session()->forget('key');`
`$request->session()->forget('key');`


## 添加自定义 Session 驱动

>步骤：实现驱动->注册驱动->使用驱动

#### 实现驱动

自定义 Session 驱动需要实现 SessionHandlerInterface 接口，该接口包含少许我们需要实现的方法，比如一个基于 MongoDB 的 Session 驱动实现如下：
```
<?php
namespace App\Extensions;
class MongoHandler implements SessionHandlerInterface
{
    public function open($savePath, $sessionName) {}
    public function close() {}
    public function read($sessionId) {}
    public function write($sessionId, $data) {}
    public function destroy($sessionId) {}
    public function gc($lifetime) {}
}
```
注：Laravel 默认并没有附带一个用于包含扩展的目录。larabvel的session驱动类放在了`Illuminate/session`目录下。
理论上自定义的session驱动类也应该放在此处，但实际项目中，框架代码是不上传的，如果自定义的东西放在此处就用不了了。所以，一般创建一个 `APP/Extensions 目录`，此目录用于存放自定义的驱动类。如 `MongoHandler.php`。

##### 方法介绍：

open 方法用于基于文件的 Session 存储系统，由于 Laravel 已经有了一个 file Session 驱动，所以在该方法中不需要放置任何代码，可以将其置为空方法。

close 方法和 open 方法一样，也可以被忽略，对大多数驱动而言都用不到该方法。

read 方法应该返回与给定 $sessionId 相匹配的 Session 数据的字符串版本，从驱动中获取或存储 Session 数据不需要做任何序列化或其它编码，因为 Laravel 已经为我们做了序列化。

write 方法应该将给定 `$data` 写到持久化存储系统相应的 `$sessionId`, 例如 MongoDB, Dynamo 等等。再次重申，不要做任何序列化操作，Laravel 已经为我们处理好了。

destroy 方法从持久化存储中移除 `$sessionId` 对应的数据。

gc 方法销毁大于给定 $lifetime 的所有 Session 数据，对本身拥有过期机制的系统如 Memcached 和 Redis 而言，该方法可以留空。

#### 注册驱动

Session 驱动被实现后，需要将其注册到框架，要添加额外驱动到 Laravel Session 后端，可以使用 Session 门面上的 extend 方法。

`app/Providers`目录下新建`SessionServiceProvider.php`的文件
```
<?php
namespace App\Providers;
use App\Extensions\MongoSessionStore;
use Illuminate\Support\Facades\Session;
use Illuminate\Support\ServiceProvider;
class SessionServiceProvider extends ServiceProvider
{
    public function boot()
    {
        Session::extend('mongo', function($app) {
            // Return implementation of SessionHandlerInterface...
            return new MongoSessionStore;
        });
    }
    public function register()
    {
        //
    }
}
```

Session 驱动被注册之后，就可以在配置文件 `config/session.php` 中使用 mongo 驱动了。`'driver' => env('SESSION_DRIVER', 'mongo'),`