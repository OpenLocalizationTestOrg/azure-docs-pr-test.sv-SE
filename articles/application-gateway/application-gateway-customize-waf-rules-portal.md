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
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="562ac-103">Anpassa web application brandväggsregler via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="562ac-103">Customize web application firewall rules through hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="562ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="562ac-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="562ac-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="562ac-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="562ac-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="562ac-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="562ac-107">Brandvägg för hello Azure Programgateway webbaserade program (Brandvägg) ger skydd för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="562ac-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="562ac-108">Dessa skydd tillhandahålls av hello öppna Web Application säkerhet projekt (OWASP) Core regeln ange (CR).</span><span class="sxs-lookup"><span data-stu-id="562ac-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="562ac-109">Vissa regler kan leda till falska positiva identifieringar och blockera verkliga trafik.</span><span class="sxs-lookup"><span data-stu-id="562ac-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="562ac-110">Därför innehåller Programgateway hello kapaciteten toocustomize regelgrupper och regler.</span><span class="sxs-lookup"><span data-stu-id="562ac-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="562ac-111">Mer information om hello specifik regelgrupper och regler finns [listan över web brandväggen CR regeln programgrupper och regler](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="562ac-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

>[!NOTE]
> <span data-ttu-id="562ac-112">Om inte din Programgateway används hello Brandvägg nivå visas hello alternativet tooupgrade hello programmet gateway toohello Brandvägg nivån i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="562ac-112">If your application gateway is not using hello WAF tier, hello option tooupgrade hello application gateway toohello WAF tier appears in hello right pane.</span></span> 

![Aktivera Brandvägg][fig1]

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="562ac-114">Visa grupper av regeln och regler</span><span class="sxs-lookup"><span data-stu-id="562ac-114">View rule groups and rules</span></span>

<span data-ttu-id="562ac-115">**tooview regelgrupper och regler**</span><span class="sxs-lookup"><span data-stu-id="562ac-115">**tooview rule groups and rules**</span></span>
   1. <span data-ttu-id="562ac-116">Bläddra toohello Programgateway och markera **Brandvägg för webbaserade program**.</span><span class="sxs-lookup"><span data-stu-id="562ac-116">Browse toohello application gateway, and then select **Web application firewall**.</span></span>  
   2. <span data-ttu-id="562ac-117">Välj **avancerade regelkonfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="562ac-117">Select **Advanced rule configuration**.</span></span>  
   <span data-ttu-id="562ac-118">Den här vyn visas en tabell på hello sida i alla hello regelgrupper med hello valt regeluppsättning.</span><span class="sxs-lookup"><span data-stu-id="562ac-118">This view shows a table on hello page of all hello rule groups provided with hello chosen rule set.</span></span> <span data-ttu-id="562ac-119">Alla hello regel kryssrutorna är markerade.</span><span class="sxs-lookup"><span data-stu-id="562ac-119">All of hello rule's check boxes are selected.</span></span>

![Konfigurera inaktiverade regler][1]

## <a name="search-for-rules-toodisable"></a><span data-ttu-id="562ac-121">Sök efter regler toodisable</span><span class="sxs-lookup"><span data-stu-id="562ac-121">Search for rules toodisable</span></span>

<span data-ttu-id="562ac-122">Hej **Web application brandväggsinställningar** bladet ger hello toofilter hello regler via textsökning.</span><span class="sxs-lookup"><span data-stu-id="562ac-122">hello **Web application firewall settings** blade provides hello capability toofilter hello rules through a text search.</span></span> <span data-ttu-id="562ac-123">hello resultatet visar endast hello regelgrupper och regler som innehåller hello text du söker efter.</span><span class="sxs-lookup"><span data-stu-id="562ac-123">hello result displays only hello rule groups and rules that contain hello text you searched for.</span></span>

![Sök efter regler][2]

## <a name="disable-rule-groups-and-rules"></a><span data-ttu-id="562ac-125">Inaktivera regelgrupper och regler</span><span class="sxs-lookup"><span data-stu-id="562ac-125">Disable rule groups and rules</span></span>

<span data-ttu-id="562ac-126">När din är inaktivera regler, du kan inaktivera en hel grupp eller särskilda regler under en eller flera regelgrupper.</span><span class="sxs-lookup"><span data-stu-id="562ac-126">When your're disabling rules, you can disable an entire rule group or specific rules under one or more rule groups.</span></span> 

<span data-ttu-id="562ac-127">**toodisable regelgrupper eller särskilda regler**</span><span class="sxs-lookup"><span data-stu-id="562ac-127">**toodisable rule groups or specific rules**</span></span>

   1. <span data-ttu-id="562ac-128">Sök efter hello regler eller regelgrupper som du vill toodisable.</span><span class="sxs-lookup"><span data-stu-id="562ac-128">Search for hello rules or rule groups that you want toodisable.</span></span>
   2. <span data-ttu-id="562ac-129">Avmarkera kryssrutorna för hello för hello regler som du vill toodisable.</span><span class="sxs-lookup"><span data-stu-id="562ac-129">Clear hello check boxes for hello rules that you want toodisable.</span></span> 
   2. <span data-ttu-id="562ac-130">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="562ac-130">Select **Save**.</span></span> 

![Spara ändringar][3]

## <a name="next-steps"></a><span data-ttu-id="562ac-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="562ac-132">Next steps</span></span>

<span data-ttu-id="562ac-133">När du har konfigurerat din inaktiverade regler kan du lära dig hur tooview Brandvägg loggarna.</span><span class="sxs-lookup"><span data-stu-id="562ac-133">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="562ac-134">Mer information finns i [Programgateway diagnostik](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="562ac-134">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
