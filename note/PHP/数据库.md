## Database 的连接

### 1. 最简单
```php
<php?
    $dsn = 'mysql:host=localhost;port=3306;dbname=blog;',
    $user = 'root';
    $password = 'root';
    // 1.建立连接
    $pdo = new PDO($dsn,$user,$password);
    // 2.写sql
    $sql = "SELECT * FROM hero limit 2,10";
    // 3.执行
    $stm = $pdo->prepare($sql);
    $stm->execute(); // 返回布尔值
    
    //与以上两步效果相同
    // $stm = $pdo->query($sql);

    // 4.处理结果
    $data = $stm->fetchAll();
    // 5.关闭 运行完成,在此关闭连接,PHP在脚本结束时也会自动关闭连接
    $pdo = null;
?>
```
 捕获异常：如果有任何连接错误，将抛出一个 PDOException 异常对象,可以捕获异常。如果不捕获异常，可能会泄漏完整的数据库连接细节
 ```php
<php?
 try{
    $pdo = new PDO($dsn,$user,$password);
    $sql = "SELECT * FROM hero";
    $stm = $pdo->query($sql);
    $dbh = null;
    }catch(PDOException $e){
        print "Error!: " . $e->getMessage() . "<br/>";
        die();
    }
?>
```


### 拓展：添加命名占位符
```php
<php?
// 当地址栏含?id=-1 or 1=1 时，会把数据库所有数据请求出来，因此为防止数据库泄露，用占位符占据位置
        
    //1
      $sql = 'SELECT * FROM message WHERE id = ?';
      $sth->execute([$id]);

     //2
      $sql = 'SELECT * FROM message WHERE id = :id';
      $sth->bindValue(':id',$id); // :id为key,$id为value

     // 命名占位符第二种绑定方式
 
        $sql = 'UPDATE message SET username = ? WHERE id = ?';
        $sth = $pdo->prepare($sql);

        $username = "kiki";
        $id = 1;
        $sth->bindValue(1,$username);
        $sth->bindValue(2,$id);
        
        $sth->execute();


?>
```