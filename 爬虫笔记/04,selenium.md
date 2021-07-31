# selenium(自动测试框架)

selenium是一个web的自动化测试工具

浏览器

​	开发使用有头浏览器,部署用无界面浏览器

​	

原理

​	代码-调用webdriver-操作浏览器

​	不同的浏览器使用各自不同的driver

selenium 简单使用

​	1,创建浏览器对象

​		driver = webdriver.Chrome()

​	2,发送请求

​		driver.get(url)

​	3,元素定位

```python
driver.find_element_by_id('username') # 通过 id 属性定位 
driver.find_element_by_name('password2')  # 通过 name 属性定位
driver.find_element_by_class_name('input-password') # 通过 class 属性定位 
driver.find_element_by_tag_name('ul') # 通过标签名定位
driver.find_element_by_link_text('标签') # 通过内容定位 a 标签(绝对匹配)
driver.find_element_by_partial_link_text('Optimization')  # 通过内容定位 a 标签(模糊匹配)
```

​	4,其他使用

```
窗口切换:
	获取窗口句柄
		指向标签页对象的标识
		driver.window.handles
	通过窗口句柄切换标签
```

控制浏览器执行js代码

````
x 水平移动  y垂直移动
driver.execute_script('scrollTo(x,y)')
````

页面等待

```
1,强制等待
	time.sleep()
2,隐式等待(常用)
	driver.implicitly_wait()	
3,显式等待(了解)
```

