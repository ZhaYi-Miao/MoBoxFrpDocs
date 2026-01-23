# MoBoxFrp API 文档

API 调用地址：https://www.moboxfrp.top/API/

!> 所有 API 均采用 `POST` 请求  
Body 统一为 `application/json`

## 注意事项

1. WebAPI 存在小概率请求失败情况，建议失败后延迟重试 2–3 次  
2. 所有接口均需携带 Token（除 Static）  

## Login登录请求

使用API必须的请求，会返回一个token，后续不管什么操作都要使用

URL：
`https://www.moboxfrp.top/API/Login`

### 特殊返回码

"404" 账号或密码错误

### 发送内容

```
  {
    "loginType": "email",         #登录方式 email/phone
    "account": "822585140@qq.com",#账户信息
    "password": "qwer1234"        #密码
  }
```

### 接收内容

```
  {
    "success":true, #是否登录成功
	"message":"",   #返回的消息，例如“操作失败！服务异常！”
	"token":"xxx"   #返回的token，应定义为全局变量，全程都要用
  }
```

### 参考用法（以下所有同）

``` JavaScript
fetch("/Login", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    loginType: "email",
    account: "822585140@qq.com",
    password: "qwer1234"
  })
}).then(res => res.json()).then(data => console.log(data));
```

### 参考返回

```
  {
    "success": true,
    "message": "",
    "token": "MTAwMDIwNTI2YjA5YmIwZDB9YmE0MtJiYjI2YzhhMzY3NWRjNDA2ODEwNC4yMzguMjIwLjE5NTE3NjkxNzY3NTkxNzQ="
  }
```

## UserCodeList - 获取用户所有穿透码

**URL：**  
`https://www.moboxfrp.top/API/UserCode/List`

### 请求

```json
{
  "token": "xxx"
}
```

### 返回

```
{
  "codes": [
    {
      "codeID": "38865",               #穿透码编号，仅为区分穿透码使用
      "node": "sq5",                   #节点编号
      "number": "6532740",             #穿透码编号，用于操作穿透码
      "portServer": "47066",           #frps端口，
      "portOpen": "47067-47075",       #可用端口
      "band": "1",                     #带宽，单位Mbps
      "status": "running",             #穿透码状态，running为正常运行，outdated为到期，banned为封禁
      "token": "3sq526243733096558983",#穿透码
      "timeOutdate": "1769338874795"   #过期时间
    }
  ],
  "success": true,
  "message": ""
}
```

## NodeList - 获取全部节点信息

URL：
`https://www.moboxfrp.top/API/Node/List`

### 请求
```
{
  "token": "xxx"
}
```

### 返回
```
{
  "nodes":[
    {
	  "cpuUsage":"0.05",					 #cpu使用率
	  "memoryUsage":2.830036163330078,       #内存使用率
	  "memoryMax":7.998970031738281,         #最大内存
	  "bandUploadTotal":132.13197880610824,  #节点总上传
	  "portStart":"45000",                   #可选开始端口
	  "bandUpload":30.546953125,             #上传带宽
	  "userCount":17,                        #现在用户总量
	  "load":61.09390625000001, 			 #节点当前负载
	  "phone":"true",                        #是否需要绑定手机
	  "provider":"10000000",				 #节点提供者
	  "bandDownload":38.1387265625,          #下载带宽
	  "enable":"true",  					 #收否启用
	  "price":"37",                          #单价 带宽/mbps/天
	  "name":"宿迁5节点-三线-30Mbps",          #节点名字
	  "bandDownloadTotal":151.85208675079048,#节点总下载
	  "online":"true",                       #是否在线
	  "portEnd":"48000",                     #可选结束端口
	  "nodeID":"sq5",                        #节点编号
	  "coin":"mixed"#这个节点可以使用的硬币类型 mixed银/金币都可，silver银币，gold金币
	},#（下面同上）
	{
	  "cpuUsage":"0.01",
	  "memoryUsage":0.7137603759765625,
	  "memoryMax":3.7006149291992188,
	  "bandUploadTotal":3.8100124755874276,
	  "portStart":"41000",
	  "bandUpload":0.117890625,
	  "userCount":9,
	  "load":9.643807713479312,
	  "phone":"true",
	  "provider":"10000146",
	  "bandDownload":0.136046875,
	  "enable":"true",
	  "price":"38",
	  "name":"宿迁2节点-BGP-30Mbps",
	  "bandDownloadTotal":4.711762698367238,
	  "online":"true",
	  "portEnd":"43000",
	  "nodeID":"sq2",
	  "coin":"mixed"
	}#（还会有更多节点省略掉了）
  ],
  "success":true,
  "message":""
}
```

## UserInfo - 用户详细信息

URL：
`https://www.moboxfrp.top/API/UserInfo`

### 请求
```
{
  "token": "xxx"
}
```

### 返回
```
{
  "userID": "10002052",       #用户id
  "username": "ZhaYi",        #用户名
  "email": "822585140@qq.com",#用户邮箱
  "qq": "822585140",          #用户qq号
  "permission": "provider",   #用户权限组
  "gold": "174117",           #金币数量
  "silver": "1140601",        #银币数量
  "success": true
}
```

## SignInCheck - 签到状态

URL：
`https://www.moboxfrp.top/API/SignIn/Check`

### 请求
```
{
  "token": "xxx"
}
```

### 返回
```
{
  "success": true,
  "signed": true,                       #是否签到
  "luck": 13,                           #幸运值
  "coin": 246,                          #签到获得到的银币
  "message": "喵~勉强及格啦！今天要加油哦！" #返回的发电msg
}
```

## Notice - 系统公告

URL：
`https://www.moboxfrp.top/API/Notice`

### 请求
```
{
  "token": "xxx"
}
```

### 返回
```
{
  "notices": [
    {
      "date": "2026-01-21",                                   #公告发布日期
      "title": "MoBoxFrp重构公告",                             #公告标题
      "content": "MossFrp已于2026-01-21基本完成技术变更与升级...",#公告内容
      "weight": 10                                            #公告权重
    }
  ],#（实际获取可能不止一个公告，同理）
  "success": true
}
```

## Static - 全站统计（公开）

URL：
`https://www.moboxfrp.top/API/Static`

### 请求
```
{}
```

### 返回
```
{
  "userCount": 18934, #已注册用户数
  "nodeCount": 9,     #在线节点总数
  "signInCount": 51,  #今日签到用户总数
  "codeCount": 55,    #在线隧道总数
  "success": true  
}
```

## UserCodeCreate - 创建穿透码

URL：
`https://www.moboxfrp.top/API/UserCode/Create`

### 请求
```
{
  "token": "xxx",
  "node": "gz1",
  "band": "1",
  "day": "3"
}
```

### 返回
```
{
  "success": true,
  "message": ""
}
```

## UserCodeRenew - 续费穿透码

URL：
`https://www.moboxfrp.top/API/UserCode/Renew`

### 请求
```
{
  "token": "xxx",
  "codeID": "38865",
  "day": "3"
}
```

### 返回
```
{
  "success": true,
  "message": ""
}
```

## UserCodeUpgrade - 升配穿透码

URL：
`https://www.moboxfrp.top/API/UserCode/Upgrade`

### 请求
```
{
  "token": "xxx",
  "codeID": "38885",
  "band": "1"
}
```

### 返回
```
{
  "success": true,
  "message": ""
}
```

## UserCodeDelete - 删除穿透码

URL：
`https://www.moboxfrp.top/API/UserCode/Delete`

### 请求
```
{
  "token": "xxx",
  "codeID": "38865"
}
```

### 返回
```
{
  "success": true,
  "message": ""
}
```
