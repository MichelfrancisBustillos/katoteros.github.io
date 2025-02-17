---
title: Monitoring Diskspace with Uptime Kuma
categories: [Scripts]
tags: [uptime kuma, monitoring, scripts]
---

## Monitoring Diskspace with Uptime Kuma

- Script:

```bash
#!/usr/bin/env bash

disk="/dev/mapper/ubuntu--vg-ubuntu--lv"  # replace with the disk that you are monitoring

percentage=$(df -hl --total ${disk} | tail -1 | awk '{printf $5}')

threshold="80"  # 80% seems reasonable, but YMMV

number=${percentage%\%*}

message="Used space on ${disk} is ${number}%"

push_url="https://status.katoteros.org/api/push/PUSHURL" # replace with Uptime Kuma Push URL ID

if [ $number -lt $threshold ]; then
    service_status="up"
else
    service_status="down"
fi

curl \
    --get \
    --data-urlencode "status=${service_status}" \
    --data-urlencode "msg=${message}" \
    --data-urlencode "ping=${number}" \
    --silent \
    ${push_url} \
> /dev/null
```

- Crontab (every 5 minutes): `*/5 * * * * PATH_TO_SCRIPT`
