### **SQL语法:**

#####    SQL语句分类:

          DDL:数据定义语言, 定义是的结构, create , drop , alter, truncate 
          DML:数据操纵语言, 操纵的数据 insert , update, delete
          DCL:数据控制语言, 控制的是权限/安全, grant revoke 
          DQL:数据查询语言, select

#####    SQL的编写顺序:

```sql
    select 输出的列 from 表名 where 条件 group by 分组 having 分组之后的条件过滤 order by 排序;`
	 '编写顺序':
       select .. from .. where .. group by .. having ..order by ..   
     '执行顺序':
       from..where .. group by .. having .. select.. rownum order by..  
```
##### Oracle中常用 函数

```json
Oracle中函数:
          单行函数: 只对一个值进行处理
				nvl(null,值) ===>表示如果参数1,为空,那么他的值为参数2的值;       || oracle中字符串拼接..
                字符串函数  to_char(sysdate,'yyyy-mm-dd hh24:mi:ss')
                数值函数 ceil	floor mod abs round 
                日期函数 months_between(sysdate,hiredate) '求两个日期之间间隔月数'
						add_months(sysdate,3)   '计算几个月之后的日期'
          多行函数: 聚合函数 max min, avg,count,sum, 处理多行记录

```

##### 条件表达式:

```sql
通用写法:
        case 列名
          when '值1' then 输出
          when '值2' then 输出
          when '值3' then 输出    
          else
            '其他'
          end [as] 别名;
     Oracle特有的写法:
          decode(列名,if1,then1,if2,then2,default)   
```

##### 子查询

```sql
概念: 查询语句中嵌套查询语句            作用: 用于解决复杂的查询需求

单行子查询:常用操作符: > >= = < <= != <>  
多行子查询:常用的操作符: in , not in , any , all, exists,
exists(查询语句): 如果查询语句,有结果,则返回true,否则返回false 
```

##### 集合运算

```sql
集合运算:  并集,交集,差集
eg:
	-- 并集 关键词'union'
        select * from emp where sal > 1500  union  select * from emp where deptno=20;(去重了)
		select * from emp where sal > 1500 union all select * from emp where deptno=20;
	--差集 'minus'
		select * from emp where sal > 1500 minus select * from emp where deptno=20;
	--交集 ''
		select * from emp where sal > 1500 intersect select * from emp where deptno=20;

注意:  
	1.'列的数量要一致'
    2.'列的类型要保持一致'
    3.'列的顺序要一致  如果不足,可以用null补齐'
```

##### 伪列

```sql
  rownum : 伪列 , 表示的行号, 主要用在'分页'的时候  
	select * from emp (select rownum r,emp.* from emp where rownum<=5) t where r>=9 (分页查询:5-9行的数据)

  rowid  : 伪列, 表示的是每行记录存在磁盘中'真实的一个物理地址', 运用在'索引查询'中  ++++运用它可以删除重复数据
```



oracle建表顺序_授权_-  DDl

```sql
   1.创建表空间
       语法:
            create tabelspace 表空间的名称
            datafile '数据文件存放的路径'------注意:这个路径是oracle服务器路径
            size 大小-------多少M/G
            autoextend on 打开自动扩容
            next 每次扩容大小   
   2. 创建用户:
       语法:
            create user 用户名
            identified by 密码
            default tablespace 表空间的名称    
-- 授权:
grant create session to zhangsan;

-- 取消授权:
revoke create session from zhangsan;

-- 授予角色:
grant dba to zhangsan;

四大约束 ----- -/主键   /唯一  /非空   /外键foreign key  约束从表中记录必须是参考主表中记录    /检查约束  check(where条件)

列类型:
  日期类型:
              date : 年月日时分秒 2018/2/3 16:01:24
              timestamp:时间戳
  数字类型:
            number(总长度,小数位) : 表示的浮点数/整数
            number
  字符串类型:
            varchar2(总长度): 可变长度字符串,不能自动增长
            char(总长度) :    固定长度字符串
```



#### 事务

```javascript
    事务: 逻辑上的一组操作,要么都成功,要么都失败
    
    事务特型ACID:
        原子性: 事务是最小不可分割的单元,要么都成功,要么都失败
        一致性: 事务执行前后,数据状态保持一致 2000 2000 3998
        隔离性: 一个事务在执行的时候,不受另外的事务影响
        持久性: 数据一旦提交,就持久的保存到磁盘上
        
    '不考虑隔离级别':
        脏读: 读到未提交的数据
        不可重复读: 一个事务中两次读取,结果不一致(update)  
        虚读/幻读:  一个事务中两次读取,结果不一致(insert)  
                                    
    隔离级别MYSQL:
        READ UNCOMMITTED :    脏读 不可重复读 虚读/幻读
        READ COMMITTED   :    不可重复读 虚读/幻读
        REPEATABLE READ  :    虚读/幻读
        SERIALIZABLE串行化:  含义: 一个事务在执行的时候,另外一个事务处于等待状态  
    
    Oracle中隔离级别:
        READ COMMITTED  默认隔离级别
        SERIALIZABLE串行化
        READ ONLY    只读    
  
  事务的保存点  savepoint 保存点的名称
   回滚到事务的点  rollback to 保存点的名称
```



##### 视图

```sql
  视图: 相当于是虚表,
       视图是对查询结果的封装,视图本身是不存储任何数据,所有的数据都存放在原来的表中
      作用:
          1.简化复杂的查询语句
          2.屏蔽表中细节 
          3.增加代码被反编译的难度 
      语法:
          create [ or replace ] view 视图的名称 as 查询语句 [with read only]  
```



##### 同义词

```sql
     同义词 : 意义相近的词
     相当于是取一个别名
     
     作用:
         1.取一个别名,缩短表名编写
         2.增加代码被反编译的难度 
		eg:	create synonym yuangong for view_test3;
```



##### 序列

```sql
  序列:
       1,2,3,4,5,6,7...
       2,4,6,8,10....
      mysql : auto_increment  
     Oracle中主要生成编号   
     语法:
         create sequence 序列的名称
         start with 从几开始
         increment by 每次递增多少
         minvalue 最小值 | nominvalue
         maxvalue 最大值 | nomaxvalue
         cycle  | nocycle  循环的意思
         cache 3缓存的数量
              1,2,3
                    4,5,6
     操作:
         当前值    : currval   注意: 当前值必须是调用了一次nextval之后才能正常使用的     
         取下一个值: nextval  
    '可以不带参数' create sequence seq_test1 默认就是  '从0开始' '每次增加1' '不循环' '没有最大值'     
    注意: 序列中值,无论发生回滚或者异常,一旦被提取过了,永不回头的向下递增    
```



#### 索引

```sql
    1.相当于是一本书的目录
      2.索引是一种已经排好顺序的数据结构,主要是用于帮助数据库快速查找到记录
      
    '主键约束自带唯一索引'
    '唯一约束自带唯一索引'  
      
   语法:
      create index 索引的名称 on 表名(列名...)   
      
   扩展: 索引的原理
             索引是一种已经排好顺序的数据结构 Btree  平衡多路查找树
                              B - Balance  平衡的
   
      select 后面的内容除了索引列,不包含其它内容, 不建议写 *  
      
   注意:
       1.索引能够提高查询效率
       2.索引会反向影响增删改的效率.   
      
   创建索引的情况:
       数据量比较大的情况,经常需要查询
       哪些列经常作为查询条件,就根据这些列来创建索引  
       
   不创建索引的情况:
       若数据量比较小,可以不需要创建索引  
       哪些列经常增删改 > 查询的操作 
  
  SQL优化:
       1.会分析 : '执行计划'
       2.如何优化: '创建索引优化'
       3. 注意: 编写查询语句的时候,要尽量使用索引
                索引失效的问题

 执行计划:
             一条SQL语句,发送给数据库之后,数据库不会立即执行这条语句
             1.先会对SQL语句进行解析
             2.解析完成需要知道执行这条需要查询哪些表,经过哪些步骤,消耗多少资源
            
             作用: 
                  1.对于数据库而言,帮助数据库分析步骤
                  2.对于程序员, 帮助程序员去分析和优化SQL语句
         '优化器': 对SQL语句简单优化  
         
         
    注意: 编写条件的时候,最好要用到索引
          索引失效的问题    
```





#### PLSQL编程

```plsql
语法:
          declare
          声明部分,声明变量 

          变量名称 变量的类型 := 赋初始值

          -- 相当于是声明了一个引用类型的变量
          vsal emp.sal%type;

          -- 声明变量 : 记录一整行信息
          vrow emp%rowtype;  -- 记录型变量

          begin
          业务逻辑
           end;  

	if语句:
		if  	条件 	then
		elsif 	条件	then
		else
		end if;
	3种循环:
		1.loop 
          --循环体
			exit when 退出的条件
          end loop
		2.for 变量 in [reverse] (1..100)范围 loop
		  end loop;
		3.   while 条件 loop  -----记得写结束条件 ,程序出口
			 end loop;
			

例外:
        declare
           声明部分          
        begin
           业务逻辑
        exception
           处理例外部分
           when 例外1 then
           when others then
         
        end;
        
        1.声明例外:
               例外名称 exception
        2. 抛出自定义的例外:
               raise 例外名称     
```



##### 游标

```sql
   游标:
       实际上是对查询结果集的封装,可以通过操作游标来操作结果集
       相当于是JDBC中ResultSet
    
   游标的使用步骤:
       1.声明游标,指定结果集: 
               cursor 游标名 is 查询结果集
               curosr 游标名(参数名 参数类型) is 查询语句 where deptno=参数名
               
       2.打开游标           : open 游标名
       3.从游标中提取数据   : fetch 游标名 into 变量
       4.关闭游标           : close 游标名   
       
       判断游标是否没有提取到数据:
                        游标名%found : 找到数据
                        游标名%notfound : 没有数据
```



##### 存储过程和存储函数

```plsql
1.存储过程
	定义:实际上将一段已经编译好的PLSQL业务逻辑代码片断,封装在Oracle数据库中 相当于在java中封装了一个方法
	好处: 1.提高了代码复用性
              2.提高代码执行效率
	create or replace procedure proc_name(变量 in|out 类型,.. )
		is|as
		--声明
	begin
		--业务逻辑
	end;
2.存储函数:有返回值
	create or replace function funName(变量 in|out 类型,.. ) return 参数
		is|as
	begin 
	end;
```

##### 

##### java调用存储过程

```java
关键代码:		
		使用jdk接口:CallableStatement
				String sql="{call proc_getyearsal(?,?)}";------存储过程写法 (存储函数参考jdk手册)
				CallableStatement call = conn.prepareCall(sql);-----类似PrepareStatement
	@Test
	public void test2() throws Exception{
//	   1.注册驱动
		Class.forName("oracle.jdbc.OracleDriver");
//	   2.建立连接: 用户名 密码, 连接的地址
		String url="jdbc:oracle:thin:@192.168.110.100:1521:orcl";
		String user="zhangsan";
		String password="zs123";
		Connection conn = DriverManager.getConnection(url, user, password);
		System.out.println(conn);
//	   3.获取执行SQL的对象: CallableStatement 第1个输入参数, 第2个输出参数
		String sql="{call proc_getyearsal(?,?)}";
		CallableStatement call = conn.prepareCall(sql);
		//3.1 设置输入参数
		call.setInt(1,7369);
		//3.2 注册输出参数
		call.registerOutParameter(2, Types.DOUBLE);
//	   4.执行SQL
		call.execute();
//	   5.处理结果
		double yearsal = call.getDouble(2);
		System.out.println("年薪:"+yearsal);
//	   6.释放资源
		call.close();
		conn.close();
	}
	
	/*
	 -- 调用输出类型为游标的存储过程: 输出所有员工信息
		-- sys_refcursor :系统引用游标, 主要是用于声明一个游标类型
		--    声明变量 : 游标名 sys_refcursor;  ,在使用的时候才去指定结果集
		create or replace procedure proc_getemps(vemps out sys_refcursor)
		is
		begin
		   -- 打开系统引用游标,指定结果集 : 谁调用谁负责关闭
		   open vemps for select * from emp;
		end;
	 * */
	@Test
	public void test3() throws Exception{
//	   1.注册驱动
		Class.forName("oracle.jdbc.OracleDriver");
//	   2.建立连接: 用户名 密码, 连接的地址
		String url="jdbc:oracle:thin:@192.168.110.100:1521:orcl";
		String user="zhangsan";
		String password="zs123";
		Connection conn = DriverManager.getConnection(url, user, password);
//	   3.获取执行SQL的对象
		String sql="{call proc_getemps(?)}";
		CallableStatement call = conn.prepareCall(sql); //oracle.jdbc.driver.T4CCallableStatement
		
		System.out.println(call.getClass().getName());
		//3.1 注册输出参数
		call.registerOutParameter(1, OracleTypes.CURSOR);
//	   4.执行SQL
		call.execute();
//	   5.处理结果   父类声明                                               =   子类实现类
		OracleCallableStatement call2 = (OracleCallableStatement)call;
		ResultSet rs = call2.getCursor(1);
		//ResultSet rs = (ResultSet) call.getObject(1);
		//System.out.println(object.getClass().getName());
		while(rs.next()){
			System.out.println(rs.getObject("empno"));
			System.out.println(rs.getObject("ename"));
			System.out.println(rs.getObject("sal"));
			
			System.out.println("========================");
		}
//	   6.释放资源
		rs.close();
		call.close();
		conn.close();
	}
```



#####触发器

```sql
 触发器:
         当用执行了 insert | update | delete 操作的时候,
                      我们可以在触发器中触发一段PLSQL业务逻辑执行
                      
         语法:
             create or replace trigger 触发器的名称
             before | after
             insert | update | delete
             on 表名
             [for each row]
             declare
                -- 声明部分
             begin
                -- 逻辑部分
             end; 
             
          作用:
             1.监听表中数据变化
             2.做一些数据校验   
             
     触发器的分类:
             语句级触发器: 一条SQL语句,无论影响多少行记录,触发器始终只执行一次
             行级触发器:  一条SQL语句,影响了多少行记录,触发器就触发多少次 
              行级触发器的内置对象  
                             :new  代表的是新的记录
                             :old  代表的是旧的记录
                        update emp set sal = sal+100;
                             :new.sal     sal +100
                             :old.sal     sal    
```