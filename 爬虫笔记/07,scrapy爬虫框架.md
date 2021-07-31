# scrapy爬虫框架

```
scrapy是一个被设计用于爬取网络数据,提取结构性数据的框架
```

![image-20210106124741855](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210106124741855.png)

```
其流程可以描述如下：

    爬虫中起始的url构造成request对象-->爬虫中间件-->引擎-->调度器
    调度器把request-->引擎-->下载中间件--->下载器
    下载器发送请求，获取response响应---->下载中间件---->引擎--->爬虫中间件--->爬虫
    爬虫提取url地址，组装成request对象---->爬虫中间件--->引擎--->调度器，重复步骤2
    爬虫提取数据--->引擎--->管道处理和保存数据
```

```
 scrapy的三个内置对象
    request请求对象：由url method post_data headers等构成
    response响应对象：由url body status headers等构成
    item数据对象：本质是个字典
```

```
scrapy项目开发流程
	创建项目
		scrapy startproject <项目名称>
		各文件的作用
	创建爬虫
		scrapy genspider <爬虫名字><允许爬取的域名> 
	完成爬虫
		修改起始的url
		检查修改允许的域名
		在parse方法中实现爬取逻辑
	运行爬虫
		scrapy  crawl <爬虫名字>
	提取数据
	保存数据
		在pipelines.py文件中定义对数据的操作
			定义一个管道
		在settings.py中配置启动管道
		
	数据建模
		在items.py文件中定义
		在spider中导入,实例化,使用
构建请求对象
	url
	scrapy.Request()	
        请求类参数
            *url
            *callback
            *meta
            dont_filter
            cookies
            headers
            
            
scrapy模拟登录
	cookie参数
	模拟登录
```

serapy 管道

```
pipeline中常用的方法：
    process_item(self,item,spider):
        管道类中必须有的函数
        实现对item数据的处理
        必须return item
        
    open_spider(self, spider): 在爬虫开启的时候仅执行一次

    close_spider(self, spider): 在爬虫关闭的时候仅执行一次
```

crawlspider

```
	继承自 Spider 爬虫类

​	自动根据规则提取链接并且发给引擎

​	创建crawlspider爬虫
```

scrapy 中间件

```
分类
	下载中间件
	爬虫中间件
作用
	对beader以及cookie进行更换和处理
	使用代理ip等
	对请求进行定制化操作
位置
	默认写在middlewares.py文件中
	
下载中间件
	process_request:
	None		如果下载器所有都返回None,则跳转到下载器
	request		如果返回为请求,则将请求交给调度器
	response	将响应对象交给spider进行解析
	
	process_response:
	request		如果返回为请求,则将请求交给调度器
	response	将响应对象交给spider进行解析
	
```

# scrapy_redis 

```
分布式 
	加快项目运行速度,但是需要资源
	
scrapy_redis的作用
	通过持久化请求队列和请求的指纹集合来实现;
	断点续爬
	分布式快速抓取
	
scrapy_redis爬虫和普通爬虫的区别
	导入类不同
	继承类不同
	注销 start_urls & allowed_domains
	redis-key
	__init__
	
分布式爬虫的编写流程:
	1,创建普通爬虫
		创建项目
		明确目标
		创建爬虫
		保存内容
	2,改造成分布式爬虫
		改造爬虫
            导入scrapy_redis的分布式爬虫类
            继承类
            注销 start_urls & allowed_domains
            设置redis-key获取start_urls
            设置__init__获取允许的域
		改造配置文件
			copy配置参数(5个)
			
			
分布式爬虫总结
	使用场景
		数据量特别巨大
		数据要求时间比较紧张
	分布式的实现
		scrapy_redis 实现分布式
		普通爬虫实现分布式,实现去重集合与任务队列的共享
	分布式的部署
		多台普通电脑
		一台服务器虚拟若干台电脑
		
爬虫的抓包
```

