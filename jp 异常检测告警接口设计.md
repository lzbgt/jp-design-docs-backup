陆宗宝， 王娇 2023/11/20 draft

```text
# zmq: jp-algo[i]  < -- dealer -- > jp-connector[router]  < -- zmq: dealer -- > jp-uibackend

tcp://192.168.101.158:2302
tcp://jp-connector:2302
content:
    frame 0: self_id := id
    frame 1: target_id := id
             id := module_id|jp-uibackend
             module_id := plant . module 
             jp-uibackend := plant. "jp-backend"
             
    frame 2: payload
        payload type1: algo -> jp-backend
    {
        "module": str,
        "alarms": [
            {
                "tag": str,
                "alarm": int, # 0: no alarm/recover, # 1: active alarm 
                "value": float,
                "time": "%Y/%m/%d %H:%M:%S", # ie: "2023/11/19 10:20:30", localtime
            }
        ]
    }

        payload type2:  jp-backend -> algo
    {     
        "module": str,
        "data": [
        	{
            	"tag": str,
                "value": int,  # -1: unkonwn, 0: negtive, 1: postive
                "time": str,   #“2023/11/20 10:20:30”, 原发生时间
        	}
        ],
        "time": str,   #“2023/11/20 10:20:30”,  处理时间
        
    }


# jp-backend <-> jp-web
## 1. ws proto
ws push to ui:
{
        "module": str,
        "alarms": [
            {
                "tag": str,
                "alarm": int, # 0: no alarm/recover, # 1: active alarm 
                "value": float,
                "time": "%Y/%m/%d %H:%M:%S", # ie: "2023/11/19 10:20:30", localtime
            }
        ]
}

## 2. restful proto
POST /api/confirm_alarms:
{  
 "data":[
    {   
        "module": str,
        "confirms": [
          {
            "tag": str,
            "value": int,  # -1: unkonwn, 0: negtive, 1: postive
            "time": str,   #“2023/11/20 10:20:30”, 原发生时间
          }
        ],
        "time": str,   #“2023/11/20 10:20:30”, 处理时间
    }
 ]
}
```