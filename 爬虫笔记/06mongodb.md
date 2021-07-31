# mongodb数据库

```
启动
	端口号: 27017
	配置文件: /etc/mongod.conf
	
	本地测试启动(功能受限)
		验证数据库能否正常运行
	生产方式启动(完整功能)
		部署启动
		
数据库操作
	数据库的命令
        查看当前的数据库：db(没有切换数据库的情况下默认使用test数据库)
        查看所有的数据库：show dbs /show databases
        切换数据库：use db_name
        db_name为show dbs后返回的数据库名
        删除当前的数据库：db.dropDatabase()
    集合的命令
    	无需手动创建集合： 向不存在的集合中第一次添加数据时，集合会自动被		 创建出来
		手动创建集合：
		db.createCollection(name,options)
		db.createCollection("stu")
		db.createCollection("sub", { capped : true, size : 10 		} )
		参数capped：默认值为false表示不设置上限，值为true表示设置上限
		参数size：集合所占用的字节数。 当capped值为true时，需要指定此			参数，表示上限大小，当文档达到上限时， 会将之前的数据覆盖，单位		为字节
		查看集合：show collections
		删除集合：db.集合名称.drop()
		检查集合是否设定上限:db.集合名.isCapped()
		
去重操作:
	db.集合名.distinct(字段)

```

