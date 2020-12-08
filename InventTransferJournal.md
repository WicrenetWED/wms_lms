# 12 库存转移

## 12.1 接口说明

| 项目     | 描述       |
| -------- | ---------- |
| 功能     | 库存转移   |
| 数据方向 | WMS -> LMS |
| 实时性   | 实时       |
| 说明     |            |

## 12.2 请求URL与方法

**Url**: /api/InventTransferJournal

**方法**：Post

## 12.3 输入参数  

Header 需传参Token

| 参数  | 描述 | 类型   | 是否必填 | 备注 |
| ----- | ---- | ------ | -------- | ---- |
| Token | 令牌 | String | 是       |      |

Body参数说明

| 字段名称               | 中文名   | 类型   | 是否必填 | 备注                                          |
| ---------------------- | -------- | ------ | -------- | --------------------------------------------- |
| ItemId                 | 物料编号 | String | 是       |                                               |
| TransDate              | 日期     | String | 是       |                                               |
| SourceInventLocationId | 源仓库   | String | 是       |                                               |
| TargetInventLocationId | 目标仓库 | String | 是       | 其他出入库时非必填                            |
| Barcode                | 条码信息 | String | 是       | 其他出入库时非必填                            |
| Qty                    | 数量     | Int    | 是       | 正数：入库  负数：出库                        |
| InventBatchId          | 批次号   | String | 是       | 转移日记账时非必填，出入库时必填              |
| JournalNameId          | 日记账ID | String | 是       | Transfer ： 转移日记账       Move：其他出入库 |
| ExtendField            | 扩展字段 | String | 否       | 支持后期扩展                                  |

## 12.4 输出参数

| 参数名称   | 描述   | 类型   | 备注 |      |
| ---------- | ------ | ------ | ---- | ---- |
| StatusCode | 状态码 | int    |      |      |
| Message    | 消息   | string |      |      |
| Data       | 数据   | object |      |      |

Status Code说明：

LMS接收到数据后，提供的反馈状态码：

Status Code

LMS接收到数据后，更新处理结果，提供的反馈状态码：

| Status Code | 描述                   |
| :---------- | ---------------------- |
| 100         | OK                     |
| 101         | 验证失败               |
| 102         | 接收成功               |
| 103         | 处理成功               |
| 104         | 空内容                 |
| 105         | 不正确的请求           |
| 负数        | -1， -xxx 出现异常情况 |

## 12.5 输入示例

```json
[{
    "JournalNameId":"Transfer",
	"ItemId": "3383840",
	"TransDate": "2020-12-04",
	"SourceInventLocationId": "CNCS2420",
	"TargetInventLocationId": "CNCS2421",
    "Qty":1
	"Barcode": "SZ03383840181226229041266130912",
    "InventBatchId" null,
	"ExtendField": null 
}, {
    "JournalNameId":"Move",
	"ItemId": "3385025",
	"TransDate": "2020-12-05",
	"SourceInventLocationId": "CNCS2420",
	"TargetInventLocationId": null,
     "Qty":20
	"Barcode": null,
    "InventBatchId" 18122622
	"ExtendField": null
}]
```



## 12.6 返回示例

```xml
{
	"statusCode": 103,
	"message": "接收成功",
	"data": ""
}
```

