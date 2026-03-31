---
name: weather-fetcher
description: 从Open-Meteo API获取迪拜阿联酋的当前天气温度数据的说明
user-invocable: false
---

# 天气获取技能

此技能提供获取当前天气数据的说明。

## 任务

获取迪拜阿联酋的当前温度 (以请求的单位为摄氏度或华氏度)。

## 说明

1. **获取天气数据**: 使用WebFetch工具从Open-Meteo API获取迪拜的当前天气数据。

   对于**摄氏度**：
   - URL: `https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius`

   对于**华氏度**：
   - URL: `https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit`

2. **提取温度**: 从JSON响应中提取当前温度：
   - 字段: `current.temperature_2m`
   - 单位标签在: `current_units.temperature_2m`

3. **返回结果**: 清晰地返回温度值和单位。

## 预期输出

完成此技能的说明后：
```
Current Dubai Temperature: [X]°[C/F]
Unit: [Celsius/Fahrenheit]
```

## 注意

- 仅获取温度，不执行任何转换或写入文件
- Open-Meteo是免费的，无需API密钥，使用基于坐标的查询以确保可靠性
- 迪拜坐标: 纬度 25.2048, 经度 55.2708
- 清晰地返回数值温度值和单位
- 根据调用者的请求支持摄氏度和华氏度
