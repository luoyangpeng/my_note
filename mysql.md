# MYSQL
### 查询 id =xx 的所有下级
1.使用sql递归
``` mysql
select id from 
	( SELECT id,parent_id,
	@le:= IF (parent_id = 0 ,0,
	IF( LOCATE(CONCAT('|',parent_id,':'),@pathlevel) > 0,SUBSTRING_INDEX( SUBSTRING_INDEX(@pathlevel,CONCAT('|',parent_id,':'),-1),'|',1) +1,@le+1) ) levels,@pathlevel:= CONCAT(@pathlevel,'|',id,':', @le ,'|') pathlevel	,@pathnodes:= 
	IF( parent_id =0,',0',CONCAT_WS(',',
	IF( LOCATE( CONCAT('|',parent_id,':'),@pathall) > 0 , SUBSTRING_INDEX( SUBSTRING_INDEX(@pathall,CONCAT('|',parent_id,':'),-1),'|',1),@pathnodes ) ,parent_id ) )paths	,@pathall:=CONCAT(@pathall,'|',id,':', @pathnodes ,'|') pathall 
	FROM 表名称, 
	(SELECT @le:=0,@pathlevel:='', @pathall:='',@pathnodes:='') vv 
	ORDER BY parent_id,id ) src 
	where LOCATE(',查询的Id',paths) > 0 
	order by id
```

2. 使用mysql函数
``` mysql

DROP FUNCTION IF EXISTS queryDepartmentChildren;
DELIMITER |
CREATE FUNCTION `queryDepartmentChildren` (departmentId INT)
RETURNS VARCHAR(4000)
BEGIN
DECLARE sTemp VARCHAR(4000);
DECLARE sTempChd VARCHAR(4000);
 
SET sTemp = '$';
SET sTempChd = cast(departmentId as char);
 
WHILE sTempChd is not NULL DO
SET sTemp = CONCAT(sTemp,',',sTempChd);
SELECT group_concat(id) INTO sTempChd FROM department where FIND_IN_SET(parent_id,sTempChd);
END WHILE;
return sTemp;
END
|
DELIMITER ;

select queryDepartmentChildren(1) ; 

```



### MYSQL主从同步
一、主库
1.主库配置
```
# bind-address           = 127.0.0.1
server-id               = 1
log_bin                 = /var/log/mysql/mysql-bin.log
```
2.创建同步账户
```
grant replication slave on *.* to 'slave'@'%' identified by '123456';
```
3.查看maser信息
```
show master status;
```
二、从库
1.从库配置
```
server-id               = 2
```
2.执行同步命令，设置主数据库ip，同步帐号密码，同步位置
```
change master to master_host='192.168.1.2',master_user='slave',master_password='123456',master_log_file='mysql-bin.000001',master_log_pos=666;
```
3.开始

```
start slave;
```
4.查看从库状态
```
show slave status\G
#Slave_IO_Running 和 Slave_SQL_Running 值 都为 Yes  说明就成功了
```


### 计算两个经纬度的距离
```
DROP FUNCTION IF EXISTS `getDistance`;
DELIMITER $$
CREATE FUNCTION `getDistance`(lat1 double,lng1 double,lat2 double,lng2 double) 
RETURNS double
BEGIN
IF(LENGTH(lat1) =0 || LENGTH(lng1) =0 || LENGTH(lat2) =0 || LENGTH(lng2) =0 || lat1 IS NULL|| lng1 IS NULL|| lat2 IS NULL|| lng2 IS NULL) THEN
      RETURN -1;
    END IF;
	return round(6378.138*2*asin(sqrt(pow(sin( (lat1*pi()/180-lat2*pi()/180)/2),2)+cos(lat1*pi()/180)*cos(lat2*pi()/180)* pow(sin( (lng1*pi()/180-lng2*pi()/180)/2),2)))*1000);
END $$
DELIMITER ;
```
