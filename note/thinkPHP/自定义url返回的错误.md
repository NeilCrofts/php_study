<!--
 * @Author: your name
 * @Date: 2021-06-09 15:27:24
 * @LastEditTime: 2021-06-09 16:28:37
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \php_study\note\thinkPHP\自定义url返回的错误.md
-->


## 1.
在控制器，自定义url访问错误时返回前端或api的错误

首先修改`.env`文件内容：`APP_DEBUG = false`

### 1.1 访问url中的方法地址错误

在`app\BaseController`文件下定义

```php
 // url访问错误时调用 
    public function __call($name, $arguments)
    {
        //如果url访问的模块是api模块，应该返回api的数据格式
        // dump($name);
        // dump($arguments);
        $result = [
            "status" => 0,
            "message" => "找不到该方法",
            "result" => null
        ];
        return json($result,400);
    }
```

### 1.2 访问url中的控制器地址错误

在`app\controller`下添加`Error.php`文件

```php
<?php
namespace app\controller;

class Error
{
    public function __call($name, $arguments)
    {
        $result = [
            "status" => 0,
            "message" => "找不到控制器",
            "result" => null
        ];
        return json($result, 400);
    }
}
```

## 2.将返回错误的格式进行封装

在`app\common.php`文件下，该文件下的函数直接调用即可用

```php
<?php
// 应用公共文件
function show($status,$message = "error",$data = [],$httpStatus = 200){
    $result = [
        "status" => $status,
        "message" => $message,
        "result" => $data
    ];
    return json($result,$httpStatus);
}
```
则
```php
public function __call($name, $arguments)
    {
        return show(0,"找不到控制器",null,404);
    }
```

## 3.更进一步,将返回的错误改为业务状态码

在config文件下创建 status.php文件

```php
<?php
/*
 * 存放业务状态码相关配置
 */
return [
    "success"=>1,
    "error"=>0,
    "not_login"=>-1,
    "user_is_register"=>-2,
    "action_not_found"=>-3,
    "controller_not_found"=>-4 //若控制器url错误，则状态码为-4
];
```
则
```php
public function __call($name, $arguments)
    {
        return show(0,config("status.action_not_found"),null,404); // config("status.xxx")表示config/status文件
    }
}
```
输出：
```json
{
  "status": 0,
  "message": -3,
  "result": null
}
```