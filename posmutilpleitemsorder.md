# 门店下单篇
## POS门店下发发货通知至OMS
### 功能描述
门店生成o2o订单后生成发货通知单,推送发货通知单至电商
### 请求说明
> **method**: POST 
> 
> **path**: pos/ordermake
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
shopCode | string | 下单门店 
dispatchNO | string | 发货通知单号 
purchaseCode | string | 销售小票号 
memberCode | string | 会员ID 
receiver | string | 收件人 
phone | string | 电话 
province | string | 省 
city | string | 市 
county | string | 区县 
address | string | 地址 
product | string | 货号 
color | string | 色号 
size | string | 尺码 
qty | int | 件数 

### 请求
```
{
    "shopCode":"",
    "dispatchNO":"",
    "purchaseCode":"",
    "memberCode":"",
    "receiver":"",
    "phone":"",
    "province":"",
    "city":"",
    "county":"",
    "address":"",
    "items":[
        {"product":"G1","color":"C1","size":"S1","qty":2},
        {"product":"G2","color":"C2","size":"S2","qty":3},
    ]
}
```
### 返回

HttpStatus | 说明
 - | -
200 | 通知单同步OMS完成
Other | 重试直至超时人工处理

## POS门店作废发货通知单
### 功能描述
门店在小票退款交易取消 或者通知单超卖的情况下,不需要OMS进行发货，作废发货通知单,同步状态给OMS
### 请求说明
> **method**: POST 
> 
> **path**: pos/orderremove
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | POS发货通知单号 
success | boolean | POS发货通知单作废状态是否同步至OMS
msg |string? | 发货通知单已经出库(false)

### 请求
```
{
    "dispatchNO":""
}
```
### 返回
> HttpStatus 200
```
{
    "success":true
}
```
```
{
    "success":false,
    "msg":"通知单已经出库"
}
```
> 其他 HttpStatus 重试直至超时人工处理


## OMS同步包裹发货
### 功能描述
OMS端会将通知单整单或者拆解至满足条件的出库网点进行发货,每个出库网点出库回传OMS后分段回传POS,一个POS发货通知单可能会回传多个transferCode

### 请求说明
> **method**: POST 
> 
> **query**: 待定
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
transferCode | string | SCM调入单号(OMS下发发货通知单)
dispatchNO | string | POS下达的发货通知单号 
shipCode | string | 波司登出库网点编号 
shipName | string | 波司登出库网点名称  
express | string | 出库快递代码
waybillCode | string | 运单号
product | string | 货号
color | string | 色号
size | string | 尺码
sn | string | 件码
success | boolean | 出库状态是否同步至POS
msg |string | 同步失败原因

### 请求
```
{
    "transferCode":"",
    "dispatchNO":"",
    "shipCode":"",
    "shipName":"",
    "express":"",
    "waybillCode":"",
    "packages":[
        {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]},
        {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]}
    ]
}
```
### 返回
> HttpStatus 200
```
{
    "success":true
}
```
```
{
    "success":false,
    "msg":"通知单已作废"
}
```
> 其他 HttpStatus 重试直至超时人工处理

## OMS同步通知单发货
### 功能描述
POS发货通知单在OMS端全部出库或者下单时即超卖,全部出库推送此接口OMS不再会有新的同步数据推送

### 请求说明
> **method**: POST 
> 
> **query**: 待定
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
transferCode | string | SCM调入单号(OMS下发发货通知单)
dispatchNO | string | POS下达的发货通知单号 
shipCode | string | 波司登出库网点编号 
shipName | string | 波司登出库网点名称  
express | string | 出库快递代码
waybillCode | string | 运单号
product | string | 货号
color | string | 色号
size | string | 尺码
sn | string | 件码
success | boolean | 出库状态是否同步至POS
msg |string | 同步失败原因

### 请求
全部出库
```
{
    "dispatchNO":"",
    "transfers":[
        {
            "transferCode":"",
            "shipCode":"",
            "shipName":"",
            "express":"",
            "waybillCode":"",
            "packages":[
                {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]},
                {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]}
                ]
        },
        {
            "transferCode":"",
            "shipCode":"",
            "shipName":"",
            "express":"",
            "waybillCode":"",
            "packages":[
                {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]},
                {"express":"","waybillCode":"","items":["product":"","color":"size","":"","sn":""]}
                ]
        },
    ]
}
```
超卖
```
{
    "dispatchNO":"",
    "transfers":[]
}
```
### 返回
> HttpStatus 200
```
{
    "success":true
}
```
```
{
    "success":false,
    "msg":"通知单已作废"
}
```
> 其他 HttpStatus 重试直至超时人工处理

## OMS同步收货
### 功能描述
门店订单的买家退货至非下单门店或者出库包裹拒收退货发货门店

### 请求说明
> **method**: POST 
> 
> **query**: 待定
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
transferCode | string | SCM调入单号(OMS下发发货通知单)
dispatchNO | string | POS下达的发货通知单号 
receiverCode | string | 波司登出库网点编号 
receiverName | string | 波司登出库网点名称  
express | string | 退货涉及库快递代码
waybillCode | string | 退货运单号
product | string | 货号
color | string | 色号
size | string | 尺码
sn | string | 件码
success | boolean | 出库状态是否同步至POS
msg |string | 同步失败原因

### 请求
> HttpStatus 200
```
{
    "transferCode":"",
    "dispatchNO":"",
    "receiverCode":"",
    "receiverName":"",
    "express":"",
    "waybillCode":"",
    "product":"",
    "color":"",
    "size":"",
    "sn":"",
    
}
```
### 返回
```
{
    "success":true
}
```
> 其他 HttpStatus 重试直至超时人工处理

## OMS同步快递单更改
### 功能描述
OMS同步运单更改

### 请求说明
> **method**: POST 
> 
> **query**: 待定
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
targetbillCode | string | 被更改运单号
express | string | 新运单快递代号
waybillCode | string | 新运单号
success | boolean | 入库状态是否同步至POS
msg |string | 同步失败原因

### 请求
> HttpStatus 200
```
{
    "targetbillCode":"",
    "express":"",
    "waybillCode":""
}
```
### 返回
```
{
    "success":true
}
```
> 其他 HttpStatus 重试直至超时人工处理


# 门店发货篇

## OMS下发电商发货通知单至POS
### 功能描述
电商产生线上线下需求的发货通知单同步至发货门店
### 请求说明
> **method**: POST 
> 
> **query**: pushorder
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | OMS发货通知单号 
shopCode | string | 发货门店 
purchaseNumber | string | 销售小票号 （是否有必要）
receiver | string | 收件人 
phone | string | 电话 
province | string | 省 
city | string | 市 
county | string | 区县 
address | string | 地址 
express | string | 偏好快递方式 
product | string | 货号 
color | string | 色号 
size | string | 尺码 
qty | int | 件数 
success | boolean | 发货通知单是否同步至POS
msg |string | 失败信息

### 请求
```
{
    "shopCode":"",
    "dispatchNO":"",
    "purchaseNumber":"",
    "receiver":"",
    "phone":"",
    "province":"",
    "city":"",
    "county":"",
    "address":"",
    "express":"",
    "items":[
        {"product":"G1","color":"C1","size":"S1","qty":2},
        {"product":"G2","color":"C2","size":"S2","qty":3},
    ]
}
```
### 返回
```
{
    "success":true
}
```
> 其他 HttpStatus 重试直至超时人工处理
> 
> 实时校验库存调用退单接口

## OMS作废电商发货通知单
### 功能描述
电商作废推送至发货门店的发货通知单
### 请求说明
> **method**: POST 
> 
> **query**: 待定
>  
> **contentType**: application/json 
>
> **提供方**: POS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | 欲作废的发货通知单号 
shopCode | string | 发货门店 
success | boolean | 发货通知单作废状态是否同步至POS
msg |string | 失败信息

### 请求
```
{
    "dispatchNO":"",
    "shopCode":""
}
```
### 返回
> HttpStatus 200
```
{
    "success":true
}
```
```
{
    "success":false,
    "msg":"通知单已出库"
}
```
> 其他 HttpStatus 重试直至超时人工处理
## 门店出库回传
### 功能描述
门店完成电商的出库通知单,回传出库信息
### 请求说明
> **method**: POST 
> 
> **path**: pos/checkout
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | 发货通知单号 
over24hReason |string | 超时24小时原因...
express | string | 快递公司代号
waybillCode | string | 运单号
sn | string | 件码
success | boolean | 发货通知单出库是否回传至OMS
msg |string | 失败信息

### 请求
```
{
    "dispatchNO":"",
    "over24hReason":"",
    "packages":[
        {"express":"","waybillCode":"","sns":["","",""]},
        {"express":"","waybillCode":"","sns":["","",""]}]
    ]
}
```
### 返回
> HttpStatus 200
```
{
    "success":true
}
```
```
{
    "success":false,
    "msg":"通知单已作废"
}
```
> 其他 HttpStatus 重试直至超时人工处理
## 门店退单
### 功能描述
门店传递OMS无法完成发货通知
### 请求说明
> **method**: POST 
> 
> **path**: pos/refuse
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | 发货通知单号 
reason |string | 退单原因
success | boolean | 退单信息是否同步至OMS
msg |string | 失败信息

### 请求
```
{
    "dispatchNO":"",
    "reason":""
}
```
### 返回
> HttpStatus 200
```
{
    "success":true,
}
```
> 其他 HttpStatus 重试直至超时人工处理
## 门店接收退货
### 功能描述
门店在出库后接到了退货商品
### 请求说明
> **method**: POST 
> 
> **path**: pos/refund
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
shopCode | string | 接收退货的门店 
express | string | 退货使用的快递类型 
waybillCode |string | 退货涉及的运单号
sn |string | 退货商品件码
success | boolean | 退货信息是否同步至OMS
msg |string? | 失败信息

### 请求
```
{
    "shopCode":"",
    "express":"",
    "waybillCode":"",
    "sn":""
}
```
### 返回
> HttpStatus 200
```
{
    "success":true,
}
```
> 其他 HttpStatus 重试直至超时人工处理

## 门店更改出库运单号
### 功能描述
发货门店在同步出库后更改快递单号,将更改信息同步至OMS
### 请求说明
> **method**: POST 
> 
> **path**: pos/delivery/waybillchange
>  
> **contentType**: application/json 
>
> **提供方**: OMS

### 请求参数
字段 | 类型 | 说明 
-|-|-
dispatchNO | string | 发货通知单号 
targetbillCode |string | 需要修改得运单号
express |string | 更改后的快递类型
waybillCode |string | 更改后的运单号
success | boolean | 退货信息是否同步至OMS
msg |string | 失败信息

### 请求
```
{
    "dispatchNO":"",
    "targetbillCode":"",
    "express":"",
    "waybillCode":""
}
```
### 返回
> HttpStatus 200
```
{
    "success":true,
    "msg":""
}
```
> 其他 HttpStatus 重试直至超时人工处理
