## OMS提问
##### 提供方
POS
##### 调用方
OMS
##### content-type
`application/json`
##### 参数
parameter | type | Optional | description
----------|------|---------|------------
issueId | string(50) | 必填 | 工单唯一批号 重推直接返回结果
issueType | string(50) | 必填 | 工单类型(催件、撤回、疑难)
dn | string(50) | 必填 | 涉及通知单号
shopCode | string(50) | 必填 | 接收门店代号 
question | string(50) |(200) | 必填 | 提问消息
drawable | base64String | 可选 | 涉及媒体
ts | string(20) | 必填 | 发起时间(yyyy-MM-dd HH:mm:ss)
##### requestbody
```
{
    "issueId":"12131313131",
    "issueType":"催件",
    "dn":"OMS123456789",
    "shopCode":"A038",
    "question":"是否在退回中",
    "drawable":"iVBORw0KGgoAAAANSUhEUgAAAnwA....",
    "ts":"2020-09-25 12:00:00"
}
```

## POS答复
##### 提供方
OMS
##### path
`pos/reply`
##### 调用方
POS
##### content-type
`application/json`
##### 参数
parameter | type | Optional | description
----------|------|---------|------------
issueId | string(50) | 必填 | 工单唯一批号 重推直接返回结果
answer | string(50)(200) | 必填 | 答复消息
ts | string(20) | 必填 | 答复时间(yyyy-MM-dd HH:mm:ss)
##### requestbody
```
{
    "issueId":"12131313131",
    "answer":"今晚退回",
    "ts":"2020-09-25 18:00:00"
}
```

##### response

httpStatus | description
----------|------
204 | 数据完成同步(issueId 已同步)
Other | 重传
