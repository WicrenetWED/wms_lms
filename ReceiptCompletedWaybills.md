# 4 收货完成

## 4.1 接口说明

| 项目         | 描述                                    |
| :----------- | :-------------------------------------- |
| **功能**     | LMS接收收货订单信息                     |
| **业务场景** | 收货完成上传，包括订单，SKU，数量，条码 |
| **数据方向** | WMS ->LMS                               |
| **实时性**   | 实时                                    |
| **备注**     |                                         |

## 4.2 请求URL及方式

URL：

方式：POST

## 4.4 输入参数

| 参数            | 描述     | 类型     | 示例值                           | 是否必填 | 备注                     |
| --------------- | -------- | -------- | -------------------------------- | -------- | ------------------------ |
| LoadId          | 运单编号 | String   | ZH-BL20190130SD0008              | 是       |                          |
| OrderId         | 订单编号 | String   | 542639082                        | 是       | JDE订单号                |
| ItemId          | 物料编号 | String   | 3417898                          | 是       | SKU                      |
| ReceivedQty     | 实收数量 | Float    | 3                                | 是       | 订单+运单+SKU的总数量    |
| ArrivedDatetime | 到货时间 | datetime | 2020-12-03T09:54:05              | 是       |                          |
| SignInDatetime  | 签收时间 | datetime | 2020-12-03T09:54:05              | 是       |                          |
| Barcode         | 条码信息 | String   | SH034178972903120S19149466281581 | 是       | SKU对应的条形码信息      |
| Status          | 流程状态 | Enum     | 1                                | 是       | 0：正常收货，1：异常收货 |
| Reason          | 原因     | String   | 异常收货                         | 否       | 异常收货需要提供原因     |
| PalletId        | 托盘编号 | String   | ABSSSSS                          | 否       |                          |

## 4.5 输出参数

**StatusCode** LMS接收到数据后，更加处理结果，提供的反馈状态码：

| Status Code | 描述                   |
| ----------: | ---------------------- |
|         100 | OK                     |
|         101 | 验证失败               |
|         102 | 接收成功               |
|         103 | 处理成功               |
|         104 | 空内容                 |
|         105 | 不正确的请求           |
|        负数 | -1， -xxx 出现异常情况 |

**Message**：LMS的响应信息

**Data**： 接收成功对应的是接收数据的记录数，如果失败对应失败的记录的信息。

`示例：`

`StatusCode: 102,`

`Message: "成功的记录总数 3",`
`Data: 3`

## 4.6 请求示例

```
[
  {
    "loadId": "ZH-BL20190130SD0008",
    "orderId": "542639082",
    "itemId": "3417898",
    "receivedQty": 3,
    "arrivedDatetime": "2020-12-03T09:54:05",
    "signInDatetime": "2020-12-03T09:54:05",
    "barcode": "SH034178972903120S19149466281581",
    "status": 0,
    "reason": "string",
    "palletId": "string"
  },
  {
    "loadId": "ZH-BL20190130SD0008",
    "orderId": "542639082",
    "itemId": "3417897",
    "receivedQty": 3,
    "arrivedDatetime": "2020-12-03T09:54:05",
    "signInDatetime": "2020-12-03T09:54:05",
    "barcode": "SH034178921903120S14149066381501",
    "status": 1,
    "reason": "",
    "palletId": "string"

  },
  {
    "loadId": "ZH-BL20190130SD0008",
    "orderId": "542639082",
    "itemId": "3417898",
    "receivedQty": 3,
    "arrivedDatetime": "2020-12-03T09:54:05",
    "signInDatetime": "2020-12-03T09:54:05",
    "barcode": "SH034178971903220S1914906638159",
    "status": 1,
    "reason": "异常收货",
    "palletId": "ABSSSSS"
  }
]
```



##  4.7 返回示例

```
{
    "statusCode": 102,
    "message": "成功的记录总数 3",
    "data": "3"
}
```







