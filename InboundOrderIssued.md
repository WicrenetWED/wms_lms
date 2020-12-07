# 3 入库通知下发

## 3.1 接口说明

| 项目         | 描述                                     |
| :----------- | :--------------------------------------- |
| **功能**     | LMS下发入库信息至WMS                     |
| **业务场景** | 入库通知下发，包括订单，SKU，数量， 条码 |
| **数据方向** | LMS -> WMS                               |
| **实时性**   | 实时                                     |
| **备注**     | 下发订单类型包括采购单，转移单，销售退单 |

## 3.2 请求URL与方式

**请求URL**： 需WMS提供支持Soap协议的Web service的WSDL地址

**方式**：POST 

**协议:  SOAP**

**服务提供public 方法名及参数类型**：

```
public string issueInboundOrders(XML xmlData)
```

**WCF/Web Service开发技术文档参考如下：**

> [Simple Demo WCF Web Services](https://www.codeproject.com/articles/884992/simple-demo-wcf-web-services)
>
> [Web Services in C#](https://www.c-sharpcorner.com/UploadFile/00a8b7/web-service/)

## 3.3 输入参数

| 参数                      | 描述         | 类型     | 示例值                                                       | 备注                 |
| ------------------------- | ------------ | -------- | ------------------------------------------------------------ | -------------------- |
| Barcode                   | 条码信息     | String   | TC03426929201104230991557108383                              | 退货单时条码非必填项 |
| CarrierCode               | 装运承运人   | String   | TCCosco                                                      |                      |
| FromSiteId                | 始发站点     | String   | TC Plant                                                     |                      |
| ItemId                    | 物料编号     | String   | 3426929                                                      | SKU                  |
| LoadDirection             | 方向         | Enum     | 入站                                                         | 出仓  或 入仓        |
| LoadId                    | 运单编号     | String   | COSCO-TC20201105PD0004                                       |                      |
| LoadStatus                | 运单状态     | Enum     | 已起运                                                       |                      |
| ConfirmedDateTime         | 预约确认时间 | DateTime | 12/5/2020 12:24:48 pm                                        |                      |
| LocationEnd               | Shipto       | String   | 20074237                                                     | 目的信息：仓库       |
| LocationStart             | 始发地       | String   | CNBS2302                                                     | 始发仓库信息         |
| OrderId                   | 订单编号     | String   | 13017210                                                     |                      |
| Qty                       | 出货数量     | Int      | 12                                                           |                      |
| TMSModeCode               | 运输方式     | String   | CW1                                                          |                      |
| ToSiteId                  | 目标站点     | String   | BL C1 RDC                                                    |                      |
| UOM                       | 单位         | String   | DRUM                                                         |                      |
| LiterQty                  | 升数         | Float    | 2496                                                         |                      |
| InventGHS                 | GHS          | Boolean  | N                                                            | Y or N               |
| InventThermal             | 保温品       | Boolean  | N                                                            | Y  or N              |
| InventDangerous           | 危险品       | Enum     | N                                                            | N, DG9,DG3           |
| ShippingInstruction       | 装运指示     | String   | #紧急订单（长期）0304托盘需求（长期）随货配送额外文件（长期）# |                      |
| ShippingInstructionRemark | 装运指示备注 | String   | ‘’                                                           |                      |
| InventBatchId             | 批次号       | String   | 23099155                                                     |                      |
| PalletId                  | 托盘编号     | String   | A23099155A                                                   |                      |



## 3.4 输出参数

| 参数       | 值                                             | 示例                   |
| ---------- | ---------------------------------------------- | ---------------------- |
| StatusCode | Y： 成功  N：失败                              | Y                      |
| Message    | 失败后的提示信息，具体类型需要根据实际情况沟通 | 例如：已经启运无法变更 |

## 3.5 下发入库订单示例

数据源：

| 运单号                 | JDE订单编号 | 物料编号 | 数量  | 升数     | 始发地   | Ship To  | 方式 | 始发站点 | 单位 |
| ---------------------- | ----------- | -------- | ----- | -------- | -------- | -------- | ---- | -------- | ---- |
| COSCO-TC20201105PD0004 | 13017210    | 3426929  | 12.00 | 2,496.00 | CNBS2302 | 20074237 | CW1  | TC Plant | DRUM |
| COSCO-TC20201105PD0004 | 13017229    | 3348908  | 4.00  | 800.00   | CNBS2302 | 20074237 | CW1  | TC Plant | DRUM |
| COSCO-TC20201105PD0004 | 13017372    | 3348958  | 4.00  | 800.00   | CNBS2302 | 20074237 | CW1  | TC Plant | DRUM |
| COSCO-TC20201105PD0004 | 13017234    | 3395070  | 30.00 | 6,000.00 | CNBS2302 | 20074237 | CW1  | TC Plant | DRUM |
| COSCO-TC20201105PD0004 | 13017234    | 3408751  | 30.00 | 6,000.00 | CNBS2302 | 20074237 | CW1  | TC Plant | DRUM |

下发的XML文件按**运单**（LoadId）+ **订单**（OrderId) + **SKU** (ItemId) + **条形码**（Barcode) 组合

> <Records>
>     <Record>
>         <Items>
>             <Item>
>                 <BarcodeList>
>                     <BarcodeItem></BarcodeItem>
>                 </BarcodeList>
>             </Item>
>         </Items>
>     </Record>

```
<?xml version="1.0" encoding="utf-8"?>
<Records>
    <Record>
        <LoadId>COSCO-TC20201105PD0004</LoadId>
        <OrderId>13017210</OrderId>
        <LoadStatus>已起运</LoadStatus>
        <LoadDirection>入站</LoadDirection>
        <LocationConfirmDateTime>12/5/2020 12:24:48 pm</LocationConfirmDateTime>
        <FromSiteId>TC Plant</FromSiteId>
        <LocationStart>CNBS2302</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20074237</LocationEnd>
        <CarrierCode>TCCosco</CarrierCode>
        <TMSModeCode>CW1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3426929</ItemId>
                <Qty>12</Qty>
                <LiterQty>2496</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
                <BarcodeList>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108383</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108384</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108385</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108386</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108387</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108388</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108389</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108390</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108395</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108396</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108397</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03426929201104230991557108398</Barcode>
                        <InventBatchId>23099155</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                </BarcodeList>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>COSCO-TC20201105PD0004</LoadId>
        <OrderId>13017229</OrderId>
        <LoadStatus>已起运</LoadStatus>
        <LoadDirection>入站</LoadDirection>
        <LocationConfirmDateTime>12/5/2020 12:24:48 pm</LocationConfirmDateTime>
        <FromSiteId>TC Plant</FromSiteId>
        <LocationStart>CNBS2302</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20074237</LocationEnd>
        <CarrierCode>TCCosco</CarrierCode>
        <TMSModeCode>CW1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3348908</ItemId>
                <Qty>4</Qty>
                <LiterQty>800</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>Y</InventGHS>
                <InventThermal>Y</InventThermal>
                <InventDangerous>N</InventDangerous>
                <BarcodeList>
                    <BarcodeItem>
                        <Barcode>TC03348908201103230991437103589</Barcode>
                        <InventBatchId>23099143</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348908201103230991437103590</Barcode>
                        <InventBatchId>23099143</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348908201103230991437103591</Barcode>
                        <InventBatchId>23099143</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348908201103230991437103592</Barcode>
                        <InventBatchId>23099143</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                </BarcodeList>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>COSCO-TC20201105PD0004</LoadId>
        <OrderId>13017234</OrderId>
        <LoadStatus>已起运</LoadStatus>
        <LoadDirection>入站</LoadDirection>
        <LocationConfirmDateTime>12/5/2020 12:24:48 pm</LocationConfirmDateTime>
        <FromSiteId>TC Plant</FromSiteId>
        <LocationStart>CNBS2302</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20074237</LocationEnd>
        <CarrierCode>TCCosco</CarrierCode>
        <TMSModeCode>CW1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3395070</ItemId>
                <Qty>30</Qty>
                <LiterQty>6000</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
                <BarcodeList>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107893</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107894</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107895</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107896</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107897</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107898</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107899</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107900</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107901</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107902</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107903</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230991767107904</Barcode>
                        <InventBatchId>23099176</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107905</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107906</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107907</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107908</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107913</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107914</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107915</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107916</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107917</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107918</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107919</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107920</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107993</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107994</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107995</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077107996</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077108013</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03395070201104230992077108014</Barcode>
                        <InventBatchId>23099207</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                </BarcodeList>
            </Item>
            <Item>
                <ItemId>3408751</ItemId>
                <Qty>30</Qty>
                <LiterQty>6000</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
                <BarcodeList>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103276</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103277</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103278</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103279</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103292</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103293</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103294</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103295</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103296</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103297</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103298</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103299</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103300</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103301</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103302</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103303</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103304</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103305</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103306</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103307</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103308</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103309</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103310</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103311</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103312</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103313</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103314</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103315</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103321</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03408751201103230991737103322</Barcode>
                        <InventBatchId>23099173</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                </BarcodeList>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>COSCO-TC20201105PD0004</LoadId>
        <OrderId>13017372</OrderId>
        <LoadStatus>已起运</LoadStatus>
        <LoadDirection>入站</LoadDirection>
        <LocationConfirmDateTime>12/5/2020 12:24:48 pm</LocationConfirmDateTime>
        <FromSiteId>TC Plant</FromSiteId>
        <LocationStart>CNBS2302</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20074237</LocationEnd>
        <CarrierCode>TCCosco</CarrierCode>
        <TMSModeCode>CW1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3348958</ItemId>
                <Qty>4</Qty>
                <LiterQty>800</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>Y</InventGHS>
                <InventThermal>Y</InventThermal>
                <InventDangerous>N</InventDangerous>
                <BarcodeList>
                    <BarcodeItem>
                        <Barcode>TC03348958201102230972757101484</Barcode>
                        <InventBatchId>23097275</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348958201102230972757101485</Barcode>
                        <InventBatchId>23097275</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348958201102230972757101486</Barcode>
                        <InventBatchId>23097275</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                    <BarcodeItem>
                        <Barcode>TC03348958201102230972757101487</Barcode>
                        <InventBatchId>23097275</InventBatchId>
                        <PalletId></PalletId>
                    </BarcodeItem>
                </BarcodeList>
            </Item>
        </Items>
    </Record>
</Records>
```



## 3.6 返回示例

| 场景         | 取值       |
| ------------ | ---------- |
| 运单下发成功 | Y          |
| 运单下发失败 | N， 和原因 |


{% include disqus.html %}