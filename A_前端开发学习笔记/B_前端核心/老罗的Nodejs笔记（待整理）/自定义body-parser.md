```js
    const qs = require("querystring");
    // 闭包用在哪里?
    // express,引入中间件的时候,就是用闭包
    module.exports = {
        urlencoded : function(){
    
            //重点就是这个闭包
            return function(req, res, next){
                let str = "";
                req.on("data", function(data){
                    str += data;
                })
                req.on("end", function(){
                    req.body = qs.parse(str)
                    next();
                })
            }
    
        }
    }

```

https://www.cnblogs.com/l8l8/p/9097452.html