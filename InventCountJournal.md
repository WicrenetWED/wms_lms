# 7反馈盘点结果

## 7.1 接口说明

| 项目     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| 功能     | LMS接收盘点数据                                              |
| 数据方向 | WMS-->LMS                                                    |
| 实时性   | 实时                                                         |
| 说明     | LMS接收盘点数据，做数据校验 如果系统已经存在数据，则会更新盘点数据 （更新主键：locationid+itemid+transdate） |

## 7.2 请求URL与方式

Url:/api/InventCountJournal

方法：Post

协议：https

## 7.3 输入参数

Header 需传参Token

| 字段名称 | 中文名 | 类型   | 是否必填 | 备注 |
| -------- | ------ | ------ | -------- | ---- |
| Token    |        | String | 是       |      |

请求Body参数

| 字段名称         | 中文名         | 类型   | 是否必填 | 备注                        |
| ---------------- | -------------- | ------ | -------- | --------------------------- |
| ItemId           | 物料编号       | String | 是       | 是                          |
| InventSiteId     | 站点           | String | 是       |                             |
| InventOnHand     | 已盘点的现有量 | Float  | 是       |                             |
| InventBatchId    | 批处理号       | String | 是       |                             |
| InventLocationId | 仓库           | String | 是       |                             |
| PalletId         | 托盘编号       | String | 否       |                             |
| Status           | 状态           | String | 是       | 0：初始 1：已盘点 2：已废弃 |
| TransDate        | 业务日期       | String | 是       | 业务日期 格式：2020-11-11   |
| ExtendField      | 扩展字段       | String | 否       | 支持后期扩展                |

## 7.4 输出参数

| 参数名称   | 中文名称 | 类型   | 备注 |      |
| ---------- | -------- | ------ | ---- | ---- |
| StatusCode | 状态码   | int    |      |      |
| Message    | 消息     | string |      |      |
| Data       | 数据     | object |      |      |

Status Code

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

## 7.5 请求示例

```
[{
	"ItemId": "3370441",
	"InventSiteId": "CQ C1 RDC",
	"InventOnHand": 45.00,
	"InventBatchId": "0S191811",
	"InventLocationId": "CNBS2420",
	"PalletId": "pallet_001",
	"Status": 1,
	"TransDate": "2020-11-11",
	"ExtendField": null
}, {
	"ItemId": "3349598",
	"InventSiteId": "CQ C1 RDC",
	"InventOnHand": 45.00,
	"InventBatchId": "0S191811",
	"InventLocationId": "CNBS2420",
	"PalletId": "pallet_001",
	"Status": 1,
	"TransDate": "2020-11-11",
	"ExtendField": null
}]
```



## 7.6 返回示例

```
{
	"statusCode": 103,
	"message": "接收2条，更新2条，创建0条",
	"data": ""
}
```

