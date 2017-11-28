---
title: "Skapa en regel för sökväg-baserade - Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Lär dig hur du skapar en sökväg-baserade regler för en Programgateway med hjälp av portalen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: c184e94a04cfbdedcae70ed154aeb7dd134d1baf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a><span data-ttu-id="ee1b3-103">Skapa en sökväg-baserade regel för en Programgateway med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="ee1b3-103">Create a Path-based rule for an application gateway by using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ee1b3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ee1b3-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="ee1b3-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ee1b3-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="ee1b3-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ee1b3-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="ee1b3-107">URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-107">URL Path-based routing enables you to associate routes based on the URL path of Http request.</span></span> <span data-ttu-id="ee1b3-108">Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen och skickar nätverkstrafiken till definierade backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-108">It checks if there is a route to a back-end pool configured for the URL listed in the Application Gateway and sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="ee1b3-109">Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-109">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="ee1b3-110">URL-baserade routning introducerar en ny regeltyp av i Programgateway.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-110">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="ee1b3-111">Programgateway har två regeltyper: basic och sökväg-baserade regler.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-111">Application gateway has two rule types: basic and path-based rules.</span></span> <span data-ttu-id="ee1b3-112">Grundläggande regeltypen resursallokering tjänst för backend-pooler när sökväg-baserade regler förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen när du väljer rätt serverdelspoolen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-112">The basic rule type, provides round-robin service for the back-end pools while path-based rules in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the appropriate backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="ee1b3-113">Scenario</span><span class="sxs-lookup"><span data-stu-id="ee1b3-113">Scenario</span></span>

<span data-ttu-id="ee1b3-114">Följande scenario genomgår skapar en sökväg-baserade regel i en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-114">The following scenario goes through creating a Path-based rule in an existing application gateway.</span></span>
<span data-ttu-id="ee1b3-115">Scenariot förutsätter att du redan har följt stegen för att [skapa en Programgateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ee1b3-115">The scenario assumes that you have already followed the steps to [Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

![URL-väg][scenario]

## <span data-ttu-id="ee1b3-117"><a name="createrule"></a>Skapa regel sökväg-baserade</span><span class="sxs-lookup"><span data-stu-id="ee1b3-117"><a name="createrule"></a>Create the Path-based rule</span></span>

<span data-ttu-id="ee1b3-118">En sökväg-baserade regel kräver sin egen lyssnare, innan du skapar regeln bör du kontrollera du ha en lyssnare som är tillgängliga att använda.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-118">A Path-based rule requires its own listener, before creating the rule be sure to verify you have an available listener to use.</span></span>

### <a name="step-1"></a><span data-ttu-id="ee1b3-119">Steg 1</span><span class="sxs-lookup"><span data-stu-id="ee1b3-119">Step 1</span></span>

<span data-ttu-id="ee1b3-120">Navigera till den [Azure-portalen](http://portal.azure.com) och välj en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-120">Navigate to the [Azure portal](http://portal.azure.com) and select an existing application gateway.</span></span> <span data-ttu-id="ee1b3-121">Klicka på **regler**</span><span class="sxs-lookup"><span data-stu-id="ee1b3-121">Click **Rules**</span></span>

![Översikt över Application Gateway][1]

### <a name="step-2"></a><span data-ttu-id="ee1b3-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="ee1b3-123">Step 2</span></span>

<span data-ttu-id="ee1b3-124">Klicka på **sökväg-baserade** för att lägga till en ny sökväg-baserade regel.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-124">Click **Path-based** button to add a new Path-based rule.</span></span>

### <a name="step-3"></a><span data-ttu-id="ee1b3-125">Steg 3</span><span class="sxs-lookup"><span data-stu-id="ee1b3-125">Step 3</span></span>

<span data-ttu-id="ee1b3-126">Den **Lägg till sökväg-baserade regler** bladet innehåller två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-126">The **Add path-based rule** blade has two sections.</span></span> <span data-ttu-id="ee1b3-127">Det första avsnittet är där du har definierat lyssnaren, namnet på regeln och standardinställningarna för sökvägen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-127">The first section is where you defined the listener, the name of the rule and the default path settings.</span></span> <span data-ttu-id="ee1b3-128">Standardinställningarna för sökvägen är för vägar som inte omfattas av anpassade sökväg-baserade vägen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-128">The default path settings are for routes that do not fall under the custom path-based route.</span></span> <span data-ttu-id="ee1b3-129">Det andra avsnittet av den **Lägg till sökväg-baserade regler** bladet är där du kan definiera regler baserat på sökvägen sig själva.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-129">The second section of the **Add path-based rule** blade is where you define the path-based rules themselves.</span></span>

<span data-ttu-id="ee1b3-130">**Grundläggande inställningar**</span><span class="sxs-lookup"><span data-stu-id="ee1b3-130">**Basic Settings**</span></span>

* <span data-ttu-id="ee1b3-131">**Namnet** -värdet är ett eget namn för regeln som är tillgänglig i portalen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-131">**Name** - This value is a friendly name to the rule that is accessible in the portal.</span></span>
* <span data-ttu-id="ee1b3-132">**Lyssnare** -värdet är lyssnare som används för regeln.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-132">**Listener** - This value is the listener that is used for the rule.</span></span>
* <span data-ttu-id="ee1b3-133">**Standard serverdelspool** -inställningen är den inställning som definierar backend som ska användas för Standardregeln</span><span class="sxs-lookup"><span data-stu-id="ee1b3-133">**Default backend pool** - This setting is the setting that defines the back-end to be used for the default rule</span></span>
* <span data-ttu-id="ee1b3-134">**Standardinställningarna för HTTP-** -inställningen är den inställning som definierar HTTP-inställningar som ska användas för Standardregeln.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-134">**Default HTTP settings** - This setting is the setting that defines the HTTP settings to be used for the default rule.</span></span>

<span data-ttu-id="ee1b3-135">**Sökväg-baserade regler**</span><span class="sxs-lookup"><span data-stu-id="ee1b3-135">**Path-based rules**</span></span>

* <span data-ttu-id="ee1b3-136">**Namnet** -värdet är ett eget namn för regeln för sökväg-baserade.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-136">**Name** - This value is a friendly name to path-based rule.</span></span>
* <span data-ttu-id="ee1b3-137">**Sökvägar** -den här inställningen definierar sökvägen regeln söker efter när vidarebefordrar trafik</span><span class="sxs-lookup"><span data-stu-id="ee1b3-137">**Paths** - This setting defines the path the rule looks for when forwarding traffic</span></span>
* <span data-ttu-id="ee1b3-138">**Serverdelspool** -inställningen är den inställning som definierar backend som ska användas för regeln</span><span class="sxs-lookup"><span data-stu-id="ee1b3-138">**Backend Pool** - This setting is the setting that defines the back-end to be used for the rule</span></span>
* <span data-ttu-id="ee1b3-139">**HTTP-inställningen** -inställningen är den inställning som definierar HTTP-inställningar som ska användas för regeln.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-139">**HTTP setting** - This setting is the setting that defines the HTTP settings to be used for the rule.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee1b3-140">Sökvägar: Listan över sökvägen mönster som matchar.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-140">Paths: The list of path patterns to match.</span></span> <span data-ttu-id="ee1b3-141">Var och en måste börja med / och endast en ”\*” tillåts i slutet.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-141">Each must start with / and the only place a "\*" is allowed is at the end.</span></span> <span data-ttu-id="ee1b3-142">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-142">Valid examples are /xyz, /xyz* or /xyz/*.</span></span>  

![Lägg till sökväg-baserade regler bladet med information som har fyllt i][2]

<span data-ttu-id="ee1b3-144">Lägger till en sökväg-baserade regel i en befintlig Programgateway är en enkel process via portalen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-144">Adding a path-based rule to an existing application gateway is an easy process through the portal.</span></span> <span data-ttu-id="ee1b3-145">När en sökväg-baserade regler har skapats kan redigeras den för att lägga till ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-145">After a path-based rule has been created, it can be edited to add additional rules.</span></span> 

![lägga till ytterligare sökväg-baserade regler][3]

<span data-ttu-id="ee1b3-147">Det här steget konfigurerar en väg baserat på sökvägen.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-147">This step configures a path-based route.</span></span> <span data-ttu-id="ee1b3-148">Det är viktigt att förstå som begär inte anges som begäran kommer i Programgateway inspektera begäran och basic i url-mönster skickar en begäran till lämplig serverdel.</span><span class="sxs-lookup"><span data-stu-id="ee1b3-148">It is important to understand that requests are not rewritten, as requests come in application gateway inspects the request and basic on the url pattern sends the request to the appropriate back-end.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee1b3-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee1b3-149">Next steps</span></span>

<span data-ttu-id="ee1b3-150">Information om hur du konfigurerar SSL-avlastning med Azure Programgateway finns [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ee1b3-150">To learn how to configure SSL Offloading with Azure Application Gateway, see [Configure SSL Offload](application-gateway-ssl-portal.md)</span></span>

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
