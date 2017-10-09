---
title: "aaaCustomize web application brandväggsregler i Azure Application Gateway - PowerShell | Microsoft Docs"
description: "Den här artikeln innehåller information om hur toocustomize Brandvägg för webbaserade program regler i Programgateway med PowerShell."
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
ms.openlocfilehash: f320e687b0f621515255469dac8e375cdd900dda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a><span data-ttu-id="b7af9-103">Anpassa web application brandväggsregler med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7af9-103">Customize web application firewall rules through PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7af9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b7af9-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="b7af9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7af9-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="b7af9-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b7af9-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="b7af9-107">Brandvägg för hello Azure Programgateway webbaserade program (Brandvägg) ger skydd för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b7af9-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="b7af9-108">Dessa skydd tillhandahålls av hello öppna Web Application säkerhet projekt (OWASP) Core regeln ange (CR).</span><span class="sxs-lookup"><span data-stu-id="b7af9-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="b7af9-109">Vissa regler kan leda till falska positiva identifieringar och blockera verkliga trafik.</span><span class="sxs-lookup"><span data-stu-id="b7af9-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="b7af9-110">Därför innehåller Programgateway hello kapaciteten toocustomize regelgrupper och regler.</span><span class="sxs-lookup"><span data-stu-id="b7af9-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="b7af9-111">Mer information om hello specifik regelgrupper och regler finns [listan över webbprogram programgrupp brandväggen CR regeln och regler](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="b7af9-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS Rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="b7af9-112">Visa grupper av regeln och regler</span><span class="sxs-lookup"><span data-stu-id="b7af9-112">View rule groups and rules</span></span>

<span data-ttu-id="b7af9-113">hello följande kodexempel visar hur tooview regler och regeln grupper som kan konfigureras på en gateway med Brandvägg-aktiverat program.</span><span class="sxs-lookup"><span data-stu-id="b7af9-113">hello following code examples show how tooview rules and rule groups that are configurable on a WAF-enabled application gateway.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="b7af9-114">Visa regelgrupper</span><span class="sxs-lookup"><span data-stu-id="b7af9-114">View rule groups</span></span>

<span data-ttu-id="b7af9-115">följande exempel visar hur hello tooview regelgrupper:</span><span class="sxs-lookup"><span data-stu-id="b7af9-115">hello following example shows how tooview rule groups:</span></span>

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

<span data-ttu-id="b7af9-116">hello följande utdata är trunkerat svar från föregående exempel hello:</span><span class="sxs-lookup"><span data-stu-id="b7af9-116">hello following output is a truncated response from hello preceding example:</span></span>

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a><span data-ttu-id="b7af9-117">Inaktivera regler</span><span class="sxs-lookup"><span data-stu-id="b7af9-117">Disable rules</span></span>

<span data-ttu-id="b7af9-118">hello följande exempel inaktiverar regler `910018` och `910017` på en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="b7af9-118">hello following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="b7af9-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7af9-119">Next steps</span></span>

<span data-ttu-id="b7af9-120">När du har konfigurerat din inaktiverade regler kan du lära dig hur tooview Brandvägg loggarna.</span><span class="sxs-lookup"><span data-stu-id="b7af9-120">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="b7af9-121">Mer information finns i [Gateway Programdiagnostik](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="b7af9-121">For more information, see [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
