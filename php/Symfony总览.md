Symfony是MVC模式的PHP框架。

>开发流程：将`用户请求`（例如点击某一条连接或地址栏输入域名）通过`路由`（route）引导入`控制器`（controller），控制器处理业务逻辑（必要时调用`model`层操作数据库，处理数据），并返回与其关联的资源（`view`层HTML页面）。

## 学习资料

[Symfony文档](http://symfony.cn/docs/index.html)

[Symfony2 学习笔记之内部构件](https://www.cnblogs.com/Seekr/archive/2012/06/25/2562780.html)

[HttpFoundation组件](http://symfonychina.com/doc/current/components/http_foundation.html)(请求、响应)

## 目录结构
```
app/

src/

web/

vendor/
```
#### app/

应用程序配置，模板和翻译

#### src/

该项目的PHP代码

#### web/

Web根目录

Web根目录是所有公共和静态文件（如图像，样式表和JavaScript文件）的主页。它也是每个前端控制器 所在的位置。

相当于laravel、TP的public目录

#### vendor/

第三方依赖项


## 请求

PHP用`$_GET`，`$_POST`，`$_FILES`，`$_COOKIE`，`$_SESSION`，`$_SERVER`来获取HTTP请求报文的数据等数组变量。

symfony 通过httpFoundation组件的Request 类，抽象了PHP中主要的全局变量`$_GET`，`$_POST`，`$_FILES`，`$_COOKIE`，`$_SESSION`，`$_SERVER`，来实现用户请求的接收。
```
<?php
namespace AppBundle\Controller;
use Symfony\Component\HttpFoundation\Request;

class TestController extends Controller
{
	public function IndexAction(Request $request)//注入request实例
	{
		$request = Request::createFromGlobals();//创建一个request对象（与注入request实例二选一）
		
		$request->get('name'); //get或post方式接收参数
		$request->getAlnum('name'); //返回按字母和数字排序的参数值
		$request->query->get('name'); //get方式接收参数
		$request->request->get('name'); //post方式接收参数
		//getter方法接收两个参数（arguments）：第一个是参数名（parameter name），第二个为可选参数，当第一个参数不存在时的默认返回值
		
		$request->getSession(); //获取session值
	}
}
```
详情见[HttpFoundation组件](http://symfonychina.com/doc/current/components/http_foundation.html)请求部分

## 路由

路由的作用就是`把URL路径映射到控制器`或者`把控制器变成URL`。

把控制器编程URL实际用途就是接口。写好接口，生成URL，其他地方就可以调用此URL来使用控制器里的方法。

#### 路由文件

顶层路由配置文件: `app/config/routing.yml`。 在这个文件中, 你可以加载其它的路由文件。

比如你，导入自定义的bundle中的路由资源`AppBundle/Controller/routing.yml`

```
# app/config/routing.yml
app:
	resource: "@AppBundle/Controller/routing.yml"
```

##### 给导入的路由资源添加前缀

你可以为导入的路由资源指定prefix来添加一个前缀，比如说后台路由一般都有个`/admin`（如`/admin/hello/{name}`）

```
# app/config/routing.yml
blog:
    resource: "@AppBundle/Controller/routing.yml"
    prefix:   /admin
```

#### 路由定义(.yml格式为例)

symfony路由定义有四种格式

- 通过yaml配置文件定义路由（推荐此方法）
- 通过xml配置文件定义路由
- 通过php配置文件定义路由
- 通过注释定义路由

路由文件：`AppBundle/Controller/routing.yml`

基础路由是由两部分组成：`pattern`部分和`defaults数组`部分。

键（比如blog)是没有意义的，只是用来保证该资源唯一不被其它行覆盖。
`pattern`为路由的匹配规则
`defaults数组`为对应的控制器动作

defaults数组部分的书写格式

`_controller`: 决定了当路由匹配时，哪个controller被执行。
`_format`: 用于设置请求格式。
`_locale`: 用于在session上设置本地化。

```
bundle:controller:action
包名:控制器:方法
```

比如`AppBundle:Blog:index`的意思是：包名为`AppBundle`, 控制器为`Blog`, 方法为`index`。

带条件的路由使用`requirements`来添加条件

```
article_show:
  pattern:  /articles/{culture}/{year}/{title}.{_format}
  defaults: { _controller: AppBundle:Article:show, _format: html }
  requirements:
      culture:  en|fr
      _format:  html|rss
      year:     \d+
```

##### 基础路由

```
welcome:
    pattern:   /
    defaults:  { _controller: AppBundle:Blog:index }
```

##### 带占位符路由

该模式将匹配任何类似`/blog/*`形式的URL。不会匹配`/blog`, 因为默认情况下所有的占位符都是必须的。 当然可以通过在defaults数组中给这些占位符赋来改变它。

```
blog_show:
    pattern:   /blog/{slug}
    defaults:  { _controller: AppBundle:Blog:show }
```
##### 添加约束条件

例1、匹配数字

页码类的只能是数字如`/blog/1`，而不能是`/blog/show`，所以，就必须给pattern添加约束

```
blog:
    pattern:   /blog/{page}
    defaults:  { _controller: AppBundle:Blog:index, page: 1 }
	requirements:
        page:  \d+
```
`\d+ 约束是一个正则表达式，它指定了{page}只接受整数`

注意路由的书写顺序或者约束条件，如果普通带占位符的路由写在了此条路由前面，那么`/blog/1`就会匹配到普通占位符的路由，它是第一个满足条件的路由。

解决办法是把带约束的路由写在普通占位符路由前，或者修改普通占位符路由为其他约束条件的路由。

例2、添加HTTP请求方法约束

```
contact:
    pattern:  /contact
    defaults: { _controller: AppBundle:Blog:contact }
    requirements:
        _method:  GET

contact_process:
    pattern:  /contact
    defaults: { _controller: AppBundle:Blog:contactProcess }
    requirements:
        _method:  POST
```

第一个路由只会匹配GET请求，而第二个只会匹配POST请求。这就意味着你可以通过同一个URL来显示表单并提交表单，而用不同的action对他们进行处理。

`如果没有指定_method约束，那么该路由会匹配所有请求方法。`跟其它约束一样，_method约束也接受正则表达式，如果只想匹配GET或者POST那么你可以用GET|POST

例3、高级路由
```
article_show:
  pattern:  /articles/{culture}/{year}/{title}.{_format}
  defaults: { _controller: AppBundle:Article:show, _format: html }
  requirements:
      culture:  en|fr
      _format:  html|rss
      year:     \d+
```
此路由在匹配时只会匹配{culture}部分值为en或者fr并且{year}的值为数字的URL。该路由还告诉我们，可以用在占位符之间使用区间代替斜线。

它能够匹配如下URL：
　　`/articles/en/2010/my-post`
　　`/articles/fr/2010/my-post.rss`

这其中有个特殊的路由参数 _format，在使用该参数时，其值变为请求格式。这种请求格式相当于Respose对象的Content-Type，比如json请求格式会翻译成一个Content-Type为application/json.该参数可以用于在controller中为每个_format渲染一个不同的模板。`它是一个很强的方式来渲染同一个内容到不同的格式。`

例4、强制路由使用HTTPS或者HTTP

有时候为了安全起见，你需要你的站点必须使用HTTPS协议访问。这时候路由组件可以通过_scheme 约束来强迫URI方案。比如：
```
secure:
    pattern:  /secure
    defaults: { _controller: AppBundle:Blog:secure }
    requirements:
        _scheme:  https
```
#### 路由与控制器

事实上，全部的defaults集合和表单的参数值合并到一个单独的数组中。这个数组中的每个键都会成为controller方法的参数。换句话说，你的controller方法的每一个参数，Symfony都会从路由参数中查找并把找到的值赋给给参数。

上面例子中的变量 $culture, $year,$title,$_format,$_controller 都会作为showAction()方法的参数。因为占位符和defaults集合被合并到一起，即使$_controller变量也是一样。你也可以使用一个特殊的变量$_route 来指定路由的名称。

#### 可视化并调试路由

当你添加和个性化路由时，能够看到它并能获取一些细节信息将是非常有用的。通过 `router:debug` 命令行工具可以查看你应用程序的路由。

项目目录下执行命令

输出你应用程序的所有路由
```
php app/console router:debug
```

添加某个路由的名字来获取单个路由信息

```
php app/console router:debug blog
```

[Symfony系列-路由](https://blog.csdn.net/wolfqong/article/details/52211056)

[路由](http://symfony.cn/docs/cookbook/routing/index.html)

## Controller

注意Symfony规定必须在类名后加上Controller (Blog=>BlogController), 方法名后加上Action(show => showAction).
## Model


## 数据库

symfony完整的框架没有默认集成ORM，但是symfony标准版，集成了很多程序，还自带集成了Doctrine这样一个库.

Doctrine是基于数据库抽像层上的ORM,它可以通过PHP对象轻松访问所有的数据库(例如MYSQL)，它`支持的PHP最低版本为5.2.3`


你会学会doctrine的基本理念并且能够了解如何轻松使用数据库。

#### 获取数据

#### 插入数据
```
持久化对象到数据库
现在我们有了一个Product实体和与之映射的product数据库表。你可以把数据持久化到数据库里。在Controller内，它非常简单。添加下面的方法到bundle的DefaultController中。


// src/AppBundle/Controller/DefaultController.php
 
// ...
use AppBundle\Entity\Product;
use Symfony\Component\HttpFoundation\Response;
 
// ...
public function createAction()
{
    $product = new Product();
    $product->setName('A Foo Bar');
    $product->setPrice('19.99');
    $product->setDescription('Lorem ipsum dolor');
 
    $em = $this->getDoctrine()->getManager();
 
    $em->persist($product);
    $em->flush();
 
    return new Response('Created product id '.$product->getId());
}
```
#### 更新数据

#### 删除数据

https://blog.csdn.net/woshiliulei0/article/details/50686562


## 响应
Response类，抽象类一些PHP函数比如header(), setcookie()和echo();
## view