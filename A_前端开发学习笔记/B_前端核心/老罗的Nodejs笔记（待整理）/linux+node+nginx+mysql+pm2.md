
 ssh-keygen -R [your domain name or IP address]


https://blog.csdn.net/qq_42815754/article/details/82980326   nginx

https://www.cnblogs.com/huangenai/p/10815426.html  node

https://www.jb51.net/article/190714.htm

MySQL

https://www.cnblogs.com/panbingwen/p/11664175.html  pm2

远程连接
再次用户root 用户登录 输入自己刚刚修改的密码就可以了

```
use mysql;
update user set host = '%' where user ='root'; 
ALTER USER 'root'@'%' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #更改加密方式
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '自己的密码'; #更新用户密码 （我这里为root ）
```