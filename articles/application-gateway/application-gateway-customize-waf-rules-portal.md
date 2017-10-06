---
title: "aaaCustomize web application brandväggsregler i Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Den här artikeln innehåller information om hur regler toocustomize Brandvägg för webbaserade program i Programgateway med hello Azure-portalen."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
ms.openlocfilehash: 36a999279e0370b9f803e12257856a56753b23a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a>Anpassa web application brandväggsregler via hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Brandvägg för hello Azure Programgateway webbaserade program (Brandvägg) ger skydd för webbprogram. Dessa skydd tillhandahålls av hello öppna Web Application säkerhet projekt (OWASP) Core regeln ange (CR). Vissa regler kan leda till falska positiva identifieringar och blockera verkliga trafik. Därför innehåller Programgateway hello kapaciteten toocustomize regelgrupper och regler. Mer information om hello specifik regelgrupper och regler finns [listan över web brandväggen CR regeln programgrupper och regler](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Om inte din Programgateway används hello Brandvägg nivå visas hello alternativet tooupgrade hello programmet gateway toohello Brandvägg nivån i hello till höger. 

![Aktivera Brandvägg][fig1]

## <a name="view-rule-groups-and-rules"></a>Visa grupper av regeln och regler

**tooview regelgrupper och regler**
   1. Bläddra toohello Programgateway och markera **Brandvägg för webbaserade program**.  
   2. Välj **avancerade regelkonfigurationen**.  
   Den här vyn visas en tabell på hello sida i alla hello regelgrupper med hello valt regeluppsättning. Alla hello regel kryssrutorna är markerade.

![Konfigurera inaktiverade regler][1]

## <a name="search-for-rules-toodisable"></a>Sök efter regler toodisable

Hej **Web application brandväggsinställningar** bladet ger hello toofilter hello regler via textsökning. hello resultatet visar endast hello regelgrupper och regler som innehåller hello text du söker efter.

![Sök efter regler][2]

## <a name="disable-rule-groups-and-rules"></a>Inaktivera regelgrupper och regler

När din är inaktivera regler, du kan inaktivera en hel grupp eller särskilda regler under en eller flera regelgrupper. 

**toodisable regelgrupper eller särskilda regler**

   1. Sök efter hello regler eller regelgrupper som du vill toodisable.
   2. Avmarkera kryssrutorna för hello för hello regler som du vill toodisable. 
   2. Välj **Spara**. 

![Spara ändringar][3]

## <a name="next-steps"></a>Nästa steg

När du har konfigurerat din inaktiverade regler kan du lära dig hur tooview Brandvägg loggarna. Mer information finns i [Programgateway diagnostik](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
