### 7.1 今日内容介绍

### 7.2【应用】SQL操作实战-查询结果插入到其它表中

* 解决的问题：查询原有goods表的分类信息数据, 写入到good_cates表中

  * 创建 goods_cates表

    > ```mysql
    > 	create table good_cates(
    >         id int not null primary key auto_increment,
    >         name varchar(50) not null
    >     );
    > ```

  * 插入数据:

    > ```mysql
    > insert into good_cates(name) select cate_name from goods group by cate_name;
    > ```

### 7.3【应用】SQL操作实战-使用连接更新表中某个字段数据

* 解决的问题：将goods表中的分类名称改为good_cates表中的id

  > ```mysql
  > update (goods inner join good_cates on goods.cate_name=good_cates.name) set goods.cate_name=good_cates.id;
  > ```

### 7.4【应用】SQL操作实战-创建表并给某个字段添加数据

* 解决的问题：创建了一个新的good_brands表, 同时给name字段插入品牌数据

  > ```mysql
  > -- 注意:这里看不到insert into的字样, 但是一旦连用, 就可以实现创建表并插入
  > create table good_brands (     
  > id int unsigned primary key auto_increment,     
  > name varchar(40) not null
  > ) select brand_name as name from goods group by brand_name;
  > 
  > -- 将goods表中的品牌名称更改成品牌表中对应的品牌id
  > update goods as g inner join good_brands gb on g.brand_name = gb.name set g.brand_name = gb.id;
  > ```

### 7.5【应用】SQL操作实战-修改goods表结构

* 解决的问题：同时修改表中的多个字段的名字\类型\约束

  > ```mysql
  > alter table goods change cate_name cate_id int not null, change brand_name brand_id int not null;
  > ```

### 7.6【了解】事务概念及特性

- 事务概念：事务就是一系列执行SQL语句的操作, 这些操作要么完全地执行，要么完全地都不执行， 它是一个不可分割的工作执行单元。
- 作用：事务能够保证数据的完整性和一致性，让用户的操作更加安全。
- 事务的特征 ACID：
  - A, 原子性
  - C,一致性
  - I，隔离性
  - D, 持久性

### 7.7【记忆】事务使用

- 步骤：

  - 开启事务

    > `begin`

  - 操作数据库

    > update goods set price=price+100 where id=21;

  - 确认修改

    > `commit`

  - 回滚

    > `rollback`

- 注意: **MySQL数据库默认采用自动提交(autocommit)模式**

### 7.8【理解】验证事务的ACID特性

- commit;

  客户端A更新数据

  客户端B查询

  如果A 不commit B就看到的是老的数据

- rollback;

- 验证隔离性：同一时间修改同一个记录

### 7.9【理解】索引

- 索引作用：能加快数据库的查询速度

- 索引的使用：

  - 查看索引: `show index from 表名`

  - 创建索引:   

    >```sql
    >alter table 表名 add index 索引名[可选](列名, ..)
    >```

  - 删除索引：`alter table 表名 drop index 索引名`

- 插入10万条数据到数据库中

  > ```sql
  > create table test_index(title varchar(10));
  > ```
  >
  > ```python
  > from pymysql import connect
  > 
  > def main():
  >     # 创建Connection连接
  >     conn = connect(host='localhost',port=3306,database='python',user='root',password='mysql',charset='utf8')
  >     # 获得Cursor对象
  >     cursor = conn.cursor()
  >     # 插入10万次数据
  >     for i in range(100000):
  >         cursor.execute("insert into test_index values('ha-%d')" % i)
  >     # 提交数据
  >     conn.commit()
  > 
  > if __name__ == "__main__":
  >     main()
  > ```

- 验证 索引效果

  - 开启检测    `set profiling=1;`
  - 执行sql  `select * from test_index where title='ha-99999';`
  - 查看每一个sql执行的时间  `show profiles;`
  - 添加索引 `alter table test_index add index my_title(title);`
  - 执行sql `select * from test_index where title='ha-99999';`
  - 查看 执行效率  `show profiles`;

### 7.10【理解】联合索引

- 联合索引

  > 又叫复合索引，即一个索引覆盖表中两个或者多个字段，一般用在多个字段一起查询的时候。

- 联合索引的好处

  > 减少磁盘空间开销，因为每创建一个索引，其实就是创建了一个索引文件，那么会增加磁盘空间的开销。

- 联合索引最左原则

  >  即index(name,age)支持 name 、name 和 age 组合查询,而不支持单独 age 查询，因为没有用到创建的联合索引。
  >
  > 在使用联合索引的查询数据时候一定要保证联合索引的最左侧字段出现在查询条件里面，否则联合索引失效

- MySQL中索引的优点和缺点和使用原则

  - 优点: 加快数据查询速度
  - 缺点: 创建索引耗费时间和占用磁盘空间
  - 使用原则:
    - 合理使用, 不是越多越好
    - 避免对经常更新的表创建索引, 对经常查询的字段创建索引
    - 数量小的表最好不要使用索引
    - 同一字段相同值较多的不要建立索引


### 7.11【应用】Python连接MySQL

- 作用：

- 使用的步骤：

  - 导入模块 pymysql

    >```python
    >import pymysql
    >```

  - 建立连接对象 pymysql.connect()

    > ```python
    > conn = pymysql.connect(host='localhost', port=3306, user='root', password='mysql',database='jing_dong', charset='utf8')
    > ```
    >
    > port 默认为3306

  - 创建游标对象

    >```python
    >cs = conn.cursor()
    >```

  - 使用游标对象执行SQL语句

    >```python
    >sql = "select * from goods;"
    ># execute执行后, 会返回影响的行数
    >count = cs.execute(sql)
    >print('影响的行数', count)
    >```

  - 获取执行的结果

    >```python
    >one = cs.fetchone()
    >
    >one = cs.fetchall()
    >```

  - 打印输出获取的内容

    >```python
    >for data in cs.fetchall():
    >    print('单行数据', data)
    >```

  - 关闭游标对象

    >```python
    >cs.close()
    >```

  - 关闭连接对象

    >```python
    >conn.close()
    >```

### 7.12【应用】Python操作数据库CURD

- 操作步骤：

  ```mysql
  # 数据库的相关操作都需要增加异常处理
  try:
      # 添加 SQL 语句
      # sql = "insert into good_cates(name) values('商务本');"
      # 删除 SQ L语句
      # sql = "delete from good_cates where id = 8;"
      # 修改 SQL 语句
      sql = "update good_cates set name = '商务本' where id = 100;"
      # 执行 SQL 语句
      # 执行SQL语句失败, 会在execute时, 就会出现异常, 导致终止
      row_count = cursor.execute(sql)
      print("SQL 语句执行影响的行数%d" % row_count)
      # 提交数据到数据库
      conn.commit()
  except Exception as e:
      # 执行失败是因为SQL语句语法有问题, 不是因为操作没有结果就导致失败(影响的行数0)
      print("失败")
      # 回滚数据， 即撤销刚刚的SQL语句操作
      conn.rollback()
  ```

  

### 7.13【应用】SQL防注入

* SQL注入: 

  * 用户提交带有恶意的数据与SQL语句进行字符串方式的拼接，从而影响了SQL语句的语义，最终产生数据泄露的现象。

  * SQL注入示例:

    ```python
    # 输入 ' or 1 = 1 or '   (单引号也要输入)
    sql = "select * from goods where name='%s'" % find_name
    
    # 当用户输入了上面的字符串时, 就会导致原先的sql语义发生改变. 导致查询了全部的数据
    sql = "select * from goods where name='' or 1=1 or ''"
    ```

- 防注入的思路：

  - 把参数封装到 列表中 

    > `param_list = [good_name]`

  - 把列表传递给 execute(sql, 列表)

    > `cs.execute(sql, param_list)`

  - sql中需要变化的地方，可以占位符 %s %d...

    > sql ="select * from goods where name = %s order by id desc"
    >
    > 注意：SQL 可以出现多个占位符，后续列表中元素的个数要与之对应

### 7.14 今日知识总结