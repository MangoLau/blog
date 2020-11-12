## PHP实现链式操作

**php链式操作的关键是在做完操作后要return $this；**
## 一、不使用__call方法实现链式操作
```php
<?php
class Sql
{
    private $sql = array(
        "from"=>"",
        "where"=>"",
        "order"=>"",
        "limit"=>""
    );

    public function from($tableName) {
        $this->sql["from"] = "FROM ".$tableName;
        return $this;
    }

    public function where($_where = '1=1')
    {
        $this->sql["where"] = "WHERE ".$_where;
        return $this;
    }

    public function order($_order = 'id DESC')
    {
        $this->sql["order"] = "ORDER BY ".$_order;
        return $this;
    }

    public function limit($_limit = '30')
    {
        $this->sql["limit"] = "LIMIT 0,".$_limit;
        return $this;
    }
    public function select($_select = '*')
    {
        return "SELECT ".$_select." ".(implode(" ",$this->sql));
    }
}

$sql = new Sql();

echo $sql->from("testTable")->where("id=1")->order("id DESC")->limit(10)->select();
//输出 SELECT * FROM testTable WHERE id=1 ORDER BY id DESC LIMIT 0,10
?>
```

## 二、使用__call方法实现链式操作
__call()在对象调用一个不可访问的方法时会被触发，所以可以实现类的动态方法的创建，实现php的方法重载功能，但它其实是一个语法糖（__construct()方法也是）。
```php
<?php
class String
{
    public $value;

    public function __construct($str = null)
    {
        $this->value = $str;
    }

    public function __call($name, $args)
    {
        $this->value = call_user_func($name, $this->value, $args[0]);
        return $this;
    }

    public function strlen()
    {
        return strlen($this->value);
    }
}
$str = new String('01389');
echo $str->trim('0')->strlen();
// 输出结果为 4；trim('0')后$str为"1389"
?>
```