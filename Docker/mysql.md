## 搜索Mysql镜像, 可以不搜索
docker search mysql
## 拉最新镜像
docker pull mysql 
## 两种方法生成容器 docker运行镜像,创建一个MySQL实例容器, 映射外部端口号:内部端口号, 镜像名为 mysql 用户名为root 密码是 root
docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=root -d mysql
or
docker run -d --name mysqltest -p 3312:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

## 查看正在运行的镜像
docker ps 
## 进入容器 ,此mysql 是容器名称
docker exec -it mysql bash
## 进入MySQL
mysql -u root -p 回车
输入密码,回车
## navicat尝试连接mysql
报错:Authentication plugin ‘caching_sha2_password’ cannot be loaded
原因是mysql8 之前的版本中加密规则是mysql_native_password,而在mysql8之后,加密规则是caching_sha2_password

## 解决报错
## 1.修改加密规则
ALTER USER 'root'@'%' IDENTIFIED BY 'root' PASSWORD EXPIRE NEVER;
## 2.更新用户密码
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
## 3.刷新权限
flush privileges;

## 进入 DB
use mysql ;
select host,user,authentication_string,plugin from user;
展示root 所在行的 plugin 属性改为 mysql_native_password 即可
## 再次连接成功

## 启动容器
docker start mysql   //通过指定容器名字, 我的容器名字是mysql
docker start 73f8811f669e  //通过指定容器ID

## 停止容器
docker stop mysql   //通过指定容器名字, 我的容器名字是mysql
docker stop 73f8811f669e  //通过指定容器ID
