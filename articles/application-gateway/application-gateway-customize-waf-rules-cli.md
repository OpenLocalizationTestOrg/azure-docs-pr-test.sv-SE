---
title: "aaaCustomize web application brandväggsregler i Azure Application Gateway - Azure CLI 2.0 | Microsoft Docs"
description: "Den här artikeln innehåller information om hur regler toocustomize Brandvägg för webbaserade program i Programgateway med hello Azure CLI 2.0."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: b83ffb9f6a7e0d0c8c970885d2bcb3b63d32581c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-cli-20"></a><span data-ttu-id="3f7a6-103">Anpassa web application brandväggsregler via hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3f7a6-103">Customize web application firewall rules through hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f7a6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3f7a6-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="3f7a6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f7a6-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="3f7a6-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="3f7a6-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="3f7a6-107">Brandvägg för hello Azure Programgateway webbaserade program (Brandvägg) ger skydd för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3f7a6-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="3f7a6-108">Dessa skydd tillhandahålls av hello öppna Web Application säkerhet projekt (OWASP) Core regeln ange (CR).</span><span class="sxs-lookup"><span data-stu-id="3f7a6-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="3f7a6-109">Vissa regler kan leda till falska positiva identifieringar och blockera verkliga trafik.</span><span class="sxs-lookup"><span data-stu-id="3f7a6-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="3f7a6-110">Därför innehåller Programgateway hello kapaciteten toocustomize regelgrupper och regler.</span><span class="sxs-lookup"><span data-stu-id="3f7a6-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="3f7a6-111">Mer information om hello specifik regelgrupper och regler finns [listan över web brandväggen CR regeln programgrupper och regler](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="3f7a6-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="3f7a6-112">Visa grupper av regeln och regler</span><span class="sxs-lookup"><span data-stu-id="3f7a6-112">View rule groups and rules</span></span>

<span data-ttu-id="3f7a6-113">hello följande kodexempel visar hur tooview regler och regeln grupper som kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="3f7a6-113">hello following code examples show how tooview rules and rule groups that are configurable.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="3f7a6-114">Visa regelgrupper</span><span class="sxs-lookup"><span data-stu-id="3f7a6-114">View rule groups</span></span>

<span data-ttu-id="3f7a6-115">hello som följande exempel visar hur tooview hello regelgrupper:</span><span class="sxs-lookup"><span data-stu-id="3f7a6-115">hello following example shows how tooview hello rule groups:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

<span data-ttu-id="3f7a6-116">hello följande utdata är trunkerat svar från föregående exempel hello:</span><span class="sxs-lookup"><span data-stu-id="3f7a6-116">hello following output is a truncated response from hello preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a><span data-ttu-id="3f7a6-117">Visa regler i en grupp</span><span class="sxs-lookup"><span data-stu-id="3f7a6-117">View rules in a rule group</span></span>

<span data-ttu-id="3f7a6-118">hello som följande exempel visar hur tooview regler i en angiven regelgrupp:</span><span class="sxs-lookup"><span data-stu-id="3f7a6-118">hello following example shows how tooview rules in a specified rule group:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

<span data-ttu-id="3f7a6-119">hello följande utdata är trunkerat svar från föregående exempel hello:</span><span class="sxs-lookup"><span data-stu-id="3f7a6-119">hello following output is a truncated response from hello preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a><span data-ttu-id="3f7a6-120">Inaktivera regler</span><span class="sxs-lookup"><span data-stu-id="3f7a6-120">Disable rules</span></span>

<span data-ttu-id="3f7a6-121">hello följande exempel inaktiverar regler `910018` och `910017` på en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="3f7a6-121">hello following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="3f7a6-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f7a6-122">Next steps</span></span>

<span data-ttu-id="3f7a6-123">När du har konfigurerat din inaktiverade regler kan du lära dig hur tooview Brandvägg loggarna.</span><span class="sxs-lookup"><span data-stu-id="3f7a6-123">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="3f7a6-124">Mer information finns i [Programgateway diagnostik](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="3f7a6-124">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
