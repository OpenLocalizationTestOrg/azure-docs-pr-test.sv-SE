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
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Anpassa web application brandväggsregler med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Brandvägg för hello Azure Programgateway webbaserade program (Brandvägg) ger skydd för webbprogram. Dessa skydd tillhandahålls av hello öppna Web Application säkerhet projekt (OWASP) Core regeln ange (CR). Vissa regler kan leda till falska positiva identifieringar och blockera verkliga trafik. Därför innehåller Programgateway hello kapaciteten toocustomize regelgrupper och regler. Mer information om hello specifik regelgrupper och regler finns [listan över webbprogram programgrupp brandväggen CR regeln och regler](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Visa grupper av regeln och regler

hello följande kodexempel visar hur tooview regler och regeln grupper som kan konfigureras på en gateway med Brandvägg-aktiverat program.

### <a name="view-rule-groups"></a>Visa regelgrupper

följande exempel visar hur hello tooview regelgrupper:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

hello följande utdata är trunkerat svar från föregående exempel hello:

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

## <a name="disable-rules"></a>Inaktivera regler

hello följande exempel inaktiverar regler `910018` och `910017` på en Programgateway:

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a>Nästa steg

När du har konfigurerat din inaktiverade regler kan du lära dig hur tooview Brandvägg loggarna. Mer information finns i [Gateway Programdiagnostik](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
