# 普通登录（已经登录过系统的可以用这个接口快速登录）

    KeyValue param = new KeyValue();
    
    param["username"]=username
    param["password"]=password

    JSON ret = CURL.Post("http://go.bilihp.com:180/v1/index/login/3",param);
    
这里就不多介绍了

ret的返回值为0或者不为0，不为0的时候data里面是错误信息string类型的


如果为0，data中的数据就是这样的

    STRING username=ret.data["username"];
    STRING token=ret.data["token"];
    
    
将username和token保存即可，至此登录部分全部结束