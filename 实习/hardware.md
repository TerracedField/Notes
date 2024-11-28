思路：

硬件暂时不用光强、温度的阈值，自动化，由后端来开关风扇和灯即可，超过阈值风扇开转直到降到阈值-5°，超过3个小时高温则产生日志

## 后端发给硬件的数据

### conn1

```json
{
  	"light_mode":1,
  	"light_level":1
}
```

| light_mode  | 灯开关，0：关，1：开 |
| ----------- | -------------------- |
| light_level | 灯挡位（0-100）      |

### conn2

```json
{
    "pump_ctrl_state":0,
    "pump_power_state":0,
    "fan_mode":0,
    "fan_level":50
}
```

| pump_ctrl_state  | 水泵电源开关，0：关，1：开 |
| ---------------- | -------------------------- |
| pump_power_state | 水泵控制开关，0：关，1：开 |
| fan_mode         | 风扇开关，0：关，1：开     |
| fan_level        | 风扇挡位（20-100）         |

## 硬件发给后端数据

### conn1

发布主题：tobacco_01b

订阅主题：ctl-a-1

```json
{
    "light_intensity":116,
    "light_mode":1,
	"light_level":50
}
```

### conn2

发布主题：tobacco_01b

订阅主题：ctl-b-1

```json
{
  	"soil_humidity":60,
    "pump_ctrl_state":0,
    "pump_power_state":0,
	"air_temperature":20,
    "fan_mode":1,
    "fan_level":1
}
```