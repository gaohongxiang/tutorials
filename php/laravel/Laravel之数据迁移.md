数据迁移是指在laravel中有一个或多个文件（`database/migrations`目录下），这个文件中写了laravel本身内置的数据结构生成器对数据库的“命令“，例如可以创建修改删除库、表、字段。通过这些文件中的代码，便可以通过版本控制达到控制数据库的目的。团队协作中，只需要统一运行下这些文件的迁移命令，即可同步不同开发人员的数据库。


Laravel的`Schema门面`提供了与数据库系统无关的创建和操纵表的支持，在Laravel所支持的所有数据库系统中提供一致的、优雅的、平滑的API。

>#### 数据迁移基本思路
1、生成迁移文件`php artisan make:migration 迁移文件名`

>2、up方法里用表结构构建器`Schema`处理需求。
如新建表、重名名表、删除表、增加表字段、修改字段属性
参考[数据结构生成器（schema）](https://laravel-china.org/docs/laravel/5.6/migrations/1400)

>3、执行迁移`php artisan migrate`（可以多个迁移文件一起迁移）

>迁移文件若执行了迁移操作（可以是多个迁移文件），那么就相当于生成了一个数据库的迁移版本。要想执行其他的数据库操作。就要重新生成新的迁移文件，重新执行迁移操作。

>回滚迁移（相当于撤销迁移操作）
>`php artisan migrate:rollback|reset|fresh|refresh`
>调用迁移文件里的down方法。

>##### 显示当前迁移的状态 

>`php artisan migrate:status`


## 创建迁移文件

>命令：`php artisan make:migration create_users_table`

可以加`--path=路径`的参数，指定生成迁移文件的位置

系统会在`database/migrations`这个目录下生成一个类似`2018_06_06_041929_create_users_table.php`的migration类文件。

>migration类文件中都会包含	`up`和`down`两个方法。

## 处理需求

在生成的迁移文件的`up方法`里处理

这里列出了常见的需求，根据自己的实际情况选用即可

#### 创建表

`Schema::create(表名,function(Blueprint $table){...})`

同一个迁移文件可创建多张表

```
public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->timestamps();
            ...
        });
        Schema::create('articles', function (Blueprint $table) {
            ...
        });
    }
```
#### 删除表
`Schema::dropIfExists('表名');`
```
public function up()
    {
        Schema::dropIfExists('users');
    }
```
#### 重命名

`Schema::rename('users', 'hahas');`



#### 增加字段
```
Schema::table('goods', function (Blueprint $table) {
      $table->string('name');//添加name字段
      ...
});
```

#### 修改字段

>先决条件

>在修改字段之前，请确保将 doctrine/dbal 依赖添加到 composer.json 文件中。Doctrine DBAL 库用于确定字段的当前状态， 并创建对该字段进行指定调整所需的 SQL 查询。若没此依赖，执行下面命令安装

>`composer require doctrine/dbal`

##### 更新字段属性
```
Schema::table('users', function (Blueprint $table) {
    $table->string('name', 50)->change();
});
```

change 方法可以将现有的字段类型修改为新的类型或修改属性。

##### 重命名字段
```
Schema::table('users', function (Blueprint $table) {
    $table->renameColumn('from', 'to');
});
```
在重命名字段前， 请确保 composer.json 文件内已经加入 doctrine/dbal 依赖

##### 删除字段
```
Schema::table('users', function (Blueprint $table) {
	//删除单个字段
    $table->dropColumn('votes');
    //删除多个字段
    $table->dropColumn(['votes', 'avatar', 'location']);
});
```

## 执行迁移

使用 Artisan 命令 migrate 来运行所有未完成的迁移文件，有几个执行几个（`如果使用 Homestead 虚拟机，则应该在虚拟机中执行命令`）

>`php artisan migrate`

>迁移文件若执行了迁移操作（可以是多个迁移文件），那么就相当于生成了一个数据库的迁移版本。要想执行其他的数据库操作。就要重新生成新的迁移文件，重新执行迁移操作。

#### 更新文件
若填充过程报错，比如某个文件不存在,需要执行一个命令
>`composer dump-autoload`

这个命令的主要作用是让 composer 更新 `autoload_classmap` 的内容,包含到`新建的文件`


#### 执行down方法的一些命令

#####  migrate:rollback

`php artisan migrate:rollback`

撤销上一步迁移（只一步），上一次migrate有多少变动，如生成5张表，这一次会全部撤销 

在写迁移时偶尔也会犯错误。如果已经运行了迁移，那么不能只是编辑迁移和再次运行迁移。Laravel假定它已经运行了迁移，那么当你再次运行artisan migrate，不会做任何事情。你必须使用`php artisan migrate:rollback`回滚迁移，然后编辑迁移，再运行artisan migrate去运行正确的版本。
   
##### migrate:reset

`php artisan migrate:reset`

回滚所有的迁移（会删掉所有表和数据）

##### migrate:fresh|refresh

`php artisan migrate:fresh|refresh`

将删除数据库、 重新创建它并将加载当前架构。这是一个方便快方式去运行重置并随后重新运行所有迁移。fresh是一步撤销。refresh是一步一步撤销