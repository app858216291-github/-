# request模块

```
作用:
	发送http请求,获取响应数据

发送get请求
	1,导入模块
	2,response = request.get(url)
	3,print(response.text)
		(response.text按照chardet解码 str)
		(网络传输都是bytes类型)
		
		(response.encodeing = 'utf8')
		(response.text)
		
		(response.text = response.content,decode)

```

# 携带请求头的请求

```
headers{自定义请求头}

response = request.get(url, headers=headers)
```

# 发送带参数的请求

```
1,url中直接带参数
2,构建请求参数字典 params=data
3,在headers中携带cookies
4,在请求时将cookies字典赋值给cookies参数 cookies = cookies
	cookie转换成字典方法:
		cookie_dict=request.utils.dict_from_cookiejar(request.cookies)
		
5,超时参数timeout,默认180s
	timeout=时间(秒 )
6,使用代理ip,使用proxies参数
	指向一个代理服务器,转发请求
	透明代理,匿名代理,高匿代理
	proxies= {
        '协议': '代理ip地址:端口'
	}
	(快代理,无忧代理,代理精灵,米扑代理)
7,使用verify参数忽略CA证书
	verify = False
```

# request模块发送post请求

```
1,实现方法:
	response = request.post(url,data)
		data是一个字典参数
		
2,post数据来源:
	固定值			抓包比较不变值
	输入值			抓包比较根据自身变化值
	预设值-静态文件			需要提前从静态html中获取
	预设值-发请求				 需要对指定地址发送请求 
	在客户端生成				分析JavaScript,模拟生成数据
	
	
```


