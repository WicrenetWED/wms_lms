# 5 发货通知

## 5.1 接口说明

| 项目         | 描述                                     |
| :----------- | :--------------------------------------- |
| **功能**     | LMS下发发货信息至WMS                     |
| **业务场景** | 发货通知下发，包括订单，SKU，数量， 条码 |
| **数据方向** | LMS -> WMS                               |
| **实时性**   | 实时                                     |
| **备注**     | 下发订单类型包括销售单，转移单           |

## 5.2 请求URL与方式

**请求URL**： 需WMS提供支持Soap协议的Web service的WSDL地址

**方式**：POST 

**服务提供public 方法名及参数类型**：**issueOutboundOrdersData**(XML **xmlData**)

**返回值类型：**

**WCF/Web Service开发技术文档参考如下：**

> [Simple Demo WCF Web Services](https://www.codeproject.com/articles/884992/simple-demo-wcf-web-services)
>
> [Web Services in C#](https://www.c-sharpcorner.com/UploadFile/00a8b7/web-service/)

## 5.3 输入参数

| 参数                      | 描述         | 类型     | 示例值                 | 备注                   |
| ------------------------- | ------------ | -------- | ---------------------- | ---------------------- |
| CarrierCode               | 装运承运人   | String   | BLAlog                 |                        |
| FromSiteId                | 始发站点     | String   | BL C1 RDC              |                        |
| ItemId                    | 物料编号     | String   | 3411987                | SKU                    |
| LiterQty                  | 升数         | Float    | 24                     |                        |
| LoadDirection             | 方向         | Enum     | 出站                   | 出仓  或 入仓          |
| LoadId                    | 运单编号     | String   | ALOG-BL20191024SD0001  |                        |
| LoadStatus                | 运单状态     | Enum     | 打开                   |                        |
| ConfirmedDateTime         | 预约确认时间 | DateTime | 10/24/2021 03:00:00 am |                        |
| LocationEnd               | Shipto       | String   | 12161101               | 目的信息：客户或者仓库 |
| LocationStart             | 始发地       | String   | CNCS2436               | 始发仓库信息           |
| OrderId                   | 订单编号     | String   | 54316242               |                        |
| Qty                       | 发货数量     | Int      | 2                      |                        |
| TMSModeCode               | 运输方式     | String   | CA1                    |                        |
| InventGHS                 | GHS          | Boolean  | N                      | Y  or N                |
| InventThermal             | 保温品       | Boolean  | N                      | Y or N                 |
| InventDangerous           | 危险品       | Enum     | DG9                    | N,  DG9,DG3            |
| ShippingInstruction       | 装运指示     | String   | 急单                   |                        |
| ShippingInstructionRemark | 装运指示备注 | String   | 客户要求非工作日送货   |                        |
| ToSiteId                  | 目标站点     | String   | BL C1 RDC              |                        |
| UOM                       | 单位         | String   | CARTON                 |                        |

## 5.4 输出参数

| 参数       | 值                                             | 示例                   |
| ---------- | ---------------------------------------------- | ---------------------- |
| StatusCode | Y： 成功  N：失败                              | Y                      |
| Message    | 失败后的提示信息，具体类型需要根据实际情况沟通 | 例如：已经启运无法变更 |

## 5.5 下发出库订单示例

数据源：

| 运单号                | JDE订单编号 | 物料编号 | 数量  | 升数     | 始发地   | Ship To  | 方式 | 始发站点  | 单位   |
| --------------------- | ----------- | -------- | ----- | -------- | -------- | -------- | ---- | --------- | ------ |
| ALOG-BL20191024SD0001 | 54316534    | 3423504  | 10.00 | 120.00   | CNCS2436 | 20033184 | CA1  | BL C1 RDC | CARTON |
| ALOG-BL20191024SD0001 | 54316361    | 3408321  | 7.00  | 1,347.98 | CNCS2436 | 20057163 | CB1  | BL C1 RDC | DRUM   |
| ALOG-BL20191024SD0001 | 54316361    | 3379971  | 4.00  | 800.00   | CNCS2436 | 20057163 | CB1  | BL C1 RDC | DRUM   |
| ALOG-BL20191024SD0001 | 54316361    | 3409276  | 4.00  | 806.22   | CNCS2436 | 20057163 | CB1  | BL C1 RDC | DRUM   |
| ALOG-BL20191024SD0001 | 54316242    | 3411987  | 2.00  | 24.00    | CNCS2436 | 12161101 | CA1  | BL C1 RDC | CARTON |
| ALOG-BL20191024SD0001 | 54316242    | 3380353  | 1.00  | 24.00    | CNCS2436 | 12161101 | CA1  | BL C1 RDC | CARTON |
| ALOG-BL20191024SD0001 | 54316242    | 3411988  | 1.00  | 24.00    | CNCS2436 | 12161101 | CA1  | BL C1 RDC | CARTON |
| ALOG-BL20191024SD0001 | 54316242    | 3383840  | 1.00  | 24.00    | CNCS2436 | 12161101 | CA1  | BL C1 RDC | CARTON |
| ALOG-BL20191024SD0001 | 54316559    | 3388858  | 5.00  | 60.00    | CNCS2436 | 20049823 | CA1  | BL C1 RDC | CARTON |

下发的XML文件按**运单**（LoadId）+ **订单**（OrderId) + **SKU** (ItemId) 组合

```
<?xml version="1.0" encoding="utf-8"?>
<Records>
    <Record>
        <LoadId>ALOG-BL20191024SD0001</LoadId>
        <OrderId>54316242</OrderId>
        <LoadStatus>打开</LoadStatus>
        <LoadDirection>出站</LoadDirection>
        <LocationConfirmDateTime>10/24/2019 03:00:00 am</LocationConfirmDateTime>
        <FromSiteId>BL C1 RDC</FromSiteId>
        <LocationStart>CNCS2436</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>12161101</LocationEnd>
        <CarrierCode>BLAlog</CarrierCode>
        <TMSModeCode>CA1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3411987</ItemId>
                <Qty>2</Qty>
                <LiterQty>24</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>__</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
            <Item>
                <ItemId>3380353</ItemId>
                <Qty>1</Qty>
                <LiterQty>24</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>__</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
            <Item>
                <ItemId>3411988</ItemId>
                <Qty>1</Qty>
                <LiterQty>24</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>__</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
            <Item>
                <ItemId>3383840</ItemId>
                <Qty>1</Qty>
                <LiterQty>24</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>__</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>Y</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>ALOG-BL20191024SD0001</LoadId>
        <OrderId>54316361</OrderId>
        <LoadStatus>打开</LoadStatus>
        <LoadDirection>出站</LoadDirection>
        <LocationConfirmDateTime>10/24/2019 03:00:00 am</LocationConfirmDateTime>
        <FromSiteId>BL C1 RDC</FromSiteId>
        <LocationStart>CNCS2436</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20057163</LocationEnd>
        <CarrierCode>BLAlog</CarrierCode>
        <TMSModeCode>CB1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3408321</ItemId>
                <Qty>7</Qty>
                <LiterQty>1347</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>Y</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
            <Item>
                <ItemId>3379971</ItemId>
                <Qty>4</Qty>
                <LiterQty>800</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
            <Item>
                <ItemId>3409276</ItemId>
                <Qty>4</Qty>
                <LiterQty>806</LiterQty>
                <UOM>DRUM</UOM>
                <ShippingInstruction></ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>ALOG-BL20191024SD0001</LoadId>
        <OrderId>54316534</OrderId>
        <LoadStatus>打开</LoadStatus>
        <LoadDirection>出站</LoadDirection>
        <LocationConfirmDateTime>10/24/2019 03:00:00 am</LocationConfirmDateTime>
        <FromSiteId>BL C1 RDC</FromSiteId>
        <LocationStart>CNCS2436</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20033184</LocationEnd>
        <CarrierCode>BLAlog</CarrierCode>
        <TMSModeCode>CA1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3423504</ItemId>
                <Qty>10</Qty>
                <LiterQty>120</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>BO4513114481</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
        </Items>
    </Record>
    <Record>
        <LoadId>ALOG-BL20191024SD0001</LoadId>
        <OrderId>54316559</OrderId>
        <LoadStatus>打开</LoadStatus>
        <LoadDirection>出站</LoadDirection>
        <LocationConfirmDateTime>10/24/2019 03:00:00 am</LocationConfirmDateTime>
        <FromSiteId>BL C1 RDC</FromSiteId>
        <LocationStart>CNCS2436</LocationStart>
        <ToSiteId>BL C1 RDC</ToSiteId>
        <LocationEnd>20049823</LocationEnd>
        <CarrierCode>BLAlog</CarrierCode>
        <TMSModeCode>CA1</TMSModeCode>
        <Items>
            <Item>
                <ItemId>3388858</ItemId>
                <Qty>5</Qty>
                <LiterQty>60</LiterQty>
                <UOM>CARTON</UOM>
                <ShippingInstruction>9915416</ShippingInstruction>
                <ShippingInstructionRemark></ShippingInstructionRemark>
                <InventGHS>N</InventGHS>
                <InventThermal>N</InventThermal>
                <InventDangerous>N</InventDangerous>
            </Item>
        </Items>
    </Record>
</Records>
```



## 5.6 返回示例

| 场景         | 取值       |
| ------------ | ---------- |
| 运单下发成功 | Y          |
| 运单下发失败 | N， 和原因 |

