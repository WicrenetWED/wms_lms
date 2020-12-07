# 10 产品主数据

## 10.1 接口说明

| 项目     | 描述       |
| -------- | ---------- |
| 功能     | 产品主数据 |
| 数据方向 | LMS-->WMS  |
| 实时性   | 实时       |
| 说明     |            |

## 10.2 请求URL与方式

说明：

由于产品LMS产品主数据需要实时同步给到多个WMS系统，为了方便统处理，

故约定在WMS写该接口时，统一方法名使用 RecieveProductMasterData

方法声明示例： 

```C#
public string ReceiveProductData(string inputData)
```

**协议：soap协议**

| WMS厂商 | 服务端地址 | 方法名称                 | 参数名称 |
| ------- | ---------- | ------------------------ | -------- |
| WMS-01  | 待WMS给出  | ReceiveProductMasterData | 不约定   |
| WMS-02  | 待WMS给出  | ReceiveProductMasterData | 不约定   |
| ......  | ......     | ......                   | ......   |
| WMS-N   | 待WMS给出  | ReceiveProductMasterData | 不约定   |



## 10.3 输入参数  

| 参数名称               | 描述         | 类型           | 备注                           |
| ---------------------- | ------------ | -------------- | ------------------------------ |
| ItemId                 | 物料编号     | String         |                                |
| ProductName            | 产品名称     | String         |                                |
| Volume                 | 产品升数     | decimal(18, 4) |                                |
| Packing                | 包装规格     | Float          |                                |
| PackagingGroup         | 包装组       | String         |                                |
| UOM                    | 产品单位     | String         |                                |
| PackagingQty           | 包装数量     | String         |                                |
| ShelfLife              | 保质期(天）  | Int            |                                |
| InventBatchId          | 批次号       | String         | 如有多个批次，为逗号分隔字符串 |
| InventOEM              | OEM          | String         |                                |
| InventGHS              | GHS          | Boolean        | Yes or No                      |
| InventThermal          | 保温品       | Boolean        | Yes or No                      |
| InventFirstReviewDays  | 首次复检(天) | Int            |                                |
| InventSecondReviewDays | 二次复检(天) | Int            |                                |
| InventThirdReviewDays  | 三次复检(天) | Int            |                                |
| InventDangerous        | 危险品       | Enum           | N, DG9,DG3                     |
| ExtendField            | 扩展字段     | String         | 支持后期扩展                   |

## 10.4 输出参数

| 参数名称   | 描述   | 类型   | 备注 |      |
| ---------- | ------ | ------ | ---- | ---- |
| StatusCode | 状态码 | string |      |      |
| Message    | 消息   | string |      |      |
| Data       | 数据   | object |      |      |

Status Code说明：

WMS接收到数据后，提供的反馈状态码：

| StatusCode | 描述                              |
| :--------- | --------------------------------- |
| Y          | 接收成功                          |
| N          | 接收失败，失败时需提供Message信息 |

## 10.5 输入示例

```xml
<?xml version="1.0" encoding="UTF-8" ?>
	<RECORDS>
		<RECORD>
			<ItemId>3366866</ItemId>
			<MC_Liter>0</MC_Liter>
			<MC_Packing></MC_Packing>
			<PackagingGroupId></PackagingGroupId>
			<TaxPackagingQty>0</TaxPackagingQty>
			<PdsShelfLife>666</PdsShelfLife>
			<MC_InventOEM>No</MC_InventOEM>
			<MC_InventGHS>N</MC_InventGHS>
			<MC_InventThermal>No</MC_InventThermal>
			<MC_InventDangerous>N</MC_InventDangerous>
			<ProductName>3366866</ProductName>
			<OUM>Carton</OUM>
			<InventBatchId></InventBatchId>
            <InventFirstReviewDays></InventFirstReviewDays>
            <InventSecondReviewDays></InventSecondReviewDays>
            <InventThirdReviewDays></InventThirdReviewDays>
		</RECORD>
		<RECORD>
			<ItemId>3368738-TM</ItemId>
			<MC_Liter>4</MC_Liter>
			<MC_Packing>1/6</MC_Packing>
			<PackagingGroupId>CARTON</PackagingGroupId>
			<TaxPackagingQty>1</TaxPackagingQty>
			<PdsShelfLife>1460</PdsShelfLife>
			<MC_InventOEM>No</MC_InventOEM>
			<MC_InventGHS>Y</MC_InventGHS>
			<MC_InventThermal>No</MC_InventThermal>
			<MC_InventDangerous>N</MC_InventDangerous>
			<ProductName>PROT SOLVENT FLUSHING OIL 6X4L</ProductName>
			<OUM>EA</OUM>
			<InventBatchId></InventBatchId>
            <InventFirstReviewDays></InventFirstReviewDays>
            <InventSecondReviewDays></InventSecondReviewDays>
            <InventThirdReviewDays></InventThirdReviewDays>
		</RECORD> 
	</RECORDS> 
```



## 10.6 返回示例

```xml
<?xml version="1.0" encoding="UTF-8" ?>
	<statusCode>Y</statusCode>
	<message>接收成功</message>
	<data></data>
```

