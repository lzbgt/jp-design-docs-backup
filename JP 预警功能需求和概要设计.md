陆宗宝 2024/1

## 背景

当前异常检测会触发应急处理， 项目上需要一个轻量级的异常检测：无需应急处理， 不涉及用户反馈（异常确认， 异常否认）， 仅仅作为单向通知到现场用户。 我们称之为预警功能。

## 实现概要

基于当前JunoPlatform平台， 将预计功能作为一个独立的算法模块来开发。

1.  关注的预警点位： 由jp runtime的 input.json指定点位列表
2.  预警逻辑超参： 由jp runtime的config.json指定预警算法自身需要使用的超参。 如： 合理点位范围，超限百分比， 时长， 屏蔽时长等 。 由算法自定义， 并可通过调参页实时调参
3.  前端/后端：
    1.  使用与异常检测相同的数据通道
    2.  后端需要对预警payload进行日志存储
    3.  前端对预警payload渲染（当且仅当payload包含alarm\_type字段且值为&quot;pre-alarm&quot;）：
        1.  popup, 只有确认按钮， 实时显示当前算法给出的所有预警
        2.  专用历史查询页面
4.  算法：
    1.  使用 dealer发送预警payload到前端。
    2.  payload定义

```python
# type1: alarm message
data = {
  "alarm_type": "pre-alarm",
  "et": "2024/01/01 01:00:00", # str
  "data" : [
    {
      "tag": "左边总磷检测仪新增“，
      "alarm": int,  # 大于0 告警
      "message": str,
      "value": float # 当前值
    },
  ]
}

# type2: data message, eg. 三江中水反渗透模块数据分析图表数据

data = {
  "data_type": "pre_alarm",
  "et": "2024/01/01 01:00:00", # str
  "data" : {
    "tpd": {
      "name": "图表模块名",
      "data": [{
          "tag": "xxxx",
          "values": [], # 1 ... N 个值
          "names": []   # 每个值的名称
      }]
    }
    ,
    "srr": {
      "name": "图表模块名", 
      "data": [
      {
        "tag": "yyyy",
        "values": [], # 1 ... N 个值
        "names": []   # 每个值的名称
      }]
    },
    
    "wrr": {
      "name": "bbbb",
      "data":[
      {
        "tag": "zzzz",
        "values": [], # 1 ... N 个值
        "names": []   # 每个值的名称
      }],
    }
  }
}



payload = json.dumps(data)

storage.dealer.write(f"{junoconfig["plant"]}.jp-backend", payload)
```