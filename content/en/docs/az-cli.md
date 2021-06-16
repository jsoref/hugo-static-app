---
title: "Azure CLI"
date: 2021-06-07T15:50:20+08:00
author: Michael Tam
email: michael.tam@fujitsu.com
weight: 3
---

```bash
az group create -l westus2 -n rg-fipdocs0616
```

```bash
az staticwebapp create \
    -n stapp-hugo \
    -g rg-fipdocs0616 \
    -s https://github.com/scout249/staticsite2 \
    -l westus2 \
    -b main \
    --login-with-github \
    --app-location / \
    --output-location public
```

