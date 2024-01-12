PHP数组数据封装成无限极分类树

来源于网络

原数组

```
$navigation = array(//定义外层数组
            1=>array('id'=>1,'name'=>'首页','pid'=>0,'type'=>'top'),
            2=>array('id'=>2,'name'=>'课程','pid'=>0,'type'=>'top'),
            3=>array('id'=>3,'name'=>'资讯','pid'=>0,'type'=>'top'),
            4=>array('id'=>4,'name'=>'小学','pid'=>2,'type'=>'top'),
            5=>array('id'=>5,'name'=>'初中','pid'=>2,'type'=>'top'),
            6=>array('id'=>6,'name'=>'高中','pid'=>2,'type'=>'top'),
			7=>array('id'=>7,'name'=>'四年级','pid'=>4,'type'=>'top'),
			8=>array('id'=>8,'name'=>'五年级','pid'=>4,'type'=>'top'),
			9=>array('id'=>9,'name'=>'初一','pid'=>5,'type'=>'top'),
			10=>array('id'=>10,'name'=>'初二','pid'=>5,'type'=>'top'),
			11=>array('id'=>11,'name'=>'初三','pid'=>5,'type'=>'top'),
			12=>array('id'=>12,'name'=>'高一','pid'=>6,'type'=>'top'),
			13=>array('id'=>13,'name'=>'高二','pid'=>6,'type'=>'top'),
			14=>array('id'=>14,'name'=>'高三','pid'=>6,'type'=>'top'),
			15=>array('id'=>15,'name'=>'语文','pid'=>7,'type'=>'top'),
			16=>array('id'=>16,'name'=>'数学','pid'=>7,'type'=>'top'),
			17=>array('id'=>17,'name'=>'语文','pid'=>8,'type'=>'top'),
			18=>array('id'=>18,'name'=>'数学','pid'=>8,'type'=>'top'),
			19=>array('id'=>19,'name'=>'语文','pid'=>9,'type'=>'top'),
			20=>array('id'=>20,'name'=>'数学','pid'=>9,'type'=>'top'),
			21=>array('id'=>21,'name'=>'语文','pid'=>10,'type'=>'top'),
			22=>array('id'=>22,'name'=>'数学','pid'=>10,'type'=>'top'),
			23=>array('id'=>23,'name'=>'语文','pid'=>11,'type'=>'top'),
			24=>array('id'=>24,'name'=>'数学','pid'=>11,'type'=>'top'),
			25=>array('id'=>25,'name'=>'语文','pid'=>12,'type'=>'top'),
			26=>array('id'=>26,'name'=>'数学','pid'=>12,'type'=>'top'),
			27=>array('id'=>27,'name'=>'语文','pid'=>13,'type'=>'top'),
			28=>array('id'=>28,'name'=>'数学','pid'=>13,'type'=>'top'),
			29=>array('id'=>29,'name'=>'语文','pid'=>14,'type'=>'top'),
			30=>array('id'=>30,'name'=>'数学','pid'=>14,'type'=>'top'),
        );
```

改造为树形数据

#### 方法1 以id为键
```
function getTree($items) {
    foreach ($items as $item)
       $items[$item['pid']]['son'][$item['id']] = &$items[$item['id']];
       return isset($items[0]['son']) ? $items[0]['son'] : array();
}
$result = getTree($data);
```
#### 方法2 正常索引
```
function getTree($items) {
    $tree = array(); //格式化好的树
    foreach ($items as $item)
       if (isset($items[$item['pid']]))
           $items[$item['pid']]['son'][] = &$items[$item['id']];
       else
           $tree[] = &$items[$item['id']];
     return $tree;
}
$result = getTree($data);
```

结果(方法1为例)
```
array(3) {
  [1]=>
  array(4) {
    ["id"]=>
    int(1)
    ["name"]=>
    string(6) "首页"
    ["pid"]=>
    int(0)
    ["type"]=>
    string(3) "top"
  }
  [2]=>
  array(5) {
    ["id"]=>
    int(2)
    ["name"]=>
    string(6) "课程"
    ["pid"]=>
    int(0)
    ["type"]=>
    string(3) "top"
    ["son"]=>
    array(3) {
      [4]=>
      array(5) {
        ["id"]=>
        int(4)
        ["name"]=>
        string(6) "小学"
        ["pid"]=>
        int(2)
        ["type"]=>
        string(3) "top"
        ["son"]=>
        array(2) {
          [7]=>
          array(5) {
            ["id"]=>
            int(7)
            ["name"]=>
            string(9) "四年级"
            ["pid"]=>
            int(4)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [15]=>
              array(4) {
                ["id"]=>
                int(15)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(7)
                ["type"]=>
                string(3) "top"
              }
              [16]=>
              array(4) {
                ["id"]=>
                int(16)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(7)
                ["type"]=>
                string(3) "top"
              }
            }
          }
          [8]=>
          array(5) {
            ["id"]=>
            int(8)
            ["name"]=>
            string(9) "五年级"
            ["pid"]=>
            int(4)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [17]=>
              array(4) {
                ["id"]=>
                int(17)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(8)
                ["type"]=>
                string(3) "top"
              }
              [18]=>
              array(4) {
                ["id"]=>
                int(18)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(8)
                ["type"]=>
                string(3) "top"
              }
            }
          }
        }
      }
      [5]=>
      array(5) {
        ["id"]=>
        int(5)
        ["name"]=>
        string(6) "初中"
        ["pid"]=>
        int(2)
        ["type"]=>
        string(3) "top"
        ["son"]=>
        array(3) {
          [9]=>
          array(5) {
            ["id"]=>
            int(9)
            ["name"]=>
            string(6) "初一"
            ["pid"]=>
            int(5)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [19]=>
              array(4) {
                ["id"]=>
                int(19)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(9)
                ["type"]=>
                string(3) "top"
              }
              [20]=>
              array(4) {
                ["id"]=>
                int(20)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(9)
                ["type"]=>
                string(3) "top"
              }
            }
          }
          [10]=>
          array(5) {
            ["id"]=>
            int(10)
            ["name"]=>
            string(6) "初二"
            ["pid"]=>
            int(5)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [21]=>
              array(4) {
                ["id"]=>
                int(21)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(10)
                ["type"]=>
                string(3) "top"
              }
              [22]=>
              array(4) {
                ["id"]=>
                int(22)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(10)
                ["type"]=>
                string(3) "top"
              }
            }
          }
          [11]=>
          array(5) {
            ["id"]=>
            int(11)
            ["name"]=>
            string(6) "初三"
            ["pid"]=>
            int(5)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [23]=>
              array(4) {
                ["id"]=>
                int(23)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(11)
                ["type"]=>
                string(3) "top"
              }
              [24]=>
              array(4) {
                ["id"]=>
                int(24)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(11)
                ["type"]=>
                string(3) "top"
              }
            }
          }
        }
      }
      [6]=>
      array(5) {
        ["id"]=>
        int(6)
        ["name"]=>
        string(6) "高中"
        ["pid"]=>
        int(2)
        ["type"]=>
        string(3) "top"
        ["son"]=>
        array(3) {
          [12]=>
          array(5) {
            ["id"]=>
            int(12)
            ["name"]=>
            string(6) "高一"
            ["pid"]=>
            int(6)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [25]=>
              array(4) {
                ["id"]=>
                int(25)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(12)
                ["type"]=>
                string(3) "top"
              }
              [26]=>
              array(4) {
                ["id"]=>
                int(26)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(12)
                ["type"]=>
                string(3) "top"
              }
            }
          }
          [13]=>
          array(5) {
            ["id"]=>
            int(13)
            ["name"]=>
            string(6) "高二"
            ["pid"]=>
            int(6)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [27]=>
              array(4) {
                ["id"]=>
                int(27)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(13)
                ["type"]=>
                string(3) "top"
              }
              [28]=>
              array(4) {
                ["id"]=>
                int(28)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(13)
                ["type"]=>
                string(3) "top"
              }
            }
          }
          [14]=>
          array(5) {
            ["id"]=>
            int(14)
            ["name"]=>
            string(6) "高三"
            ["pid"]=>
            int(6)
            ["type"]=>
            string(3) "top"
            ["son"]=>
            array(2) {
              [29]=>
              array(4) {
                ["id"]=>
                int(29)
                ["name"]=>
                string(6) "语文"
                ["pid"]=>
                int(14)
                ["type"]=>
                string(3) "top"
              }
              [30]=>
              array(4) {
                ["id"]=>
                int(30)
                ["name"]=>
                string(6) "数学"
                ["pid"]=>
                int(14)
                ["type"]=>
                string(3) "top"
              }
            }
          }
        }
      }
    }
  }
  [3]=>
  array(4) {
    ["id"]=>
    int(3)
    ["name"]=>
    string(6) "资讯"
    ["pid"]=>
    int(0)
    ["type"]=>
    string(3) "top"
  }
}
```

费劲想了好久都没有写出来的东西就这么优雅的解决了.程序员的智慧真的是厉害啊.还有,不要重复造轮子,别人已经优雅的解决了的问题拿来用就好了(当然,不能做拿来主义者,还是要自己研究一下的),服务于项目.