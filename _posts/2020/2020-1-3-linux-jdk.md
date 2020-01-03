------- 
title: Mysql常用基础操作
------- 

# 前言
 持续学习技能
 - 研习理论：看第一手资料(看书，看官网，看源码)
 - 动手实践: 自己实现一边(验证课本知识，实践官网文档，分析源代码，画出类结构图，生命周期图，设计模式，数据结构算法，理论思想)
 - 记录笔记: 实践代码，文档解说，知识验证思路与步骤，对源代码的分析思路与结果
 - 反复回顾: 持续关注官网，有机会就看看源码，反复回顾自己的笔记，加深对知识的理解

# 目标
- ## 1.记录语法结构，以及常用函数
- ## 2.写出常用例子
- ## 3.对函数参数，算法思想都要写注释
# 数据库基础操作
## 根据函数名成查看函数
SHOW function STATUS LIKE 'find_and_concact';
# 数据类型
varchar
text
longtext
longblob
tinyint
smallint
int
integer
bigint
numeric
enum
double
set
binary
varbinary

# 基础语法
# 增删改查
## 表结构的创建 
-- user_name 子
-- agent_name 父
-- agents 树形结构扁平化
 ```shell
create table tg_member(id BIGINT not null PRIMARY key ,user_name VARCHAR ,agent_name VARCHAR,agents VARCHAR(3000));
-- 插入测试数据
insert into tg_member values(1L,'def','abc','abc.def');
insert into tg_member values(2L,'123','','123');
insert into tg_member values(3L,'456','123','123.456');
insert into tg_member values(4L,'789','456','123.456.789');
commit;
```
# 表关系
# 常用函数
# 动态SQL
# 系统表结构
# 存储过程
## 计算1-100的总和是多少？ 
```
-- 存储过程的创建
drop PROCEDURE if exists sum1;
create PROCEDURE sum1(a int) 
begin
    declare sum int default 0;  -- default 是指定该变量的默认值
    declare i int default 1;
while i<=a DO -- 循环开始
    set sum=sum+i;
    set i=i+1;
end while; -- 循环结束
select sum;  -- 输出结果
end;
```
# 自定义函数


## 查询父子结构的数据表（树结构）
```shell
-- 函数的创建
drop function if exists find_and_concact;
create function find_and_concact(param_user_name VARCHAR(255)) 
returns VARCHAR(3000)
begin
    declare v_agents VARCHAR(3000) default '';  -- default 是指定该变量的默认值
    declare v_agent_name VARCHAR(255) default '';
		set v_agents =param_user_name; -- 也可以写成 set v_agents :=param_user_name; := = 通常是等价的
    select agent_name into v_agent_name from tg_member t where t.user_name = param_user_name;
		while v_agent_name is not null and v_agent_name != '' DO -- 循环开始
				 set v_agents=CONCAT_WS('.',v_agent_name,v_agents);
				 set @vagent_name = v_agent_name;
				 set v_agent_name = '';
				 select agent_name into v_agent_name from tg_member t where t.user_name = @vagent_name;
		end while; -- 循环结束
return v_agents;  -- 输出结果
end;
-- 结束
commit;
```
# 视图
# 游标
# 定时任务
# 正则表达式
# 事务
# ACID
# 数据的备份
# 分库分表
# 数据库引擎
# 数据结构
# 性能监控
# 性能优化
