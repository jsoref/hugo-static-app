
---
title: "Azure CLI"
date: 2021-06-07T15:50:20+08:00
author: Michael Tam
email: michael.tam@fujitsu.com
weight: -2
---

```bash
az group create -l westus2 -n rg-fipdocs0616
```

```bash
az staticwebapp create \
    -n stapp-hugo \
    -g rg-fipdocs0616 \
    -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
    -l westus2 \
    -b main \
    --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
```

