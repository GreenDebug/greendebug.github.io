

# **easybot**文档

## [框架下载链接](https://1145.s3.ladydaily.com/easybot/easybot_relese-pre10.7z)



**本项目是一个机器人框架，旨在帮助那些无法使用官方py库的开发者开发机器人。**



## 一、目录说明

-  **Commands  目录为命令目录，你可以将js脚本放在里面**
-  **Config 目录为配置目录**
-  **baseCommand 基本脚本目录**
-  **baseCommand/OnLoad 机器人初始化完毕会执行目录里面的js脚本**
-  **baseCommand/UserAddFunc   用户进入事件，当有用户进来的时候会执行里面的js文件**
-  **baseCommand/UserLeftFunc  用户离开事件，当有用户离开的时候会执行里面的js文件**
-  **baseCommand/MessageFunc  用户发送事件，当有用户发送的时候会执行里面的js文件(对象为json)**
-  **baseCommand/ChatFunc     用户发送事件，和上面那个一样(对象为data_chat)**
-  **baseCommand/WarnFunc   异常事件，异常会执行里面的js文件。**

## 注意:

**如果只编写命令的ChatFunc和MessageFunc文件夹请勿创建js文件，否则将导致机器人刷屏！**





## 二、内置对象和函数

### 内置函数:



```c#
    xc.SendMsg(string uname) //发送公开消息
    xc.SendMsgTo(string uname,string sendto) //私信
    xc.MoveTo(string channel)//换房间
    xc.ChangeName(string uname)//更换名字
    xc.Restart()//重载机器人
    xc.SetHeader(string url)//设置头像
    bot.AddObject(string name,object obj)//添加一个全局变量
    bot.GetObject(string name)//返回对应的全局变量
    bot.RemoveObject(string name)//移除一个全局变量
    bot.Exit()//自裁
    bot.Success(string text)//输出成功信息
    bot.Error(string text)//输出错误信息
    bot.Info(string info)//输出详细信息
    console.log(string text)//输出到控制台
    console.Obj(object obj)//用于调试
    xc.AddOP(string trip)//添加op
    xc.RemoveOP(string trip)//移除op
    xc.IsinOPList(string trip)//检测是否在oplist里面
    xc.GetUserInfo(string uname)//通过用户名获取用户信息
    net.Get(string url)//get 操作
    net.Post(string url,object obj)//post 操作
    
        
	
    
```

### 跨文件读取全局变量:

```js
//demo.js
function Run(x){
     bot.AddObject("homo","1919810");//注意：这里的第一个参数是名字，下面读取的时候名字要一样否则读取失败。
    xc.SendMsg("变量写入成功。");
}
Exports={
    run:Run,
    name:"write",
    help:"示例脚本"
    
};
```

```js
//demo2.js
function Run(x){
    xc.SendMsg(xc.GetObject("homo"));
}
Exports={
    run:Run,
    name:"read",
    help:"示例脚本"
    
};
```

## 覆盖框架help:

```js
//把下面的代码复制保存到exe目录/Commands/help.js
//如果需要可自行修改help
function demo(x){
  var element=Commands;
  //console.log(element.length);

  var msg=".\r\n|命令|帮助|\r\n|--|--|\r\n";
 
  for(var i=0;i<element.Count;i++){
    msg+=`|${element[i].name}|${element[i].help}|\r\n`;
  }

   
  xc.SendMsgTo(msg,x.nick);
 //console.log(commands[0].name.toString());
}
Exports={
    run:demo,
    name:"help",
    help:"测试覆盖help",
  

};

```





## 文件操作:

注意:此文件只能操作**confing目录**下面的**目录/文件**

```c#
	public class FileConfig
        {
        	
            file.Exist(string _path)//检测文件是否存在
            file.ExistDir(string _path)//检测目录是否存在
            file.WriteText(string name, string text)//写入文件
            file.DeleteFile(string name)//删除文件
            file.WritePush(string name, string push)//向文件续写
            file.ReadFile(string name)//读取文件
            file.CreatDir(string name)//创建文件夹
          
        }
```



## 写入txt/读取txt/文件示例:

```js
//writedemo.js
//注意：下面的写入/读取的路径都在/config/文件夹的里面
function Run(x){
    file.WaiteText("demo.txt","你好");
    if(file.Exist("demo.txt")==true){
    xc.SendMsg("写入成功。");
    }
    
     xc.SendMsg("尝试读取文件");
    if(file.Exist("demo.txt")==true){
		xc.SendMsg(`读取的结果为:${bot.ReadFile("demo.txt")}`);
}
}
Exports={
    run:Run,
    name:"read",
    help:"示例脚本"
    
};
```

### 如果要操作系统文件请使用:sysfile

```c#
//C# File常用函数
sysfile.ReadAllText()//读取文件
sysfile.WriteAllText()//写入文件
sysfile.Exist()//检测文件    
```

### 更多的函数请使用搜索引擎搜索:C# File常用函数

### 下面是一些例子:

#### **UserAddFunc**例子

**(UserAddFunc文件夹创建js文件):**

```js
//UserAddFunc/UserLeftFunc目录下面的只需申明Exports和Exports.run就可以
function RUN(x){
xc.SendMsg(`欢迎${x.nick} 进入聊天室。`);
}
Exports={
run:RUN
};
```

#### **UserLeftFunc**例子:

**(UserLeftFunc文件夹创建js文件):**

```js
//UserAddFunc/UserLeftFunc目录下面的只需申明Exports和Exports.run就可以
function RUN(x){
xc.SendMsg(`${x.nick} 离开了聊天室。`);
}
Exports={
run:RUN
};
```



### 内置变量:

- users

- nicks

- Commands

  users变量为用户列表

  nicks为呢称列表

  Commands为命令列表

  

  ### 内置类型：
  
  ### UserInfo类
  
  ```c#
  public class UserInfo
          {
              
              public string nick { get; set; }
              public string trip { get; set; }
              public string utype { get; set; }
              public string hash { get; set; }
              public string userid { get; set; }
              public string level { get; set; }
          }
  ```

  
  
  
  
  js获取:
  
  ```js
  function Run(x){
      var arg=x.text.split(' ');
      if(arg.length>=1){
      	for(var i=0;i<users.Count;i++){
  		if(arg[0]==users[i].nick){
              xc.SendMsg(`
              用户信息:
              呢称：${users[i].nick}
              识别码：${users[i].trip}
              hash：${users[i].hash}
              `);
          }
          }    
      }
      
  }
  Exports={
      run:Run,
      name:"getuserinfo",
      help:"获取用户信息demo"
  };
  
  ```
  
  ### data_chat类
  
  ```c#
    public class data_chat
          {
              
              public string head { get; set; }
              public string nick { get; set; }
              public string show { get; set; }
              public string text { get; set; }
              public string trip { get; set; }
             
          }
  ```
  
  **head**为头像地址，**nick**为呢称，**text**为信息，**trip**为识别码
  
  下面是一个例子:
  
  ```js
  function Run(x){
      var name=x.nick;
      var trip=x.trip;
     xc.SendMsg(`用户名:${name},识别码:${trip}`);
  }
  Exports={
      run:Run,
      name:"demo",
      help:"示例"
      
  };
  
  ```
  
  ## 三、网络操作
  
  ### 基本get/post方法
  
  ```js
  //get.js
  function Run(x){
      var arg=x.text.split(' ');
      if(arg.length>=2){
         xc.SendMsg(net.Get(arg[1]));//get 需要一个参数 url
      }
  }
  Exports={
      run:Run,
      name:"get",
      help:"示例脚本"
      
  };
  ```
  
  ```js
  function Run(x){
      var arg=x.text.split(' ');
      if(arg.length>=2){
         xc.SendMsg(net.Post(arg[1],{"a":"b"}));//post需要两个参数 url data
      }
  }
  Exports={
      run:Run,
      name:"post",
      help:"示例脚本"
      
  };
  ```
  
  
  
  
  
  ## 四、编写命令
  
  
  下面是一个最简单的例子:
  
  ```js
  function Run(x){
      xc.SendMsg("Hello World!");
  }
  Exports={
      run:Run,
      name:"hello",
      help:"示例脚本"
      
  };
  
  ```
  
  #### 注意:
  
  Commands脚本必须包含**Exports**变量、**Exports**变量必须包含：
  
  **run**(主函数)、**name**(命令的名字)、**help**(命令帮助)
  
  
  
  
  
  



