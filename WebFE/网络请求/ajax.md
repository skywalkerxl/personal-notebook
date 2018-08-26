# 异步的JavaScript和XML

首先列一下知识点:

1. 如何创建一个ajax
2. dataType的类型
3. ajax的生命周期
4. 原生ajax的实现
5. React的ajax为什么要放在componentDidMount里面？

### 1. 如何创建一个ajax

简单来说为先创建一个xhr对象，然后open， 监听readystate变化，然后send

``` javascript
let _w = {
    ajax: function(options){
        let settings = {
            type: "get",
            async: false
        }
        for(let key in options){
            settings[key] = options[key] || settings[key];
        }
        
        let xhr = new XMLHttpRequest();
        
        xhr.open(url, type, settings["async"]);
        
        xhr.readystatechange = function(){
            if(xhr.readyState === 4){
                if(xhr.status === 200){
                    settings.complete(xhr.responseText);
                }
            }
        }
        
        xhr.send();
    }
}
```



### 2.dataType的类别

datatype的类别,也是接收到的数据类型，为下面几种之一

1. json
2. jsonp
3. xml
4. html
5. script
6. text

### 3. ajax的生命周期

ajax的生命周期，分为下面几个步骤:

1. uninitialized --- 请求尚未初始化
2. loading --- 请求已经初始化完成
3. loaded --- 请求已经发送，正在等待确认
4. interactive --- 正在从服务端下载相应数据
5. complete --- 请求结束

### 4. jQuery的ajax实现

### 5. React的ajax为什么要放在componentDidMount里面？ 

[React数据获取为什么一定要在componentDidMount里面调用？](https://segmentfault.com/q/1010000008133309)