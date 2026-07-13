---
title: database-03-SQL介绍
published: 2026-03-25
updated: 2026-03-25
description: "lue"
image: ./images/ly3.png
tags: [database,base,class]
category: database
draft: false
---

<!--{"show":{"hx4no":6,"ident4lvls":0}}-->
<style>
sup>code{background-color:rgb(var(--ok0-fg-,0,144,0)) !important;color:rgb(var(--ok0-bg-,208,255,208)) !important}
[x-bnf]{/*语法定义*/
	line-height:1.5;tab-size:2em;white-space:pre;
	color:rgb(var(--warn0-fg-,255,128,0));font-weight:bold;
}
[x-bnf] kbd{font-weight:bold;color:rgb(var(--ask0-fg-,0,128,255));font-family:consolas}
[x-bnf] var{color:rgb(var(--ask0-fg-,0,128,255));font-weight:normal}
[x-bnf] var::before,[x-bnf] var::after, [x-bnf] b{
	font-style:normal;color:rgba(var(--box-txt-,128,128,128),.7);font-weight:bold;
}
[x-bnf] var::before{content:"<"}
[x-bnf] var::after{content:">"}
[x-bnf] dfn{font-style:normal}
[x-bnf] dfn::before{content:"["attr(hd);color:rgba(var(--box-txt-,128,128,128),.5)}
[x-bnf] dfn::after{content:attr(ft)"]";color:rgba(var(--box-txt-,128,128,128),.5)}
[x-bnf] u{
	text-decoration:none;display:inline-block;border:.12em dotted #f80;height:.8em;width:.5em;
	margin:0 .12em .12em .12em;vertical-align:middle;line-height:1;box-sizing:border-box}
[x-bnf] [md-ok]{color:rgb(var(--ok0-fg-,0,164,0));font-weight:normal}
[s-tree="md-tree"] [x-bnf]>bnf-left, [s-tree="md-tree"] [x-bnf] [align-left]{margin-left:-1em}

.katex{font-size:150%;color:rgb(var(--ask0-fg-,0,128,255))}
table>tbody>tr>td>em>strong{background:#ffff00;color:red}
[md-frames] table{
	border-collapse:collapse;color:inherit;line-height:1.5;font-size:inherit;
	border:0 solid rgba(var(--box-txt-),.8);border-width:2px;
}
[md-frames] table>caption, [my-math]{/*标题/脚注*/
	font:italic 1.5em/2 "Times New Roman";color:rgb(var(--ask0-fg-,0,88,204))
}
[md-frames] table>thead{/*表头*/
	border-bottom:2px solid rgba(var(--box-txt-),.8);
	background:rgba(var(--box-txt-),.15);
}
[md-frames] table th,
[md-frames] table td{/*单元格*/
	border:1px solid rgba(var(--box-txt-),.5);padding:.25em .5em;
}
</style>
# <sub>数据库系统原理</sub><br>SQL介绍<svg viewBox="0 -5.5 26 26" style="fill:rgba(var(--err0-fg-,255,0,0),.7);height:1em;transform:rotate(-20deg) translate(-.9em,.6em);position:absolute"><path d="M2 2h1v3H2zM2 6h1v4H2zM6 5h2v1H6zM6 7h2v1H6zM9 7h2v1H9zM9 5h2v1H9z"/><path d="M13 0H0v15h13V9h1v6h1V0h-2zM4 2v9H2v3H1V1h3zm3 11v1H5v-1h1v-3h1zm5 1h-2v-4h1v3h1zm0-11H9v1h3v5H9v5H8V9H5V4h3V3H5V2h3V1h1v1h3zm2 5h-1V5h1zm0-4h-1V1h1zM17 0h-1v15h1V9h1v6h1V0h-2zm1 8h-1V5h1zm0-4h-1V1h1zM25 10h-4V9h4V8h-4V7h4V2h-4V1h4V0h-5v11h5v3h-4v-1h3v-1h-4v3h6v-5zm-4-7h3v1h-3zm0 2h3v1h-3z"/></svg><br><code style="font-size:.5em">2025</code>
<!--{"show":{"listSub":0,"sub":"--li1-size:1.2"}}-->
## SQL查询语言概览<sup>`§3.1`</sup>
+ Sequel language 是IBM圣何塞研究实验室R系统项目的一部分
	+ 后更名： Structured Query Language (`SQL`)
+ ANSI 和 ISO 标准SQL:
	+ SQL-86, SQL-89, SQL-92 
	+ SQL:1999, SQL:2003, SQL:2008,SQL:2011,SQL:2016
+ 一般商业数据库
	+ 支持SQL-92多数特性+部分高版标准特性+部分非标扩展特性
	+ 不是所有的例子都能运行在某个特定的系统上
+ `DDL` – Data Definition language.
	+ `integrity`(`完整性`) – DDL 包含用于定义完整性约束的功能.
	+ `View definition`(`视图定义`) – DDL能够定义视图.
	+ `Authorization`(`授权`) – 能够定义视图和关系的访问权限
+ `DML` – 查询、插入、删除、修改
	+ `Transaction control`(`事务控制`) – 控制事务的开始和结束.
	+ `Embedded SQL`(`嵌入SQL`) 和 `Dynamic SQL`(`动态SQL`) - 定义如何将 SQL 语句嵌入到通用编程语言中
### SQL的语言特性<sup>`§3.1.0`</sup>
+ 综合统一
	+ 非关系模型数据语言
		+ 以下语言分别定义
			+ 模式DDL（Schema Data Definition Language）
			+ 外模式DDL（Subschema Data Definition Language）
			+ DSDL（Data Storage Description Language）
			+ DML（Data Manipulation Language）
		+ 如需修改模式，必须停止当前DB运行、备份、修改、编译、重装数据
	+ SQL语言
		+ 集DDL、DML、DCL于一体
		+ 有能力运行中修改模式
		+ 数据结构的单一带来数据操作符的简化
+ 高度非过程化
	+ 不再关心存取路径
+ 面向集合的操作方式
	+ 不再说明具体处理过程（如存取路径、如何循环处理）
+ 一种语法两种用途
	+ 自含式语言
	+ 嵌入式语言
+ 语言简洁、易学易用
	+ 查询：`SELECT`
	+ 定义：`CREATE`, `DROP`, `ALTER`
	+ 操纵：`INSERT`, `UPDATE`, `DELETE`
	+ 控制：`GRANT`, `REVOKE`
### SQL的语法特性<sup>`§3.1.0`</sup>
+ 大小写<b md-most>不</b>敏感
	关键词、表名(关系名)、字段名(属性名)、内建/自定义函数或过程名
+ 以 **`;`** 作为语句`分隔符`
	```warning
	注意："分隔符"不是"结束符"
	```
<!--{"show":{"listSub":0,"sub":"--li1-size:1.2;--li2-size:1.1;--md-line-height:1.8"}}-->
## SQL数据定义<sup>`§3.2`</sup>
SQL数据定义语言`data-definition language`(`DDL`)可定义关系的如下特性:
+ 每个`关系`（`relation`）的`模式`（`schema`）
+ 每个属性的`值域`(`domain`)
+ `完整性约束`/`Integrity constraints` 
+ 以及后面会讲到的： 
	+ 关系的`索引`定义(`indices`, `index`)
	+ 每个关系的`安全与权限设定`(Security and Authorization)
	+ 关系的`物理存储结构`(physical storage structure)
<!--{"show":{"sub":"--li1-size:1.15;--md-line-height:1.8"}}-->
### SQL中的基本(数据)类型<sup>`§3.2.1`</sup>
+ `char(n)`定长串，不足充空
+ `varchar(n)`变长串，不足截断
+ `int` 整数(精度长度和具体机器相关，4字节居多)
+ `smallint`小整数(机器相关，2字节居多)
+ `numeric(p,d)`定点数，用户指定精度为p位，小数点右侧为d位
+ `real`,`double`浮点数和双精度浮点数(精度长度和具体机器相关)
+ `float(n)` 浮点数，用户指定的精度至少为n位
+ Chapter 4 会涉及更多类型
### 基本模式(表)定义 - Create Table<sup>`§3.2.2`</sup>
+ `create table`语法
	<div x-bnf><kbd>CREATE TABLE</kbd> <var
	>表名</var> (
		<var>字段名</var> <var>数据类型</var><dfn ft="..."><u></u><var>列级约束条件</var></dfn>
		<dfn ft="...">,<var>字段名</var> <var>数据类型</var><dfn ft="..."><u></u><var>列级约束条件</var></dfn></dfn>
		<dfn ft="...">,<var>表级约束条件</var></dfn>
	<bnf-left>)</bnf-left></div>
+ 例
	```sql
	CREATE TABLE Cou( --课程表
		Cno int primary key, /*课程号*/
		Cname varchar(16) not null, --课程名
		Cpno int, --先导课程号
		Ccredit int not null, --学分
		foreign key (Cpno) references Cou(cno) --外码约束条件
	)
	```
### 完整性约束条件<sup>`§3.2.2`</sup>
+ <b x-bnf><var>列级约束条件</var><b>::=</b></b>
	<b x-bnf><kbd>null<b>|</b>not null<b>|</b>unique<b>|</b>primay key<b>|</b>references</kbd> <var>表名</var><dfn>(<var>字段名</var>)</dfn></b>
<!--{"show":{"sub":"font-size:.87em"}}-->
+ <b x-bnf><var>表级约束条件</var><b>::=</b></b>
	<div x-bnf><kbd>unqiue</kbd>(<var>字段名</var><dfn  ft="...">,<var>字段名</var></dfn>)
	<bnf-left><b>|</b><kbd>primary key</kbd>(<var>字段名</var><dfn  ft="...">,<var>字段名</var></dfn>)</bnf-left>
	<bnf-left><b>|</b><kbd>foreign key</kbd>(<var>字段名</var><dfn  ft="...">,<var>字段名</var></dfn>) <kbd>references</kbd> <var>表名</var>(<var>字段名</var><dfn  ft="...">,<var>字段名</var></dfn>)</bnf-left></div>
+ 例
	```sql
	Create Table Tea( --教师表
		tno int primary key, --工号
		tname varchar(16) not null, --姓名
		phone varchar(13) unique --联系电话
	);
	CREATE TABLE TC( --教师可讲学科表
		tno int references Tea, --教师工号
		cno int references Cou, --课程号
		primary key(tno,cno)
	);
	create table CouHis( --开课记录表
		at_year int, --开课学年
		cno int references Cou, --课程号
		serialNo int, --课序号
		tno int not null references Tea, --主讲老师工号
		weekDay int not null, --星期几
		secNo int not null, --第几大节
		unique(at_year,cno,weekDay,secNo,tno),
		primary key(at_year,cno,serialNo),
		foreign key(tno,cno) references TC
	)
	```
```tip
💡大多数数据库的 primary key 隐含 not null 约束
😈SQLite是例外，它要求 not null 必须显式标出
```
### 修改表定义(模式)<sup>`§3.2.2`</sup>
+ 语法:<b x-bnf><kbd>ALTER TABLE</kbd> <var>表名</var> <var>修改指令</var><dfn ft="...">,<var>修改指令</var></dfn></b>
	<div x-bnf><var>修改指令</var><b>::=</b>
	<kbd>ADD <dfn>COLUMN</dfn></kbd> <var>字段名</var> <var>数据类型</var><dfn ft="..."><u></u><var>列级约束条件</var></dfn>
	<b>|</b><kbd>DROP <dfn>COLUMN</dfn></kbd> <var>字段名</var>
	<b>|</b><kbd>ALTER COLUMN</kbd> <var>字段名</var> <dfn><var>新字段名</var></dfn> <var>数据类型</var><dfn ft="..."><u></u><var>列级约束条件</var></dfn>
	<b>|</b><kbd>DROP CONSTRAINT</kbd> <var>约束名称</var>
	<b>|</b><kbd>ADD CONSTRAINT</kbd> <dfn><var>约束名称</var></dfn> <var>表级约束条件</var>
	</div>
+ 例
	```sql
	ALTER TABLE Tea add gender char(2) not null;
	--上方SQL如表中已有数据，会执行失败
	ALTER TABLE Tea add gender char(2) not null default '男';
	--上方SQL成功
	```
	```warning
	🚨SQL中的字符串常量应使用单引号 ' 而不是双引号 "
	🚨许多数据库不支持删除已有字段，不支持操作约束
	🚨各版本数据库 ALTER TABLE 语法实现不够规范，注意查看数据库手册
	```
<!--{"show":{"listSub":0}}-->
## SQL查询基本语法<sup>`§3.3`</sup>
+ `select from`语法
	<div x-bnf><kbd>SELECT <dfn>ALL<b>|</b>DISTINCT</kbd></dfn> <var>目标列</var><dfn ft="...">,<var>目标列</var></dfn>
	<kbd align-left>FROM</kbd> <var>表名<b>|</b>视图名<b>|</b>子查询</var><dfn ft="...">,<var>表名<b>|</b>视图名<b>|</b>子查询</var></dfn>
	<dfn align-left><kbd>WHERE</kbd> <var>条件表达式</var></dfn>
	<dfn align-left><kbd>GROUP BY</kbd> <var>字段名</var><dfn ft="...">,<var>字段名</var></dfn><dfn><u></u><kbd>HAVING</kbd> <var>条件表达式</var></dfn></dfn>
	<dfn align-left><kbd>ORDER BY</kbd> <var>字段名</var><dfn><u></u><kbd>ASC<b>|</b>DESC</kbd></dfn><dfn ft="...">,<var>字段名</var><dfn><u></u><kbd>ASC<b>|</b>DESC</kbd></dfn></dfn></dfn></div>
<!--{"show":{"sub":"--li2-size:1em"}}-->
+ 含义  
	+ 根据`WHERE`子句的条件表达式，从`FROM`子句指定的基本表或视图中找出满足条件的元组
	+ 按`SELECT`子句中的目标列(表达式)，选出元组中的属性值形成结果表
	+ 如有`GROUP BY`子句，则结果按`<字段名>`的值进行分组(该分量相等的元组为一个组)，每组产生结果表中的一条记录(一般会在每组中作用集函数)
	+ 如果GROUP子句带`HAVING`短语，则只有满足指定条件的组才予输出
	+ 如有`ORDER BY`子句，则结果表要按`<字段名>`分量值的升序或降序排序
+ 要求掌握2～3层的嵌套查询
<!--{"show":{"shrinkRng":[1,1],"listSub":0}}-->
### select子句<sup>`§3.3.1`,`§3.4.1`</sup>
<div x-bnf><kbd>SELECT <dfn>ALL<b>|</b>DISTINCT</kbd></dfn> <var>目标列</var><dfn ft="...">,<var>目标列</var></dfn>
	<var>目标列</var><b>::=</b><var>字段名</var><dfn><u></u><kbd>AS</kbd> <var>别名</var></dfn><b>|</b><var>计算表达式</var><dfn><u></u><kbd>AS</kbd> <var>别名</var></dfn>
</div>

select子句列出查询结果希望获得的属性，对应关系代数中的投影(**`π`**)
```sql
--指定目标字段
select Sname,Sage from Stu;
```
```sql
--获取全部字段
select * from Stu;
```
```sql
--计算表达式
select Sname,2025-Sage,'乖学生' from Stu;
```
```sql
--更名(别名)
select Sname as 姓名, 2025-Sage as 出生年, '乖学生' as 属性
from Stu;
```
```sql
--更名(别名)
select Sname as 姓名, strftime('%Y','now')-Sage as 出生年
from Stu;
```
+ <span x-bnf><dfn><kbd>ALL<b>|</b>DISTINCT</kbd></dfn></span>  
	SQL允许查询结果中有重复行
	```sql
	select dept from Stu; --返回所有行，包括重复行
	select ALL dept from Stu; --同上，ALL是默认值
	select DISTINCT dept from Stu; --去重
	select DISTINCT dept,Sname from Stu; --DISTINCT的作用范围
	```
<!--{"show":{"shrinkRng":[1,1]}}-->
#### 字符串目标列补充<sup>`§3.3.1.0`</sup>
```sql
-- 字符串连接
select sname || '-' || ssex || sage from Stu; --使用连接符
select concat(sname,'-',ssex,sage) from Stu; --使用内建函数
```
```sql
-- 大小写转换，获取字符串长度，抽取子字符串等内建函数
select sname,lower(dept),upper(dept) from Stu; --大小写转换
select cname,length(cname) from Cou; --获取字符串长度
select cname,substr(cname,2,3) from Cou; --抽取子字符串
```
```warning
🚨内建函数并无统一标准，不同数据库需注意查手册
```
<!--{"show":{"listSub":0,"startStep":1}}-->
### where子句<sup>`§3.3` `§3.4`</sup>
<div x-bnf><dfn><kbd>WHERE</kbd> <var>条件表达式</var></dfn>
	<var align-left>条件表达式</var><b>::=</b><var>逻辑运算</var><dfn ft="..."><u></u><var>逻辑连接符</var> <dfn>(</dfn><dfn><kbd>NOT</kbd><u></u></dfn><var>条件表达式</var><dfn>)</dfn></dfn>
	<var align-left>逻辑连接符</var><b>::=</b><kbd>AND</kbd><b>|</b><kbd>OR</kbd>
	<var align-left>逻辑运算</var><b>::=</b><dfn><kbd>NOT</kbd><u></u></dfn>
		<dfn>(</dfn><var>比较运算</var><b>|</b><var>范围判断</var><b>|</b><var>集合成员</var><b>|</b><var>字符串匹配</var><b>|</b><var>空值判断</var><dfn>)</dfn>
</div><br>

where子句查询满足条件的元组，对应关系代数中的选择(**`σ`**)
<!--{"show":{"shrinkRng":[1,1]}}-->
#### <b x-bnf><var>比较运算</var></b><sup>`§3.3.1`</sup>
<div x-bnf><var>比较运算</var><b>::=</b><var>目标列</var><var>比较运算符</var><var>目标列</var>
	<var>比较运算符</var><b>::=</b> &gt;<b>|</b>&lt;<b>|</b>=<b>|</b>&lt;=<b>|</b>&gt;=<b>|</b>&lt;&gt;<b>|</b>!=<b>|</b>!&gt;<b>|</b>!&lt;
</div>

```sql
select * from Stu where sage<20;
select * from Stu where sage<20 AND ssex='女';
```
```sql
select * from Stu where NOT sage<20 AND ssex='女';
select * from Stu where NOT(sage<20 AND ssex='女');
```
```warning
🚨比较运算符: >,<,>=,<=,!>,!< 要求<目标列>的值是能够比较大小的(值是有序的)
```
#### <b x-bnf><var>范围运算</var></b><sup>`§3.4.5`</sup>
<div x-bnf><var>范围运算</var><b>::=</b><var>目标列</var><dfn><u></u><kbd>NOT</kbd></dfn> <kbd>BETWEEN</kbd> <var>目标列</var> <kbd>AND</kbd> <var>目标列</var>
</div>

```sql
select * from SC where grade BETWEEN 80 AND 90;
select * from SC where grade>=80 AND grade<=90; --等价
```
```warning
🚨范围判断运算符要求<目标列>的值是有序的
🚨范围区间是闭区间
```
#### <b x-bnf><var>集合成员</var></b><sup>`§3.8.1`</sup>
<div x-bnf><var>集合成员</var><b>::=</b><var>目标列</var><dfn><u></u><kbd>NOT</kbd></dfn> <kbd>IN</kbd>(<var>值</var><dfn ft="...">,<var>值</var></dfn><b>|</b><var>子查询</var>)
	<var>值</var><b>::= 和</b><var>目标列</var><b>同一个域的任何值</b>
</div>

```sql
select * from Stu where dept IN('CS','IS');
```
#### 字符串比较<sup>`§3.4.2`</sup>
+ `=` `<` `>` `<=` `>=` `<>` 都可以用来做字符串比较
	```sql
	select * from Cou where cname<='数据库';
	```
+ 字符串的有序性问题
	+ 字符串的大小比较实质上是字符 **`编码`** 值的比较，字符集编码方案不止一个，所以……🙈
	+ 数据库能够支持的编码方案也不止一种，所以……😰
```warning
🚨字符串的比较默认是`大小写敏感`的
```
```tip
💡很多数据库在字符串常量中使用连续的两个单引号表示一个单引号
```
<!--{"show":{"shrinkRng":[1,1],"sub":"--li1-size:1.15em"}}-->
#### <b x-bnf><var>字符串匹配</var></b><sup>`§3.4.2`</sup>
<div x-bnf><var>字符串匹配</var><b>::=</b><var>目标列1</var><dfn><u></u><kbd>NOT</kbd></dfn> <kbd>LIKE</kbd> <var>目标列2</var>
</div>
<i x-bnf><var>目标列2</var></i>一般是字符串常量，可使用通配符进行字符串匹配

+ <code><b md-most>%</b></code>：匹配0到多个任意字符; <code><b md-most>_</b></code>：匹配1个任意字符
```sql
select * from Stu where sname LIKE '李%';
/* 示例改名
update Stu set sname='李刘晨' where sno='95002';
update Stu set sname='张王敏' where sno='95003'; -- */
```
```sql
insert into Stu values('96001','李%','男',17,'CS');
select * from Stu where sname LIKE '李\%' ESCAPE '\';
--select * from Stu where sname='李%';
```
```warning
🚨字符串匹配运算符要求<目标列>的值是字符串
🚨字符串的匹配默认是`大小写敏感`的
```
#### <b x-bnf><var>空值判断</var></b><sup>`§3.6`</sup>
<div x-bnf><var>空值判断</var><b>::=</b><var>目标列</var> <kbd>IS</kbd><dfn><u></u><kbd>NOT</kbd></dfn> <kbd>NULL</kbd></div>

```sql
select * from Cou where cpno IS NULL;
```
```notice
有的系统允许使用＝和<>进行NULL比较完成空值判断，但不推荐
```
<!--{"show":{"listSub":0,"startStep":1,"sub":"--li1-size:1;--md-line-height:1.7"}}-->
### from子句与多关系查询<sup>`§3.3.1`~`§3.3.2`</sup>
<div x-bnf><dfn><kbd>FROM</kbd> <var>表名<b>|</b>视图名<b>|</b>子查询</var><dfn ft="...">,<var>表名<b>|</b>视图名<b>|</b>子查询</var></dfn></div>

from子句指明查询涉及到的表(关系)，**概念上**对应关系代数中的笛卡尔积(**`×`**)
+ 只涉及到一张表时，就是单关系查询
+ 涉及到的表(关系)不止一个即多关系查询
```sql
select Sname,Cname from Stu,Cou;
```
```tip
笛卡尔积直接使用一般没有意义，通常和`where`配合使用构成连接
```
#### from子句中表的别名
语法: <span x-bnf><kbd>from</kbd> <var>原表名</var><dfn><u></u><kbd>as</kbd></dfn><u></u><var>别名</var></span>
```sql
select S.sno,sname,cname,grade
from Stu AS S,SC,Cou AS C
where S.sno=SC.sno and C.cno=SC.cno;
```
```warning
💡有些数据库(比如Oracle)只接受省略关键词`AS`的别名语法
```
<!--{"show":{"shrinkRng":[1,1]}}-->
#### 多关系查询中的命名冲突
```sql
/* 反派黑料 */
select sno,sname,cname,grade
from Stu,SC,Cou
where sno=sno and cno=cno;
```
```sql
/* 这才是正面角色 */
select SC.sno,sname,cname,grade
from Stu,SC,Cou
where SC.sno=Stu.sno and SC.cno=Cou.cno;
```
```sql
/* '*'之领域和别名的妙用 */
select Cou.*,CP.cname as 先导课
from Cou,Cou as CP
where Cou.cpno=CP.cno;
```
```tip
💡实际项目中增加和修改字段时，应避免非必要的重复名称
```
### 简单SQL查询与关系代数的联系<sup>`§3.3.2`</sup>
```sql
SELECT A1, A2, ..., An
FROM T1, T2, ..., Tm
WHERE F;
```
$\Pi_{A1,A2,...An}\sigma_F(T1 \times T2 \times ... \times Tm)$
+ 简单SQL查询的逻辑
	1. 通过`from`子句列出的表(关系)得到笛卡尔积
	2. 使用`where`子句给出的条件筛选步骤1的结果
	3. 对步骤2得到结果中的每一条元组，根据`select`子句指明的属性或计算表达式输出
### order by子句 - 结果排序<sup>`§3.4.4`</sup>
<div x-bnf><dfn><kbd>ORDER BY</kbd> <var>字段名</var><dfn><u></u><kbd>ASC<b>|</b>DESC</kbd></dfn><dfn ft="...">,<var>字段名</var><dfn><u></u><kbd>ASC<b>|</b>DESC</kbd></dfn></dfn></dfn>
</div>

order by子句使输出关系按`<字段名>`分量值的升序或降序排序  
+ <code><b md-most>ASC</b></code> - 升序; <code><b md-most>DESC</b></code> - 降序
```sql
select * from Stu ORDER BY sage; --升序(从小到大)
select * from Stu ORDER BY sage ASC; --同上，ASC是默认值
select * from Stu ORDER BY sage ASC,ssex DESC; --多属性组合排序
```
### 聚集函数/Aggregate Functions<sup>`§3.7.1`</sup>
<i x-bnf><var>目标列</var></i>中一种特殊的<i x-bnf><var>计算表达式</var></i>
<div x-bnf><var>聚集函数表达式</var><b>::=</b>
	<kbd>SUM</kbd>(<dfn>all<u></u><b>|</b>distinct<u></u></dfn><var>字段名</var>) <i md-ok>--总和(需数值集合)</i>
	<b>|</b><kbd>AVG</kbd>(<dfn>all<u></u><b>|</b>distinct<u></u></dfn><var>字段名</var>) <i md-ok>--平均值(需数值)</i>
	<b>|</b><kbd>MAX</kbd>(<dfn>all<u></u><b>|</b>distinct<u></u></dfn><var>字段名</var>) <i md-ok>--最大值(需有序值)</i>
	<b>|</b><kbd>MIN</kbd>(<dfn>all<u></u><b>|</b>distinct<u></u></dfn><var>字段名</var>) <i md-ok>--最小值(需有序值)</i>
	<b>|</b><kbd>COUNT</kbd>(<dfn>all<u></u><b>|</b>distinct<u></u></dfn><kbd>*</kbd><b>|</b><var>字段名</var><dfn ft="...">,<var>字段名</var></dfn>) <i md-ok>--计数</i>
</div><br>

```sql
select MAX(sage),MIN(sage),AVG(sage) from Stu;
```
```sql
/* COUNT那些事 */
select COUNT(*),COUNT(cno) from Cou; --COUNT(*) vs. COUNT(key)
select COUNT(*),COUNT(cpno) from Cou; --遇到null
```
```sql
/* distinct那些事 */
select SUM(sage),SUM(distinct sage) from Stu;
```
```sql
/* AVG=SUM/COUNT ? */
select AVG(sage),SUM(sage),COUNT(sage),
	SUM(sage)/COUNT(sage) as avg from Stu;
select AVG(sage),SUM(sage),COUNT(sage),
	cast(SUM(sage) as float)/COUNT(sage) as avg from Stu;
```
<!--{"show":{"listSub":0,"startStep":1,"sub":"--li1-size:1"}}-->
### group by子句 - 分组<sup>`§3.7.2`</sup>
<div x-bnf><dfn><kbd>GROUP BY</kbd> <var>字段名</var><dfn ft="...">,<var>字段名</var></dfn><dfn><u></u><kbd>HAVING</kbd> <var>条件表达式</var></dfn></dfn>
</div>

+ 在group by指定字段上取值**都相同**的元组分到同一组  
+ 每一组在结果集中产生一条元组(行)  
+ 一般会在`select`子句中使用`聚集函数`配合group by
```sql
/* 列出每门课的课程号、最高分、最低分、平均分 */
select cno,max(grade),min(grade),avg(grade)
from SC GROUP BY cno;

/* 🚫下面是一个错误的SQL，请大家批判地看它 */
select cno,cname,max(grade),min(grade),avg(grade)
from SC,Cou GROUP BY cno;
```
<!--{"show":{"shrink":1}}-->
```sql
/* 下面这个例子才是光明正确滴 */
select SC.cno,cname,max(grade),min(grade),avg(grade)
from SC,Cou where SC.cno=Stu.cno
GROUP BY SC.cno,cname;
```
```warning
🚨select子句中的字段只能使用出现在group by后的字段
🚨作为聚集函数的参数没有上述限制
```
#### 分组的HAVING子句<sup>`§3.7.3`</sup>
```sql
select sno,avg(grade) from SC
group by sno HAVING avg(grade)>80;

select sno,avg(grade) as avg from SC
group by sno HAVING avg>80 and sno='95001';
```
1. 按`group by`指定的属性分组
2. 然后依据`having`剔除不符合条件的分组
#### HAVING vs. WHERE<sup>`§3.7.3`</sup>
```sql
select sno,avg(grade) from SC where cno<>2
group by sno HAVING avg(grade)>80;
```
1. 首先通过`where`筛选符合条件的元组
2. 然后`group by`指定的属性分组
3. 接着依据`having`剔除不符合条件的分组
4. 最后每组分别计算聚集后产生一条元组
## 当NULL遇到……<sup>`§3.6`,`§3.7.4`</sup>
<!--{"show":{"shrinkRng":[1,1],"sub":"--li1-size:1"}}-->
### 当NULL遇到计算表达式<sup>`§3.6`</sup>
<!--{"show":{"shrink":0}}-->
```sql
select 5+NULL, 'string' || NULL, concat('A',NULL,'B');
select DISTINCT cpno from Cou;
```
+ 规则
	```tip
	💡包含NULL的计算表达的结果是NULL
	💡内建函数试图忽略NULL
	💡DISTINCT判断时，NULL=NULL 处理为 true
	```
<!--{"show":{"sub":"--li1-size:1.1;--li2-size:.8;--md-line-height:1.3"}}-->
### 当NULL遇到逻辑运算<sup>`§3.6`</sup>
+ 包含`null`的**逻辑表达式**的结果是`unknown`
	+ 如:  $5<null$  ;  $null<>null$  ;  $null=null$
+ `unknown` 参与逻辑运算有以下规则
	+ $(unknown~OR~true)=true$
	+ $(unknown~OR~false)=unknown$
	+ $(unknown~OR~unknown)=unknown$
	+ $(unknown~AND~true)=unknown$
	+ $(unknown~AND~false)=false$
	+ $(unknown~AND~unknown)=unknown$
	+ $(NOT~unknown)=unknown$
+ `where`/`having`子句的逻辑表达式如结果为`unknown`，视为`false`
<!--{"show":{"shrinkRng":[1,1],"sub":"--li1-size:1"}}-->
### 当NULL遇到聚集函数<sup>`§3.7.4`</sup>
<!--{"show":{"shrink":0}}-->
```sql
select avg(cpno),sum(cpno),count(*),count(cpno) from Cou;
```
+ 规则
	```tip
	💡所有聚集函数都忽略输入集中的空值，COUNT(*)除外
	💡如输入集为空(或忽略空值后导致空集)，COUNT返回0，其他返回null
	```
<!--{"show":{"shrinkRng":[0,1]}}-->
## 集合运算<sup>`§3.5`</sup>
+ `UNION`
	```sql
	select * from Stu where dept='IS'
	UNION
	select * from Stu where Sage<19;
	-- 等价于:
	select DISTINCT * from Stu where dept='IS' OR Sage<19;
	```
	UNION会自动去掉重复元组(去重)；可用 **`UNION ALL`** 表示不去重。
	```sql
	select sno from SC where cno=1
	UNION
	select sno from SC where cno=2
	```
+ `INTERSECT`
	```sql
	select * from Stu where dept='IS'
	INTERSECT
	select * from Stu where Sage<=19;
	-- 等价于:
	select DISTINCT * from Stu where dept='IS' AND Sage<=19;
	```
	INTERSECT自动去重；可用 **`INTERSECT ALL`** 表示不去重。
	```sql
	select cpno from Cou INTERSECT select cpno from Cou;
	-- 对比:
	select cpno from Cou;
	```
+ `EXCEPT`
	```sql
	select cno from Cou EXCEPT select cpno from Cou;
	select cpno from Cou EXCEPT select 1 from Cou;
	```
	EXCEPT自动去重；可用 **`EXCEPT ALL`** 表示不去重。
+ 关于消重(eliminates duplicates)
	+ 默认情况下，SQL中的集合运算都会自动消重
	+ 用`union all`,`intersect all`,`except all`表示不消重；<br>😰但很多数据库不支持`intersect all`,`except all`语法
<!--{"show":{"listSub":0}}-->
## 嵌套子查询/Nested Subqueries<sup>`§3.8`</sup>
&emsp;　在SQL中，一个`SELECT-FROM-WHERE`语句称为一个`查询块`。将一个查询块**嵌套**在另一个查询块中的查询称为**嵌套查询**  
&emsp;◆ 父查询（外层查询）  
&emsp;◆ **`子查询`**（内层查询）/**`Subquery`**
```sql
select sname from Stu
where sno in(select sno from SC where cno=2);
```
&emsp;　嵌套查询使得可以用一系列简单查询构成复杂的查询，从而明显地增强了SQL的查询能力。  
&emsp;　以层层嵌套的方式来构造程序正是 **S**QL(**Structured** Query Language)中“**结构化**”的含义所在
<!--{"show":{"shrinkRng":[0,1]}}-->
### 集合成员谓词IN带子查询<sup>`§3.8.1`</sup>
IN带子查询是指父查询与子查询之间用`IN`/`NOT IN`进行连接，判断某个属性列值是否在子查询的结果中。  
🔎 **查询与“刘晨”在同一个系的学生的学号、姓名、所在系**
```sql
-- 自身连接求解:
SELECT S1.Sno,S1.Sname,S1.dept FROM Stu S1,Stu S2
WHERE S1.dept=S2.dept AND S2.sname='刘晨';
-- 上方语句步骤分解:
SELECT dept FROM Stu WHERE Sname='刘晨';--步骤1
SELECT Sno,Sname,dept FROM Stu WHERE dept IN('CS');--步骤2
-- 子查询求解:
SELECT Sno,Sname,dept FROM Stu WHERE dept IN(
	SELECT dept FROM Stu WHERE Sname='刘晨'
)
```
🔎 **查询选修了课程名为“数据库”的学生的学号和姓名**
```sql
-- 连接求解:
SELECT Stu.Sno,Sname FROM Stu,SC,Cou
WHERE Stu.Sno=SC.Sno AND SC.Cno=Cou.Cno AND Cname='数据库';
-- 子查询求解:
SELECT Sno,Sname FROM Stu WHERE Sno IN(
	SELECT Sno FROM SC WHERE Cno IN(
		SELECT Cno FROM Cou WHERE Cname='数据库'
	)
);
```
🔎 **查询没有选修课程名为“数据库”的学生的学号和姓名**
```sql
SELECT Sno,Sname FROM Stu WHERE Sno NOT IN(
	SELECT Sno FROM SC WHERE Cno IN(
		SELECT Cno FROM Cou WHERE Cname='数据库'
	)
);
```
如果不使用子查询
```sql
SELECT Stu.Sno,Sname FROM Stu
	EXCEPT
SELECT Stu.Sno,Sname FROM Stu,SC,Cou
WHERE Stu.Sno=SC.Sno AND SC.Cno=Cou.Cno AND Cname='数据库';
```
<!--{"show":{"listSub":0,"startStep":1}}-->
### 比较运算带子查询<sup>`§3.8.2`</sup>
<!--{"show":{"css":"line-height:1.8"}}-->
父查询与子查询之间用比较运算符(`>` `<` `=` `<=` `>=` `<>`)进行连接
```tip
需要注意的是，子查询一定要跟在比较符之后而不是之前
```
🔎 **找出超过平均年龄的学生的学号和姓名**
```sql
SELECT Sno,Sname FROM Stu WHERE Sage>(
	SELECT AVG(Sage) FROM Stu
);
```
<!--{"show":{"sub":"--li1-size:1.15"}}-->
#### 单值(标量/Scalar)比较<sup>`§3.8.2`,`§3.8.7`</sup>
<!--{"show":{"css":"font-size:1.3em"}}-->
语法: <span x-bnf><var>比较运算符</var>(<var>子查询</var>)</span>

+ `标量`查询/`Scalar` query
	查询结果<b md-most>有且只有一个元组</b>，且元组<b md-most>有且只有一个分量</b>(属性)的查询，如：
	```sql
	select AVG(Sage) from Stu; --要求至少有一个学生
	select cname from Cou where cno=1; --要求必须有1号课程
	select cno from Cou where cname='数据库'; --这个不一定是标量
	```
+ 计算表达式中的标量查询
	```sql
	(SELECT count(*) FROM SC) / (SELECT count(*) FROM Stu);
	```
+ 比较运算直接带子查询要确保**子查询是标量查询**
	```sql
	SELECT Sno,Sname FROM Stu WHERE Sage>(
		SELECT AVG(Sage) FROM Stu --这个必需是标量查询
	);
	```
<!--{"show":{"sub":"--li1-size:1.15"}}-->
#### 集合比较/Set Comparison<sup>`§3.8.2`</sup>
<!--{"show":{"css":"font-size:1.3em"}}-->
语法: <span x-bnf><var>比较运算符</var> <var><kbd>all<b>|</b>some<b>|</b>any</kbd></var>(<var>子查询</var>)</span>
+ `all`: 子查询结果中所有的值都必须满足条件
+ `some`等同`any`: 子查询结果中有一个满足条件即可

🔎 **找出年龄比'IS'系的至少一个学生大的学生姓名**
<!--{"show":{"shrink":1}}-->
```sql
select sname from Stu where sage>SOME(
	select sage from Stu where dept='IS'
);
```
🔎 **找出年龄比'IS'系的所有学生大的学生姓名**
<!--{"show":{"shrink":1}}-->
```sql
select sname from Stu where sage>ALL(
	select sage from Stu where dept='IS'
);
```
+ 等效语义
	+ **`=some(...)`** 等同于 **`in(...)`**
	+ **`<>some(...)`** 基本无意义
	+ **`<>all(...)`** 等同于 **`not in(...)`**
	+ **`>some(...)`** 等同于 **`>(select min(...) ...)`**
	+ **`<some(...)`** 等同于 **`<(select max(...) ...)`**
	+ **`>all(...)`** 等同于 **`>(select max(...) ...)`**
	+ **`<all(...)`** 等同于 **`<(select min(...) ...)`**
### 嵌套查询的求解<sup>`§3.8.0`</sup>
#### 嵌套查询的一般求解<sup>`§3.8.0`</sup>
DBS对嵌套查询的求解方法一般是由里向外处理。即每个子查询在其上一级查询处理之前求解，子查询的结果用于建立其父查询的查找条件
```sql
SELECT Sname FROM Stu WHERE Sno IN(
	SELECT Sno FROM SC WHERE Cno=2;
);
--首先求解子查询:
SELECT Sno FROM SC WHERE Cno=2; --得到{('95001'),('95002')}
--代入上级查询求解:
SELECT Sname FROM Stu WHERE Sno IN('95001','95002');
```
<!--{"show":{"sub":"--md-line-height:1.65"}}-->
#### 相关变量/Correlation Variables<sup>`§3.8.3`</sup>
考虑这个查询：🔎**找出每个学生超过自己选修课程平均成绩的课程号**
<!--{"show":{"css":"margin-bottom:0"}}-->
```sql
SELECT Sno,Cno FROM SC X WHERE Grade>=(
	SELECT AVG(Grade) FROM SC Y WHERE Y.Sno=X.Sno
);
```
<b md-most>子查询</b>的查询条件<b md-most>依赖</b>于<b md-most>外层查询</b>的<b md-most>属性</b>（在本例中是`X.Sno`）  
&emsp;　这种子查询称为 **`相关子查询`/`Correlated Subquery`**  
&emsp;　这种属性被称为 **`相关变量`/`Correlation Variables`**

#### 相关子查询的求解<sup>`§3.8.3`</sup>
求解相关子查询不能象求解不相关子查询那样，一次将子查询求解出来，然后求解父查询。相关子查询的内层查询由于与外层查询有关，因此必须反复求值
<!--{"show":{"sub":"--li2-size:1;--md-line-height:1.8"}}-->
+ 从概念上讲，相关子查询的一般处理过程是
	1. 首先取外层查询中S表的第一个元组
	2. 根据它与内层查询相关的属性值处理内层查询，若WHERE子句返回值为真，则取此元组放入结果表
	3. 然后再检查S表的下一个元组
	4. 重复这一过程，直至S表全部检查完毕
<!--{"show":{"listSub":0,"startStep":1,"sub":"--li1-size:1.15"}}-->
### 空关系(Empty Relation)测试：EXISTS<sup>`§3.8.3`</sup>
<!--{"show":{"css":"font-size:1.3em"}}-->
语法: <span x-bnf><dfn><kbd>NOT</kbd><u></u></dfn><kbd>EXISTS</kbd>(<var>子查询</var>)</span>
+ 判断子查询结果是否<b md-most>空集</b>，代表存在量词<code md-most>**∃**</code>
	🔎**列出所有选修了1号课程的学生姓名**
	<!--{"show":{"shrink":1}}-->
	```sql
	-- 连接求解:
	SELECT Sname FROM Stu,SC WHERE SC.Sno=Stu.Sno AND Cno=1;
	-- EXISTS子查询求解:
	SELECT Sname FROM Stu WHERE EXISTS(
		SELECT * FROM SC WHERE SC.Sno=Stu.Sno AND Cno=1
	);
	```
<!--{"show":{"shrink":1}}-->
+ 等价替换  
	一些带EXISTS或NOT EXISTS谓词的子查询不能被其他形式的子查询等价替换，但所有带IN谓词、比较运算符、ANY和ALL谓词的子查询都能用带EXISTS谓词的子查询等价替换
<!--{"show":{"sub":"--md-line-height:1.75"}}-->
#### 全称量词的表达<sup>`§3.8.3.0`</sup>
$\forall x(P)\equiv\lnot(\exists x(\lnot P))$  
选修了全部课程的学生姓名 ≡ 不存在一门课程是没选的
```sql
select Sname from Stu where NOT EXISTS(
	select * from Cou where NOT EXISTS(
		select * from SC where Sno=Stu.Sno and Cno=Cou.Cno
	)
);
```
#### 蕴含的表达<sup>`§3.8.3.0`</sup>
$p\to q\equiv\lnot p\lor q$  
🔎**查询至少选修了学生'95002'选修的全部课程的学号**  
<!--{"show":{"css":"font-size:.75em"}}-->
$p="学生95002选修了课程y";\quad q="学生x选修了课程y"$  

$\forall y(p\to q)\\
=\lnot(\exists y(\lnot(p \to q)))\\
=\lnot(\exists y(\lnot(\lnot p \lor q)))\\
=\lnot\exists y(p \land \lnot q)$ 不存在课程$y$被学生95002选了($p$)而学生$x$没有选($\lnot q$)
<!--{"show":{"shrink":1}}-->
```sql
select Sno from Stu X where NOT EXISTS(
	select * from Cou Y where Cno in(
		select Cno from SC where Sno='95002' --95002选了Y
	) and NOT EXISTS(
		select * from SC where Sno=X.Sno and Cno=Y.Cno
	)
);
```
<!--{"show":{"shrink":1}}-->
```sql
select distinct Sno from SC X where NOT EXISTS(
	select * from SC Y where Y.Sno='95002' and NOT EXISTS(
		select * from SC Z where Z.Sno=X.Sno and Z.Cno=Y.Cno
	)
);
```
<!--{"show":{"listSub":0,"startStep":1,"sub":"--li2-size:1;--md-line-height:1.8"}}-->
### 重复元组(Duplicate Tuples)存在测试<sup>`§3.8.4`</sup>
<!--{"show":{"css":"font-size:1.3em"}}-->
语法: <span x-bnf><dfn><kbd>NOT</kbd><u></u></dfn><kbd>UNIQUE</kbd>(<var>子查询</var>)</span>
+ 判断子查询结果是否有<b md-most>重复元组</b>  
	🔎**列出最多只被一门课作为先导课的课程号**
	<!--{"show":{"shrink":1}}-->
	```sql
	select Cno from Cou C where UNIQUE(
		select Cpno from Cou where Cpno=C.Cno
	)
	```
+ 很多RDBS不支持`UNIQUE(子查询)`，可用替代方案
	<!--{"show":{"shrink":1}}-->
	```sql
	select Cno from Cou C where 1>=(
		select COUNT(*) from Cou where Cpno=C.Cno
	)
	```
### FROM子句中的子查询<sup>`§3.8.5`</sup>
<div x-bnf><kbd>FROM</kbd> <var>表名<b>|</b>视图名<b>|</b>子查询</var><dfn ft="...">,<var>表名<b>|</b>视图名<b>|</b>子查询</var></dfn>
<var>子查询</var><b>::=</b>(<var>SELECT语句</var>)<dfn><u></u><kbd>AS</kbd> <var>别名</var></dfn>
</div>

```sql
select avg_grade,Cname from Cou,(
	select Cno,avg(grade) as avg_grade from SC group by Cno
) AS SC_AVG
where SC_AVG.Cno=Cou.Cno and avg_grade>85
```
<!--{"show":{"css":"margin-bottom:-.5em"}}-->
### WITH子句<sup>`§3.8.6`</sup>
<div x-bnf><kbd>WITH</kbd> <var>模式声明</var> <kbd>AS</kbd>(<var>子查询</var>)<dfn ft="...">,<var>模式声明</var> <kbd>AS</kbd>(<var>子查询</var>)</dfn>
	<var>模式声明</var><b>::=</b><var>别名</var>(<var>字段名</var><dfn ft="...">,<var>字段名</var></dfn>)
</div>

`WITH...AS...`子句提供一种定义临时关系的方式，作用域仅在同一查询中
```sql
WITH SC_AVG(Cno,avg_grade) AS(
	select Cno,avg(grade) as avg_grade from SC group by Cno
)
select avg_grade,Cname from Cou,SC_AVG
where SC_AVG.Cno=Cou.Cno and avg_grade>85;
```
## SQL数据修改<sup>`§3.9`</sup>
<!--{"show":{"css":"margin-bottom:-.5em"}}-->
### DELETE - 删除元组<sup>`§3.9.1`</sup>
<div x-bnf><kbd>DELETE FROM</kbd> <var>表名</var><dfn><u></u><kbd>WHERE</kbd> <var>条件表达式</var></dfn></div>

```sql
DELETE FROM Cou; --尝试删除所有课程
DELETE FROM SC WHERE grade<60;
DELETE FROM SC WHERE grade<(select avg(grade) from SC);
-- 疑问：删除会导致平均成绩变化！
-- 实际上会先计算“子查询” 然后再带入求解
```
```warning
🚨注意：FROM后只能有一个关系表!!!
```
### INSERT INTO - 添加元组<sup>`§3.9.2`</sup>
<div x-bnf><kbd>INSERT INTO</kbd> <var>表名</var><dfn>(<var>字段名</var><dfn ft="...">,<var>字段名</var></dfn>)</dfn> <kbd>VALUES</kbd>(<var>值</var><dfn ft="...">,<var>值</var></dfn>)
<b>|</b><kbd>INSERT INTO</kbd> <var>表名</var><dfn>(<var>字段名</var><dfn ft="...">,<var>字段名</var></dfn>)</dfn> <var>子查询</var></div>

<!--{"show":{"shrink":1}}-->
```sql
--不指定字段名，values中的值数量和域需匹配create table的字段定义:
INSERT INTO Stu VALUES('95020','陈冬','男',18,'CS');
--指定属性列values中值的数量和顺序需和属性列匹配:
INSERT INTO Stu(Sname,Sno,Ssex) VALUES('辛夏','95021','女');
--指定为空值:
INSERT INTO Stu VALUES('95020','梁秋','男',NULL,NULL);
```
```warning
🚨注意：INSERT INTO后只能有一个关系表!!!
```
+ 缺省值和默认值
	<!--{"show":{"shrink":1}}-->
	```sql
	-- /*假定有表
	create table T(
		id int primary key,
		x int not null DEFAULT 0,
		y int not null,
		z int
	); -- */
	insert into T(y,id) values(2,1); -- (id,x,y,z)=(1,0,2,null)
	insert into T(id) values(2);-- y报错
	```
	```tip
	💡未指定某字段值时，首先尝试字段的DEFAULT设定，其次尝试NULL
	```
+ 插入子查询，子查询先完成，然后再插入
	<!--{"show":{"shrink":1}}-->
	```sql
	insert into R(A,B,C) select S.*,0 from S;
	```
	```warning
	🚨子查询的结果的模式必需和指定的目标模式匹配!!!
	```
<!--{"show":{"listSub":0,"startStep":1}}-->
### UPDATE - 修改元素分量<sup>`§3.9.3`</sup>
<div x-bnf><kbd>UPDATE</kbd> <var>表名</var> <kbd>SET</kbd> <var>字段</var>=<var>值</var><dfn ft="...">,<var>字段</var>=<var>值</var></dfn><dfn><u></u><kbd>WHERE</kbd> <var>条件表达式</var></dfn></div>

🚨注意：`UPDATE`后只允许存在一个关系表!!!  
💡<span x-bnf><var>值</var></span>可以是常量、计算表达式,也可以是`标量查询`
```sql
UPDATE SC SET grade=grade+10; --分数可能超100
--两条语句分段改分，不同顺序会导致不同结果:
UPDATE SC SET grade=grade*1.05 WHERE grade>80;
UPDATE SC SET grade=grade*1.25 WHERE grade<=80;
--使用标量子查询赋值:
update Stu set Sage=(SELECT avg(sage) FROM Stu)+1;
```
#### 条件修改/Case Statement for Conditional Updates<sup>`*` `§3.9.3`</sup>
```sql
update SC set grade=CASE
	WHEN grade<=90 THEN grade*1.05
	WHEN grade<=80 THEN grade*1.25
	ELSE grade*1.01
END;
```