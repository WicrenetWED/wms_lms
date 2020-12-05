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

**URL：**api/ReceiptCompletedWaybills

**方式：POST**

## 4.3 输入参数

**Header：**需要设置Token 参数

下列是Body的参数：

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
| PalletId        | 托盘编号 | String   | AQ13KS00003                      | 否       |                          |

## 4.4 输出参数

**Status Code**

LMS接收到数据后，更新处理结果，提供的反馈状态码：

| Status Code | 描述                   |
| ----------: | ---------------------- |
|         100 | OK                     |
|         101 | 验证失败               |
|         102 | 接收成功               |
|         103 | 处理成功               |
|         104 | 空内容                 |
|         105 | 不正确的请求           |
|        负数 | -1， -xxx 出现异常情况 |

**Message**：

LMS的响应信息

**Data**：

接收成功对应的是接收数据的记录数，如异常提示错误信息。

`示例：`

`StatusCode: 102,`

`Message: "成功的记录总数 3",`
`Data: 3`

## 4.5 请求示例

支持JSON格式，请严格按照 **运单**（LoadId）+ **订单**（OrderId) + **SKU** (ItemId) 组合回传对应的条码信息

```
[
  {
    "loadId": "ZH-BL20190130SD0008Test3",
    "orderId": "542639083",
    "itemId": "341789801",
    "receivedQty": 4,
    "arrivedDatetime": "2020-12-05T08:23:52",
    "signInDatetime": "2020-12-05T08:23:52",
    "receiptBarcodes": [
      {
        "barcode": "SH034178972903120S19149466281581",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281582",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281583",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281584",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      }
    ]
  },
  {
    "loadId": "ZH-BL20190130SD0008Test3",
    "orderId": "542639083",
    "itemId": "341789802",
    "receivedQty": 3,
    "arrivedDatetime": "2020-12-05T08:23:52",
    "signInDatetime": "2020-12-05T08:23:52",
    "receiptBarcodes": [
      {
        "barcode": "SH034178972903120S19149466281585",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00003"
      },
      {
        "barcode": "SH034178972903120S19149466281586",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00003"
      },
      {
        "barcode": "SH034178972903120S19149466281587",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00003"
      }
    ]
  },
  {
    "loadId": "ZH-BL20190130SD0008Test3",
    "orderId": "54263908302",
    "itemId": "3417898",
    "receivedQty": 3,
    "arrivedDatetime": "2020-12-05T08:23:52",
    "signInDatetime": "2020-12-05T08:23:52",
    "receiptBarcodes": [
      {
        "barcode": "SH034178972903120S19149466281588",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281589",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281590",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      }
    ]
  },
  {
    "loadId": "ZH-BL20190130SD0008Test4",
    "orderId": "54263908303",
    "itemId": "3417898",
    "receivedQty": 4,
    "arrivedDatetime": "2020-12-05T08:23:52",
    "signInDatetime": "2020-12-05T08:23:52",
    "receiptBarcodes": [
      {
        "barcode": "SH034178972903120S19149466281591",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281592",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281593",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      },
      {
        "barcode": "SH034178972903120S19149466281594",
        "status": 0,
        "reason": "",
        "palletId": "AQ13KS00002"
      }
    ]
  }
]
```



##  4.6 返回示例

```
{
    "statusCode": 102,
    "message": "成功的记录总数 4",
    "data": "4"
}
```







