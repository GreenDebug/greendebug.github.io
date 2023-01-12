

# **easybot**文档

## [框架下载链接](https://1145.s3.ladydaily.com/easybot/easybot_relese_pre8.7z)



本项目是一个机器人框架，旨在帮助那些无法使用官方py库的开发者开发机器人。

运行本框架需要Windows 8 或更高版本，Windows 7需要安装.net core 3.1运行时，若单文件无法运行请去下面的链接下载并安装运行时。

[下载 .NET Core 3.1 (Linux、macOS 和 Windows) (microsoft.com)](https://dotnet.microsoft.com/zh-cn/download/dotnet/3.1)

## 一、目录说明

-  "Commands"  目录为命令目录，你可以将js脚本放在里面

-  "Config" 目录为配置目录

-  "baseCommand" 基本脚本目录

-  "baseCommand/OnLoad" 机器人初始化完毕会执行目录里面的js脚本

-  "baseCommand/UserAddFunc"   用户进入事件，当有用户进来的时候会执行里面的js文件

-   baseCommand/UserLeftFunc"   用户离开事件，当有用户离开的时候会执行里面的js文件

- "baseCommand/MessageFunc"  用户发送事件，当有用户发送的时候会执行里面的js文件(对象为json)

- "baseCommand/WarnFunc"   异常事件，异常会执行里面的js文件。

- "baseCommand/hook" 该目录用于重写:OnMessage,OnOpen,OnError,OnClose事件

- "baseCommand/ChatFunc"     用户发送事件，当有用户发送的时候会执行里面的js文件(对象为data_chat)

UserAddFunc例子:

```js
//UserAddFunc/UserLeftFunc目录下面的只需申明Exports和Exports.run就可以
function RUN(x){
xc.SendMsg(`欢迎${x.nick} 进入聊天室。`);
}
Exports={
run:RUN
};
```
UserLeftFunc例子:
```js
//UserAddFunc/UserLeftFunc目录下面的只需申明Exports和Exports.run就可以
function RUN(x){
xc.SendMsg(`${x.nick} 离开了聊天室。`);
}
Exports={
run:RUN
};
```



#### 注意:

如果在**hook**目录下面**创建js**请重写：**OnMessage,OnOpen,OnError,OnClose事件**，否则将**导致机器人异常**！





## 二、内置对象和函数

**本框架内置Chrome V8 JavaScript 引擎。**

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
    
        
	
    
```

### 跨文件读取全局变量:

```js
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





### 如果要操作系统文件请使用:sysfile

```c#
//C# File常用函数
sysfile.ReadAllText()//读取文件
sysfile.WriteAllText()//写入文件
sysfile.Exist()//检测文件    
```

### 更多的函数请使用搜索引擎搜索:c# File常用函数



上面的代码都可以在js脚本调用。

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
  
  
  
  ## 三、编写命令
  
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
  
  1.Commands脚本必须包含**Exports**变量、**Exports**变量必须包含：
  
  **run**(主函数)、**name**(命令的名字)、**help**(命令帮助)
  
  2.本框架调用主函数时会传入一个**data_chat**对象，如果想获取**用户的信息**可看上面的说明。
  
  



