---
title: 如何发现代码的坏味道之SLAP原则
date: 2019-03-22 14:28:42
tags:
---


##  Long Method (代码的坏味道)

* 可读性很差
* 复用性差
* 难以调试
* 难以维护
* 冗余代码多

## SLAP(单一抽象原则)

### 定义
SLAP 是 Single Level of Abstraction 的缩写。解释：指定代码块上的代码应该在单一的抽象层上。

## SLAP优势
* 短方法的提取产生，会使得方法更加具有原子性，职责更加单一，更加的符合Unix的哲学 Do one thing, and do it well。
* 短方法的复用性更强，使得编码更加便捷
* 短方法可读性更强，更加便于理解
* 实践表明，SLAP应用后，维护成本应该是降低的。

## 举例

```
<?php 
  class User {
  	public static function getUserInfo() {
  		return [
  			'email' => '786188095@qq.com',
  			'password' => '232321312312'
  		];
  	}
  }

  function validateUser($user_info) {
  	$preg_email='/^[a-zA-Z0-9]+([-_.][a-zA-Z0-9]+)*@([a-zA-Z0-9]+[-.])+([a-z]{2,5})$/ims';
  	if (!preg_match($preg_email,$user_info['email'])) {
  		return false;
  	}

  	if (strlen($user_info['password']) <= 6) {
  		return false;
  	} 

  	//验证用户名 不包含特殊符号

  	//验证银行卡号

  	//验证银行卡开户行信息

  	//以此类推 方法越来越长  
  
  }

  validateUser(User::getUserInfo());
```

代码存在的问题

* validateUser 方法中暴露了校验email和密码的具体实现 
* validateUser 方法应该只关心校验结果（第一层抽象），而不是具体实现（第二层抽象）
* validateUser 违反了SLAP原则

修改后


```
<?php 
  class User {
  	public static function getUserInfo() {
  		return [
  			'email' => '786188095@qq.com',
  			'password' => '232321312312'
  		];
  	}
  }

  class UserValidator {
  	 protected static $preg_email = '/^[a-zA-Z0-9]+([-_.][a-zA-Z0-9]+)*@([a-zA-Z0-9]+[-.])+([a-z]{2,5})$/ims';

  	 public static function validateEmail($email) {
  	 	if (!preg_match(self::$preg_email,$email)) {
  		    return false;
  		}
  		return true;
  	 }

  	 public static function validatePassWord($password) {
  	 	if (strlen($password) <= 6) {
  			return false;
  		} 
  		return true;
  	 }
  }

  function vailUserInfo($user_info) {
  	return UserValidator::validateEmail($user_info['email']) && UserValidator::validatePassWord($user_info['password']);
  }


  var_dump(vailUserInfo(User::getUserInfo()));

```


