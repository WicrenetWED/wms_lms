# 10.1 CRM主数据

## 1 接口说明

| 项目     | 描述      |
| -------- | --------- |
| 功能     | CRM主数据 |
| 数据方向 | LMS-->WMS |
| 实时性   | 实时      |
| 说明     |           |

## 2 请求URL与方式

说明：

由于产品LMS产品主数据需要实时同步给到多个WMS系统，为了方便统处理，

故约定在WMS写该接口时，统一方法名使用RecieveCRMMasterData，参数

方法声明示例： 

```C#
public string RecieveCRMMasterData(string inputData)
```

协议：soap协议

| WMS厂商 | 服务端地址 | 方法名称             | 参数名称 |
| ------- | ---------- | -------------------- | -------- |
| WMS-01  | 待WMS给出  | RecieveCRMMasterData | 不约定   |
| WMS-02  | 待WMS给出  | RecieveCRMMasterData | 不约定   |
| ......  | ......     | ......               | ......   |
| WMS-N   | 待WMS给出  | RecieveCRMMasterData | 不约定   |



## 3 输入参数说明

| 参数名称       | 中文名     | 数据类型 | 备注           |
| -------------- | ---------- | -------- | -------------- |
| ShipToCode     | 客户编号   | String   |                |
| ShipToCodeName | 客户名称   | String   |                |
| DemandItem     | 物流需求项 | 数组     | 需求项结构如下 |
| ExtendField    | 扩展字段   | String   |                |

需求项结构字段说明：

| 参数名称     | 中文名     | 数据类型 | 备注 |
| ------------ | ---------- | -------- | ---- |
| ItemName     | 需求项名称 | String   |      |
| Value        | 需求值     | String   |      |
| DemandRemark | 需求备注   | String   |      |

## 4 输出参数

| 参数名称   | 中文名称 | 类型   | 备注 |      |
| ---------- | -------- | ------ | ---- | ---- |
| StatusCode | 状态码   | int    |      |      |
| Message    | 消息     | string |      |      |
| Data       | 数据     | object |      |      |

Status Code说明：

WMS接收到数据后，提供的反馈状态码：

| StatusCode | 描述                              |
| :--------- | --------------------------------- |
| 0          | 接收成功                          |
| -1         | 接收失败，失败时需提供Message信息 |

## 5 输入示例

```xml
<?xml version="1.0" encoding="UTF-8" ?>
	<RECORDS>
		<RECORD>
			<ShipToCode>12102201</ShipToCode>
			<ShipToCodeName>深圳市中汽南方汽车配套服务有限公司</ShipToCodeName>
			<DemandItem>
				<ItemName>MOQ</ItemName>
				<Value></Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>产品收货标准</ItemName>
				<Value>与BP/嘉实多标准一致</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>允许通行的货车最大荷载质量（吨）</ItemName>
				<Value>有限制，请备注</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>允许通行的货车最长整车长度（米）</ItemName>
				<Value>有限制，请备注</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>其他物流需求</ItemName>
				<Value>无</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>卸货场地是否配备雨天卸货设施</ItemName>
				<Value>有雨棚</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>固定收货人</ItemName>
				<Value>曲 晨</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>固定收货人联系电话</ItemName>
				<Value></Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>客户卸货方式</ItemName>
				<Value>人工卸货</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>托板是否循环使用</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>收货地址是否处于货车交通管制区</ItemName>
				<Value>Y</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>收货委托书是否有收货专用章</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否有收货委托书</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要 SDS（化学品安全技术说明书）</ItemName>
				<Value>需要运输随行附带</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要BP卸货</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要QAC（产品质保书）文件</ItemName>
				<Value>需要运输随行附带</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要到货通知</ItemName>
				<Value>Y</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要带托板运输</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>是否需要提供电子回单服务</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>签收单类型</ItemName>
				<Value>BP标准四联单</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>签收方式</ItemName>
				<Value>客户直接签收</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>订单收货时间要求</ItemName>
				<Value>无要求</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>货物码放层数是否有特殊需求</ItemName>
				<Value>N</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>货车允许通行时间</ItemName>
				<Value>有限制，请备注</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>货车长宽高限制（米）</ItemName>
				<Value>有限制，请备注</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>车型要求</ItemName>
				<Value>无要求</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
			<DemandItem>
				<ItemName>送货前是否需要联系</ItemName>
				<Value>Y</Value>
				<DemandRemark></DemandRemark>
			</DemandItem>
		</RECORD>
	</RECORDS> 
```



## 6 返回示例

```xml
<?xml version="1.0" encoding="UTF-8" ?>
	<statusCode>0</statusCode>
	<message>接收成功</message>
	<data></data>
```

