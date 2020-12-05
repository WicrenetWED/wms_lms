# 6 确认出仓

## 6.1 接口说明

| 项目         | 描述                                |
| :----------- | :---------------------------------- |
| **功能**     | LMS接收已出仓订单信息               |
| **业务场景** | 确认出仓条码，包括订单，SKU，条形码 |
| **数据方向** | WMS ->LMS                           |
| **实时性**   | 实时                                |
| **备注**     |                                     |

## 6.2 请求URL与方式

**URL：**/api/OutWarehouseInformation

**方式：POST**

**协议：HTTP**

## 6.3 输入参数

**Header参数：**需要设置Token 参数以获得接口访问权限

| 参数  | 描述           | 类型   | 是否必填 | 备注 |
| ----- | -------------- | ------ | -------- | ---- |
| Token | 令牌，用于认证 | String | 是       |      |

**Body的参数：**

| 参数             | 描述         | 类型     | 是否必填 | 示例值 | 备注 |
| ---------------- | ------------ | -------- | -------- | ------ | ---- |
| LoadId           | 运单编号     | String   | 是       |        |      |
| OrderId          | 订单编号     | String   | 是       |        |      |
| ItemId           | 物料编号     | String   | 是       |        | SKU  |
| Qty              | 订运单数量   | Float    | 是       |        |      |
| CheckOutTime     | 出仓时间     | DateTime | 是       |        |      |
| InventBatchId    | 批次号       | String   | 是       |        |      |
| InventLocationId | 仓库         | String   | 是       |        |      |
| PalletId         | 托盘编号     | String   | 是       |        |      |
| WMSLocation      | 出库地标编号 | String   | 是       |        |      |
| Barcode          | 条码信息     | String   | 是       |        |      |

## 6.4 输出参数

Status Code**

LMS接收到数据后，更加处理结果，提供的反馈状态码：

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

## 6.5 请求示例

支持JSON格式，请严格按照 **运单**（LoadId）+ **订单**（OrderId) + **SKU** (ItemId) 组合回传对应的条码信息

```
[
  {
    "loadId": "HT-BL20200718SD0005",
    "orderId": "51848725",
    "itemId": "3331247",
    "qty":3,
    "checkOutTime": "2020-12-05T14:45:44",
    "outWHSBarcodes": [
      {
        "inventLocationId": "CNBS2436",
        "palletId": "BS13KS08205",
        "wmsLocation": "GZ1-EX02",
        "barcode": "TC03331247200226230299626732286",
        "inventBatchId": "23029962"
      },
      {
        "inventLocationId": "CNBS2436",
        "palletId": "BS13KS08205",
        "wmsLocation": "GZ1-EX02",
        "barcode": "TC03331247200226230299626732287",
        "inventBatchId": "23029962"
      },
      {
        "inventLocationId": "CNBS2436",
        "palletId": "BS13KS08205",
        "wmsLocation": "GZ1-EX02",
        "barcode": "TC03331247200226230299626732288",
        "inventBatchId": "23029962"
      }
    ]
  },
  {
    "loadId": "HT-BL20200718SD0005",
    "orderId": "51848725",
    "itemId": "3331248",
    "qty":2,
    "checkOutTime": "2020-12-05T14:45:44",
    "outWHSBarcodes": [
      {
        "inventLocationId": "CNBS2436",
        "palletId": "BS13KS08206",
        "wmsLocation": "GZ1-EX02",
        "barcode": "TC03331248200226230299626732286",
        "inventBatchId": "23029963"
      },
      {
        "inventLocationId": "CNBS2436",
        "palletId": "BS13KS08206",
        "wmsLocation": "GZ1-EX02",
        "barcode": "TC03331248200226230299626732287",
        "inventBatchId": "23029963"
      }
    ]
  }
]
```



## 6.6 返回示例

```
{

  "statusCode": 102,

  "message": "成功的记录总数 2",

  "data": "2"

}
```

