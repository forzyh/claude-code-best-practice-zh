---
description: 显示巴基斯坦标准时间 (PKT, UTC+5) 的当前时间
---

# 时间命令

显示巴基斯坦标准时间 (PKT, UTC+5) 的当前日期和时间。

## 说明

1. 运行以下bash命令获取PKT的当前时间：
   ```
   TZ='Asia/Karachi' date '+%Y-%m-%d %H:%M:%S %Z'
   ```

2. 按此格式向用户显示结果：
   ```
   Current Time in Pakistan (PKT): YYYY-MM-DD HH:MM:SS PKT
   ```

## 需求

- 始终使用 `Asia/Karachi` 时区 (UTC+5)
- 使用24小时格式
- 包括日期和时间
- 保持输出简洁
