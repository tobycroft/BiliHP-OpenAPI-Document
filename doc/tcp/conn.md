# 连接前准备

首先在你连入bilihp网络前，你需要知道tcp数据传输，是基于流，也就是你经常看见的stream，因为是流，所以没有"截断"这么一说


所以正常在处理流的时候，接入端需要自行合并数据流，并且自行完成拆粘包动作


在连入的部分，你可以使用IPV4或者IPV6进行连接


连接地址为：go.bilihp.com:181


## 拆粘包处理

首先如果需要进行拆粘包处理，那么你就需要准备2个缓存一个是tcp缓存一个是string缓存

    ByteBuffer buf = new ByteBuffer(4096);

    STRING temp = "";
    
    TCP conn = new TCP("go.bilihp.com:181");
    
    for{
        if (conn.connected()==null){
            //网路断开就结束掉
            break;
        }
        temp+=conn.readString()
        
    }
    
    
这里大家可以想象一下因为接收到的都是一段一段的json，所以json头顶着json尾

就会是这样:
    
    JSON str={"code":"123","data":"456"}{"code":"11","data":"222"}{"code":"1771","data":"1"}
    
那么要怎么区分呢，这里以js代码为例
    
    function JsonSplitor2(json) {
        var arr = new Array();
        //创建一个数组，一会会将拆解的json分成完整的一段一段的数据
        
        //在这里判断数据长度，如果数据太长了，说明之前的解析里面存在脏的永远都不可能正常解析的数据，所以要将缓存全部清空
        //这里也是一个防错处理
        if (json.length > 655350) {
            localstorage.setStorage('__temp__', "");
            return arr;
        }
        //首先先找到两个json的分界点也是"}{"的位置
        var parts = json.split("}{");
        
        //在这里先判断之前切出来了几段，如果length大于1，说明切出来了的数量超过两段
        //那么这就是粘包的情况，我们要对数据进行拆包
        if (parts.length > 1) {
       //先创建一个不能被解析的unable string，用于存储无法被解析的数据
            var unable = "";
           //因为这个时候数量一定是2个以上的，所以for循环中一定会触发一个if和一个else
            for (var i in parts) {
            //先判断数据的位置
                if (i == 0) {
                //如果是在0的位置上，那么补充一个}右括号，因为split的时候会去掉}符号，所以这里要补上
                    var t = parts[i] + "}";
                    //判断json是否可以被解析，如果可以被解析就push过去
                    if (isJson(t)) {
                        arr.push(t);
                    } else {
                    //如果无法被解析，那么就将这个不能被解析的string加到unable里面去
                        unable += t;
                    }
                } else if (i == parts.length - 1) {
                //如果只有两个，那么第二个被触发的就是这个，但是可能会出现故障
                //类似{"code":"123","data":"456"}{"co这样的第一个完整第二个不完整的json
                //这里处理的就是第二个可能完整可能不完整的string
                    var t = "{" + parts[i];
                    //先补充{左括号
                    if (isJson(t)) {
                    //能解析就推到数组
                        arr.push(t);
                    } else {
                    //不能解析就将不能解析的内容添加回unable
                        unable += t;
                    }
                } else {
                //这里也是一样补充完整，成功就推不成功就不推
                    var t = "{" + parts[i] + "}";
                    if (isJson(t)) {
                        arr.push(t);
                    } else {
                        unable += t;
                    }
                }
            }
            //最后这里将不能解析的内容返回到temp字段，等待下次加载
            localstorage.setStorage('__temp__', unable);
            return arr;
        } else {
        //如果切出来的数量不足1，那么这有可能是一段完整的json，也有可能是不完整的json
        //例如{"code":"123","dat这样的不完整状态
        //所以这里需要判断一下当前的json是否为一个合法有效的json
            if (isJson(json)) {
                var unable = "";
                localstorage.setStorage('__temp__', unable);
                //如果这个json是合法的json那么就将这个json推到之前创建的数组里面，并且清空当前的string 缓存
                arr.push(JSON.parse(json));
            }
            return arr;
        }
        return arr;
    }



当然在Go语言当中的拆包方法和JS略有不同

    func TCPJObject(temp *string) ([]map[string]interface{}, error) {
        var arr []map[string]interface{}
    
        var json = jsoniter.ConfigCompatibleWithStandardLibrary
        //var strs []string
    
        data := *temp
        if len(*temp) > 65535 {
            *temp = ""
            return nil, fmt.Errorf("%s", "too long")
        }
        strs := strings.Split(data, "}{")
        if len(strs) > 1 {
            unable := ""
            for i, v := range strs {
                arr2 := make(map[string]interface{})
                if i == 0 {
                    err := json.Unmarshal([]byte(v+"}"), &arr2)
                    if err != nil {
                        unable += v + "}"
                        fmt.Println(1, i, i+1, v+"}")
                    } else {
                        arr = append(arr, arr2)
                    }
                } else if len(strs) == int(i+1) {
                    err := json.Unmarshal([]byte("{"+v), &arr2)
                    if err != nil {
                        unable += "{" + v
                        fmt.Println(2, i, i+1, "{"+v)
                    } else {
                        arr = append(arr, arr2)
                    }
                    //fmt.Println(2, "len", len(strs), i+1, "{"+v)
                } else {
                    err := json.Unmarshal([]byte("{"+v+"}"), &arr2)
                    if err != nil {
                        unable += "{" + v + "}"
                        fmt.Println(3, i, "{"+v+"}")
                    } else {
                        arr = append(arr, arr2)
                    }
                }
            }
    
            *temp = unable
            return arr, nil
        } else {
            arr2 := make(map[string]interface{})
            err := json.Unmarshal([]byte(data), &arr2)
            if err != nil {
                //fmt.Println("2",data)
                //fmt.Println(err)
                return nil, nil
            } else {
                *temp = ""
                arr = append(arr, arr2)
                return arr, err
            }
        }
    }


如果你学会了拆粘包的方法后，就可以顺利接收到bilihp的网络消息了

	temp := ""
	buf := make([]byte, 4096)
	for {
		n, err := Conn[username].Read(buf)
		if n == 0 || err != nil {
			wg.Done()
			fmt.Println("handler出错:", err)
			return

			//将当前用户从在线字典中删除
			//address := strings.Split(addr, ":")
			//ip := address[0]
			//Conclose(conn, addr, ip)
			//通知其他客户端该用户退出登录
			//Messages <- "logout"
			//Send_All("logout")
			return
		}
		temp += string(buf[:n])
		jsons, err := Jsong.TCPJObject(&temp)
		if err != nil {
			fmt.Println("err:", err)
		} else {
			for _, jobject := range jsons {
				go ActionRoute.ActionRoute(jobject, username, Conn[username])
			}
		}

	}
	