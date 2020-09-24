## 出库回传
##### path
`/scm/checkout/`
##### content-type
`application/json`
##### 参数
parameter | type | Optional | description
----------|------|---------|------------
seq | string(50) | 必填 | 接口调用唯一批号 重推直接返回结果
dn | string(50) | 必填 | 通知单号 
tn | string(50) | 必填 | 快递包裹号
sn | string(50) | 必填 | 22位件码
##### requestbody
```
{
    "seq":"DDDDDDDDD",
    "dn":"OMS123456789",
    "pkg":{
        "tn":"EXPRESS123",
        "sns":["SERIALIZENUMBER"]
    }
}
```

1.回传批号98d23x1da 通知单号 出库1个包裹(面单SF1885704192041),包裹内存在3件衣服
```
{
    "seq":"98d23x1da",
    "dn":"OMS009246908643",
    "pkg":{
            tn:"SF1885704192041",
            sns:[
                "5046351998702494052302",
                "5046352028652491386059",
                "5046414472652494709281"]}}
```
2.回传批号s3f53xbbt 通知单号 出库包裹1(面单SF1885708314178),包裹内存在3件衣服,
包裹2(面单SF1885708314179),包裹内存在4件衣服
```
{
    "seq":"s3f53xbbt",
    "dn":"OMS009246907476",
    "pkg":[
        {
            "tn":"SF1885708314178",
            "sns":[
                "4681714472647473454967",
                "4681814472001472323238",
                "4681815383643472322810"]},
        {
            "tn":"SF1885708314179",
            "sns":[
                "4681950025001473456292",
                "4682551000001472316168",
                "4683314472252472337002",
                "5048952027645494253517"]}]}
```
##### response

httpStatus | description
----------|------
204 | 数据完成同步
409 | 数据无法同步(通常是指通知单为非待出库状态 需要追件)
Other | 重传

## 入库回传
##### path
`/scm/refund`
##### content-type
`application/json`
##### 参数
parameter | type | Optional | description
----------|------|---------|------------
seq | string(50) | 必填 | 接口调用唯一批号 重推直接返回结果
storeCode | string(50) | 必填 | 入库仓编号
dn | string(50) | 可选 | 拒收等能追溯得提供通知单号，疑难件可选 
cc | string(50) | 可选 | 退回快递类型
tn | string(50) | 必填 | 退回包裹号
sn | string(50) | 必填 | 退回件码
ts | string(50) | 必填 | 退回时间(yyyy-MM-dd HH:mm:ss)
##### requestbody
```
{
    "seq":"DDDDDD",
    "storeCode":"DDD",
    "dn":"OMS009246908643",
    "tn":"EXPRESS123",
    "sn":"5046414472652494709281"}
```
1.回传批号4frd21d6u 通知单号OMS009246908643 退回1件4681950025001473456292,包裹面单SF1885708314178
```
{
    "seq":"4frd21d6u",
    "dn":"OMS009246908643",
    "cc":"SF",
    "tn":"SF1885708314178",
    "sn":"4681950025001473456292",
    "ts":"2020-09-22 12:00:03"
}
```
##### response
httpStatus | description
----------|------
204 | 数据完成同步
Other | 重传

## 退单
##### path
`/scm/refuse`
##### content-type
`application/json`
##### 参数
parameter | type | Optional | description
----------|------|---------|------------
seq | string(50) | 必填 | 接口调用唯一批号 重推直接返回结果
dn | string(50) | 可选 | 拒收等能追溯得提供通知单号，疑难件可选 
msg | string(100) | 可选 | 备注
##### requestbody
```
{
    "seq":"DDDDDD",
    "dn":"OMS009246908643",
    "msg":""}
```
1.回传批号8ud22e996 通知单号OMS009246908643
```
{
    "seq":"8ud22e996",
    "dn":"OMS009246908643",
    "msg:"
}
```
##### response
httpStatus | description
----------|------
204 | OMS以确认发货方不再执行出库操作
Other | 重传
