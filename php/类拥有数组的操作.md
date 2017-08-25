
#### ArrayAccess 让对象像数组一样赋值、读取、删除
```
interface ArrayAccess {  
    function offsetExists($key);  //被isset调用时  
    function offsetGet($key);  //被当作数组读时  
    function offsetSet($key, $value);  //被当作数组赋值时  
    function offsetUnset($key);  //被当作数组unset时  
}
```
#### Iterator 让对象像数组一样循环

```
IteratorAggregate extends Traversable {
    abstract public Traversable getIterator ( void )
}
```

#### 完美的数组类

```
<?php  
/** 
 * 测试接口的使用 
 *  
 * @author zane 
 * @since 2017-08-24 
 */  
  
  
class arrayTest implements ArrayAccess, IteratorAggregate  
{  
    protected $d = [];  
    protected $currIndex = 0;  
    protected $all = 0;  
    protected $more = false;  
    
    public function __construct($array)
    {  
        $this->d = $array;  
        $this->currIndex = 0;  
        $this->all = count($this->d);  
        $this->more = ($this->all > 0);  
    }  
    
    public function offsetExists($offset)
    {  
        echo __METHOD__."\n";  
        return array_key_exists($offset, $this->d);  
    } 
    
    public function offsetGet($offset)
    {  
        echo __METHOD__."\n";  
        return $this->d[$offset];  
    }  
    
    public function offsetSet($offset, $value)
    {  
        echo __METHOD__."\n";  
        $this->d[$offset] = $value;  
    }  
    
    public function offsetUnset($offset)
    {  
        echo __METHOD__."\n";  
        unset($this->d[$offset]);  
        return true;  
    } 
    
    public function getIterator() {
        return new ArrayIterator($this);
    }
}  
  
$t = new arrayTest(['a' => 'A', 'b' => 'B', 'c' => 'C']);  
  
echo $t['a'];  
  
foreach($t as $key => $item){  
    echo "{$key} => {$item} \n";  
}    
```
