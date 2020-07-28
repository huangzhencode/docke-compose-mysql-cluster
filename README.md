# docke-compose-mysql-cluster
- 用于快速构建mysql主从(一主一从)
- 初始化了MySQL的连接数，查询缓存，SQL_MODE，UTF_8编码，最大包传输，全局cache
- 感谢 https://github.com/docker-box/mysql-cluster 的作者 

# 运行
1.自定义docker网络
```sh
docker network create hz-mysql --subnet 172.40.0.0/16
```

2.构建mysql主从
```sh
git clone https://github.com/huangzhencode/docker-compose-mysql-cluster.git
cd docke-compose-mysql-cluster/
./build.sh
```
> build.sh中可以自定义内容来对应docker-compose.yml和master和slave中的内容。
> 需要安装docker和docker-compose，另外本机需要有MySQL。

# 测试
1.给mysql_M1创建一个表并插入数据

```sh
docker exec mysql_M1 sh -c "export MYSQL_PWD=123456; mysql -u root cbdata -e 'create table code(code int); insert into code values (100), (200)'"
```

2.从mysql_S1中查询

```sh
docker exec mysql_S1 sh -c "export MYSQL_PWD=123456; mysql -u root cbdata -e 'select * from code \G'"
```
> 如果能查询出来，就说明主从配置生效

3.进入mysql_M1查看数据库配置是否生效
```sh
docker exec -it mysql_M1 bash
mysql -uroot -p123456

show variables like '%max_connections%';
show variables like 'character%';
show variables like '%max_allowed_packet%';
show variables like '%sql_mode%';
show variables like '%query_cache%';
```
