### 添加用户
```
CREATE USER 'harbour'@'%' IDENTIFIED BY 'harbour123.321';
GRANT ALL ON *.* TO 'harbour'@'%';
```

### 查看密码  
```
mysql>use mysql;
mysql>select user, host, plugin, authentication_string from user\G;
***************** 2. row *****************
user: root
host: %
plugin: caching_sha2_password 
```

### caching-sha2-password  
```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #修改加密规则 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'; #更新一下用户的密码 
FLUSH PRIVILEGES; #刷新权限 
再重置下密码：alter user 'root'@'localhost' identified by '123qwe';
```

### 创建用户 
create user 'test'@'localhost' identified by '123456';

### 分配权限  
grant all privileges on `testdb`.* to 'test'@'%';
