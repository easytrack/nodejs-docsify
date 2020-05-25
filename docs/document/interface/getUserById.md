## 获取单个用户信息

> 总路径：/mobile/v1_0/user

- 地址：`/fetchuserinfo`
- 演示地址：[localhost:8899//mobile/v1_0/user/fetchuserinfo?uid=0001](localhost:8899//mobile/v1_0/user/fetchuserinfo?uid=0001)
- 请求方式：`GET`
- 请求参数


|字段|说明|是否必须|类型|
|---|---|---|---|
| `uid` |用户ID|`Y`|String|

- 返回参数 

``` json
{
    "status": "0001",
    "msg": "success",
    "data": {
        "uname": "TigerChain",
        "age": 28,
        "sex": "男",
        "address": "中国陕西",
        "phone": "1**********"
    }
}
```

- 返回字段说明

|返回值字段|字段说明|字段类型|备注|
|---|---|---|---|
|`status`|`状态码`|`String`|`0001` 表示请求成功|
|`msg`|`状态对应的信息`|`String`|有成功失败等信息|
|`data`|`返回数据json对象`|`jsonObject`|是一个对象|
|`uname`|`用户姓名`|`String`|data 中的字段|
|`...`|`...`|`...`|`...为了显示，其它省略`|