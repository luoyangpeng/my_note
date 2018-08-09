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
CREATE FUNCTION `queryBranchChildren` (departmentId INT)
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
