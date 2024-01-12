数据填充用于生成测试数据

用模型工厂方法进行批量数据填充

以goods填充数据为例

## 创建Model

`app/Models`目录下创建`Good.php`
>`php artisan make:model Models/Good`

此Model即对于goods数据表

## 创建模型工厂

>`php artisan make:factory GoodFactory`

默认创建的`GoodFactory.php`会在`database/factories`目录下
```
<?php

use Faker\Generator as Faker;

$factory->define(\App\Models\Good::class, function (Faker $faker) {
    return [
        'cid'=> $faker->numberBetween(1,10),
        'key_word'=>$faker->word,
        'num'=>$faker->numberBetween(100,300),
        'price'=>$faker->numberBetween(20,60),
        'title'=>str_random(10),
        'desc'=>str_random(10),
        'config'=>str_random(10),
    ];
});
```
define 方法中
第一个参数表示关联`Good模型类` 注意下路径
第二个参数传入的是`$faker`

Faker是一个开源类库,主要用于生成一些测试数据,比如电话号码,人名,IP地址等等,这里Laravel内置了Faker,因此可以直接使用.

>function闭包中是需要根据自己的表来填的。对应每个必须的字段,填充上对应的值

## 创建seeder文件

>`php artisan make:seeder GoodsTableSeeder`

默认创建的`GoodsTableSeeder.php`会在`database/seeds`目录下

```
<?php

use Illuminate\Database\Seeder;

class GoodsTableSeeder extends Seeder
{
    public function run()
    {
	    //create()表示插入数据库中
        factory(\App\Models\Good::class)->times(10)->create(); 
    }
}
```

## 执行填充

>`php artisan db:seed --class=GoodsTableSeeder`

执行完毕之后,就会发现goods数据表中新增了十条数据

## 多个数据表填充

不同的表创建不同的model、模型工厂、seeder文件

比如categorys表

`Category.php`
`CategoryFactory.php`
`CategorysTableSeeder.php`

创建完需要的文件，并且字段和值都填好后，把seeder文件引入`database/seeds/DatabaseSeeder.php`文件中
```
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(GoodsTableSeeder::class);
        $this->call(CategorysTableSeeder::class);
    }
}
```
执行多个seeder文件的填充

>`php artisan db:seed`

这个命令的作用就是执行DatabaseSeeder 的run方法,因此只需要执行上面的命令,就可执行所有表的填充命令了!


其实还有个最为强大的命令

>`php artisan migrate:refresh --seed`

这个命令主要做了三件事:

执行所有的回滚,也就是migrations目录下所有文件的down方法,执行的时候按照时间顺序.

所有回滚执行完毕后,执行所有的迁移,也就是按照时间顺序执行所有的up方法.

执行所有的数据填充,也就是执行DatabaseSeeder 中的run方法.如果在run方法中没有调用其他的seeder,则这个seeder的run方法不会被执行.

#### 更新文件
若填充过程报错，比如某个文件不存在,需要执行一个命令
>`composer dump-autoload`

这个命令的主要作用是让 composer 更新 `autoload_classmap` 的内容,包含到`新建的文件`


参考文章：[Laravel 实践之路: 数据库迁移与数据填充](https://blog.csdn.net/rain_while/article/details/60764741)

#### 自增Id归零

测试完成后，自增Id归零

>使用mysql的truncate命令，用法：`truncate table 表名;`