## 获取单个用户信息

> 总路径：/mobile/v1_0/user

- 地址：`/getUserById`
- 演示地址：[http://localhost:9000/mobile/v1_0/user/getUserById?uid=0001](http://localhost:9000/mobile/v1_0/user/getUserById?uid=0001)
- 请求方式：`GET`
- 请求参数


|字段|字段说明|是否必须|字段类型|备注|
|:-:|---|:-:|:-:|---|
| `uid` |`用户ID`|`Y`|`String`|用户唯一标志|

- 返回参数 

``` json
{
    "returnCode": "0001",
    "returnMsg": "success",
    "returnData": {
        "uname": "TigerChain",
        "age": 28,
        "sex": "男",
        "address": "中国陕西",
        "phone": "1**********"
    }
}
```

- 返回字段说明

|    返回值字段    | 字段说明               |     字段类型     | 备注                             |
| :--------------: | :--------------------- | :--------------: | :------------------------------- |
| **`returnCode`** | **`状态码`**           |   **`String`**   | **`0001` 表示请求成功**          |
| **`returnMsg`**  | **`状态对应的信息`**   |   **`String`**   | **有成功失败等信息**             |
| **`returnData`** | **`返回数据json对象`** | **`jsonObject`** |**是一个json对象，包含用户信息** |
|     `uname`      | `用户名`               |     `String`     | 用户信息                           |
|      `age`       | `用户年龄`             |     `String`     | 用户信息 |
|      `sex`       | `用户性别`             |     `String`     | 用户信息 |
|    `address`     | `中国陕西`             |     `String`     | 用户信息 |
|     `phone`      | `手机号码`             |     `String`     | 用户信息 |

