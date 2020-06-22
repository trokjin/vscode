# 2020电商退货规则
## EWM持有发货字典
- SN(SKU),订单平台，出库时间
- 门店发货数据由下单平台同步至EWM
## 唯品会退货抵达售后
- 退货想箱码,对应SN(SKU)，SN(SKU) 直接推送电商
- 缺吊牌的请物流做好补打流程，过程中保持退货箱码跟SKU的原子性
## 其他退货抵达售后
- 退货实体:快递单，SN(SKU)，退货无吊牌，回传|匹配[归属方,归属方订单],[无归属]库存所在仓
  >由于电商退货占大部分比重，无法直接匹配得货品算电商库存
- EWM根据发货字典（收货时间最接近但是不晚于出库时间得项目确定归属方）直接匹配归属，同步退货消息至归属下单平台
- 同步可以拒绝，比如电商事先根据快递签收已经通过快递凭证退货( 拒绝后的退货实体设置无归属方)
- 无匹配的退货，后期由归属平台根据快递单凭证认领
- 门店订单（讨论是否采用由EWM匹配确定归属问题，直接由客服发起回传归属）
  
## 平台售后客服根据快递单号
- 要求EWM返回退货信息快递单，SN(SKU)，无吊牌，归属方,归属方订单
- 客服认领此快递单下得无归属退货
- 客服向已经有归属得平台申请转让(撤销占用)，以便通过无归属退货（涉及得退款以折扣处理）
- 确认快递丢件,同步快递丢件信息至EWM
- 客服发送转寄单（寄回快递单号,寄回物件,寄送信息）请求,同步寄出信息，后期财务核算
  

   
