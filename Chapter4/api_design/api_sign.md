# 接口签名验证

防止参数被修改







## postman 如何自动生成签名

***放在Pre-request-Script处的代码***

postman的console使用快捷键  : ctrl + alt + c

~~~ js
(function () {
    pm.globals.set("timestamp",Math.round(new Date().getTime()/1000).toString()); // 设置全局变量 timestamp
    // 这段脚本兼容GET参数、以及表单形式的BODY和JSON形式的BODY
    let queryParam = pm.request.url.query.members;
    let param = request.data;
    var data = request.data;
    try {
        let json = JSON.parse(param); //序列化JSON BODY
        param = json;
    }catch(err){
        //BODY不是JSON格式
    }

    //Get & 合并GET和POST参数
    for (let i in queryParam){
        param[queryParam[i].key] = queryParam[i].value;
    }
    
    // 取值的是全局，还是环境变量， 下标和你设定的保持一致
    var signKey = pm.environment.get("appKey");
    console.log(signKey)
    // 这里的值，直接在 request params 里面用{{}}取，上面的js无法取到，只能通过以下方式
    param['timestamp'] = pm.globals.get('timestamp');
    param['token'] = pm.environment.get('token');
    param['v'] = pm.environment.get('v');
    
    var sign = calcSign(param, signKey);
    pm.globals.set('api_sign', sign);// 设置全局变量api_sign
})();

function calcSign(data, signKey) {
    console.log(data)
    delete data['api_sign'];
    var keys = [];
    for(var k in data) {
        
        keys.push(k);
    }
    keys.sort();
    var kv = [];
    for (var v of keys) {
        console.log(k + ' --- ' + data[v])
        kv.push(v + '=' + encodeURIComponent(data[v]));
    }
    let kvStr = kv.join('&') + signKey;
    console.log('herer')
    console.log(kvStr)
    var sign =  CryptoJS.MD5(kvStr).toString(CryptoJS.enc.Hex); 
    return sign;
    
    // var sign_sha1 =  CryptoJS.SHA1(kvStr).toString(CryptoJS.enc.Hex); 
    // return sign_sha1;
}  
~~~























[参考 1](https://blog.csdn.net/qq598535550/article/details/81276594)

[参考 postman 接口自动化，环境变量的自动设置](https://www.cnblogs.com/JHblogs/p/6418802.html)

[参考 设置全局api-sign](https://blog.csdn.net/tomoya_chen/article/details/86312605)