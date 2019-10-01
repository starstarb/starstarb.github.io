---
title: 每天一道Js基础：AJAX
copyright: true
tags: Javascript
categories: Javascript
abbrlink: 61518bb1
date: 2019-09-29 21:35:44
---
AJAX=> Asynchronous JavaScript and XML   JavaScript执行异步网络请求
<!--more-->
AJAX请求是异步执行的，要通过回调函数获得响应。
在现代浏览器上写AJAX主要依靠XMLHttpRequest对象

**CORS=> Cross-Origin Resource Sharing 跨域访问共享资源**

在引用外域资源时，除了JavaScript和CSS之外，都要验证CORS。

Origin表示本域，也就是浏览器当前页面的域。当JavaScript向外域（如sina.com）发起请求后，浏览器收到响应后，首先检查Access-Control-Allow-Origin是否包含本域，如果是，则此次跨域请求成功，如果不是，则请求失败，JavaScript将无法获取到响应的任何数据。



**一般来说，如果开发API的是自己人的话，让他直接设置CORS就可以了。（我现在项目就是前后端分离，后台就是这样设置。） 如果要请求的API不在自己控制的话，如果对方提供了JSONP（像豆瓣那样的），可以使用JSONP。 最后如果是对方没有提供JSONP，前端又需要用到那方面的API的话，那就需要搭建发现代理服务器咯。 其实我比较倾向于搭建代理服务器~**


JavaScript请求外域(其他网站)的URL的方式：
1. 通过Flash插件发送HTTP请求，这种方式可以绕过浏览器的安全限制，但必须安装Flash，并且跟Flash交互。不过Flash用起来麻烦，而且现在用得也越来越少了。
2. 通过在同源域名下架设一个代理服务器来转发，JavaScript负责把请求发送到代理服务器：
```
'/proxy?url=http://www.sina.com.cn'
```
**代理服务器再把结果返回，这样就遵守了浏览器的同源策略。这种方式麻烦之处在于需要服务器端额外做开发。**

3. 第三种方式称为JSONP，它有个限制，只能用GET请求，并且要求返回JavaScript。这种方式跨域实际上是利用了浏览器允许跨域引用JavaScript资源：
```
<html>
<head>
    <script src="http://example.com/abc.js"></script>
    ...
</head>
<body>
...
</body>
</html>
```
JSONP通常以函数调用的形式返回，例如，返回JavaScript内容如下：

foo('data');

这样一来，我们如果在页面中先准备好foo()函数，然后给页面动态加一个< script >节点，相当于动态读取外域的JavaScript资源，最后就等着接收回调了。

以163的股票查询URL为例，对于URL：http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice，你将得到如下返回：
```
refreshPrice({"0000001":{"code": "0000001", ... });
```
因此我们需要首先在页面中准备好回调函数：
```
function refreshPrice(data) {
    var p = document.getElementById('test-jsonp');
    p.innerHTML = '当前价格：' +
        data['0000001'].name +': ' + 
        data['0000001'].price + '；' +
        data['1399001'].name + ': ' +
        data['1399001'].price;
}
```
最后用getPrice()函数触发：
```
function getPrice() {
    var
        js = document.createElement('script'),
        head = document.getElementsByTagName('head')[0];
    js.src = 'http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice';
    head.appendChild(js);
}
```
就完成了跨域加载数据。




使用AJAX写一个简单的天气查询页面

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>简单的天气查询</title>
</head>
<body>
        <p>输入当前城市:<input type="text" id="city"></p>
        <button type="submit" onclick="getWeather()">查询</button>
        <p id="weatherInfo"></p>
    <script>
        function getWeather(){
            let url = 'https://www.apiopen.top/weatherApi?city=';
            let city = document.getElementById('city');
            // 获取要查询的城市
            let newURL = url + city.value; 
            console.log(newURL);
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    if(xhr.status === 200){
                        return success(xhr.responseText);
                    }else{
                        alert('失败！')
                    }
                }
            }
            xhr.open('GET',newURL);
            xhr.send();
        }

        function success(data){
            let weather = document.getElementById('weatherInfo');
            let weatherInfo = JSON.parse(data);
            if(weatherInfo.code ===  200){
                weather.innerHTML = '查询成功' + '<br>'+
                                     '当前城市：'+weatherInfo.data.city+ '<br>'+
                                     '当前温度：'+weatherInfo.data.wendu+'<br>'+
                                     '气温：'+'最'+weatherInfo.data.forecast[0].high +'，最'+weatherInfo.data.forecast[0].low+'<br>'+
                                     '天气：'+weatherInfo.data.forecast[0].type+'<br>'+
                                     '风向：'+weatherInfo.data.forecast[0].fengxiang + weatherInfo.data.forecast[0].fengli+'<br>' +
                                     '注意：'+weatherInfo.data.ganmao
            }else{
                weather.innerText  = weatherInfo.msg;
            }
        }
    </script>
</body>
</html>
```

__注意:__
price.src= 'http://api.money.126.net/data/feed/0000001,1399001?callback=showPrice';的含义应该是将http://api.money.126.net/data/feed/0000001,1399001返回的结果传入回调函数showPrice()中, 然后showPrice()负责显示:

```
<!doctype>
<html>
<head>
    <meta charset='utf-8'></meta>
    <script>
        //data即getPrice中price.src传入的返回结果, 是一个JSON字符串
        function showPrice(data){
            var p=document.getElementById("test-jsonp");
            p.innerHTML="当前价格："+
                data['0000001'].name +': ' +
                data['0000001'].price + '；' +
                data['1399001'].name + ': ' +
                data['1399001'].price;
        }

        //点击"刷新"按钮时, 从'http://api.money.126.net/data/feed/0000001,1399001'请求JSON字符串,并把请求结果传给自定义函数showPrice()
        function getPrice(){
            var price=document.createElement('script');
            var head=document.getElementsByTagName("head")[0];
            price.src= 'http://api.money.126.net/data/feed/0000001,1399001?callback=showPrice';
            head.appendChild(price);
        }
    </script>
</head>

<body>
<p id="test-jsonp">placehoder</p>
<button type='button' onclick="getPrice()">刷新</button>
</body>
</html>
```