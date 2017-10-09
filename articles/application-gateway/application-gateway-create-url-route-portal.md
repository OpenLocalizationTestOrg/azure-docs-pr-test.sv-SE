---
title: "en sökväg-baserad aaaCreate regel - Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Lär dig hur hello toocreate en sökväg-baserade regler för en Programgateway med hjälp av portalen"
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
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a><span data-ttu-id="02a4c-103">Skapa en sökväg-baserade regel för en Programgateway med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="02a4c-103">Create a Path-based rule for an application gateway by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="02a4c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="02a4c-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="02a4c-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="02a4c-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="02a4c-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="02a4c-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="02a4c-107">URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="02a4c-107">URL Path-based routing enables you tooassociate routes based on hello URL path of Http request.</span></span> <span data-ttu-id="02a4c-108">Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som anges i hello Programgateway och skickar hello nätverket trafik toohello definierats backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="02a4c-108">It checks if there is a route tooa back-end pool configured for hello URL listed in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="02a4c-109">Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="02a4c-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="02a4c-110">URL-baserade routning introducerar en ny regel typen tooapplication gateway.</span><span class="sxs-lookup"><span data-stu-id="02a4c-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="02a4c-111">Programgateway har två regeltyper: basic och sökväg-baserade regler.</span><span class="sxs-lookup"><span data-stu-id="02a4c-111">Application gateway has two rule types: basic and path-based rules.</span></span> <span data-ttu-id="02a4c-112">Hej grundläggande regeltyp, tillhandahåller resursallokering tjänst för hello backend-pooler när sökvägen regler dessutom tooround robin distribution, också beaktas sökvägar för hello URL-begäran vid val hello lämpliga serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="02a4c-112">hello basic rule type, provides round-robin service for hello back-end pools while path-based rules in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello appropriate backend pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="02a4c-113">Scenario</span><span class="sxs-lookup"><span data-stu-id="02a4c-113">Scenario</span></span>

<span data-ttu-id="02a4c-114">hello genomgår följande scenario skapar en sökväg-baserade regel i en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="02a4c-114">hello following scenario goes through creating a Path-based rule in an existing application gateway.</span></span>
<span data-ttu-id="02a4c-115">hello scenariot förutsätter att du har redan följt hello stegen för[skapa en Programgateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="02a4c-115">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

![URL-väg][scenario]

## <span data-ttu-id="02a4c-117"><a name="createrule"></a>Skapa hello sökväg-baserade regel</span><span class="sxs-lookup"><span data-stu-id="02a4c-117"><a name="createrule"></a>Create hello Path-based rule</span></span>

<span data-ttu-id="02a4c-118">En sökväg-baserade regel kräver sin egen lyssnare, innan du skapar hello regeln vara att tooverify som du har en tillgänglig lyssnare toouse.</span><span class="sxs-lookup"><span data-stu-id="02a4c-118">A Path-based rule requires its own listener, before creating hello rule be sure tooverify you have an available listener toouse.</span></span>

### <a name="step-1"></a><span data-ttu-id="02a4c-119">Steg 1</span><span class="sxs-lookup"><span data-stu-id="02a4c-119">Step 1</span></span>

<span data-ttu-id="02a4c-120">Navigera toohello [Azure-portalen](http://portal.azure.com) och välj en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="02a4c-120">Navigate toohello [Azure portal](http://portal.azure.com) and select an existing application gateway.</span></span> <span data-ttu-id="02a4c-121">Klicka på **regler**</span><span class="sxs-lookup"><span data-stu-id="02a4c-121">Click **Rules**</span></span>

![Översikt över Application Gateway][1]

### <a name="step-2"></a><span data-ttu-id="02a4c-123">Steg 2</span><span class="sxs-lookup"><span data-stu-id="02a4c-123">Step 2</span></span>

<span data-ttu-id="02a4c-124">Klicka på **sökväg-baserade** knappen tooadd en ny sökväg-baserade regel.</span><span class="sxs-lookup"><span data-stu-id="02a4c-124">Click **Path-based** button tooadd a new Path-based rule.</span></span>

### <a name="step-3"></a><span data-ttu-id="02a4c-125">Steg 3</span><span class="sxs-lookup"><span data-stu-id="02a4c-125">Step 3</span></span>

<span data-ttu-id="02a4c-126">Hej **Lägg till sökväg-baserade regler** bladet innehåller två avsnitt.</span><span class="sxs-lookup"><span data-stu-id="02a4c-126">hello **Add path-based rule** blade has two sections.</span></span> <span data-ttu-id="02a4c-127">hello första avsnittet är där du har definierat hello lyssnare, hello namnet på regeln för hello och hello standardinställningarna för sökvägen.</span><span class="sxs-lookup"><span data-stu-id="02a4c-127">hello first section is where you defined hello listener, hello name of hello rule and hello default path settings.</span></span> <span data-ttu-id="02a4c-128">hello standardinställningarna för sökvägen är för vägar som inte omfattas av hello dirigering av anpassade sökväg-baserad.</span><span class="sxs-lookup"><span data-stu-id="02a4c-128">hello default path settings are for routes that do not fall under hello custom path-based route.</span></span> <span data-ttu-id="02a4c-129">Hej andra avsnittet av hello **Lägg till sökväg-baserade regler** bladet är där du definierar hello sökväg-baserade regler för sig själva.</span><span class="sxs-lookup"><span data-stu-id="02a4c-129">hello second section of hello **Add path-based rule** blade is where you define hello path-based rules themselves.</span></span>

<span data-ttu-id="02a4c-130">**Grundläggande inställningar**</span><span class="sxs-lookup"><span data-stu-id="02a4c-130">**Basic Settings**</span></span>

* <span data-ttu-id="02a4c-131">**Namnet** -värdet är ett eget namn toohello regeln som är tillgänglig i hello portal.</span><span class="sxs-lookup"><span data-stu-id="02a4c-131">**Name** - This value is a friendly name toohello rule that is accessible in hello portal.</span></span>
* <span data-ttu-id="02a4c-132">**Lyssnare** -värdet är hello lyssnare som används för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="02a4c-132">**Listener** - This value is hello listener that is used for hello rule.</span></span>
* <span data-ttu-id="02a4c-133">**Standard serverdelspool** -inställningen är hello-inställning som definierar hello backend-toobe används för hello standardregel</span><span class="sxs-lookup"><span data-stu-id="02a4c-133">**Default backend pool** - This setting is hello setting that defines hello back-end toobe used for hello default rule</span></span>
* <span data-ttu-id="02a4c-134">**Standardinställningarna för HTTP-** -inställningen är hello-inställning som definierar hello HTTP inställningar toobe används för hello Standardregeln.</span><span class="sxs-lookup"><span data-stu-id="02a4c-134">**Default HTTP settings** - This setting is hello setting that defines hello HTTP settings toobe used for hello default rule.</span></span>

<span data-ttu-id="02a4c-135">**Sökväg-baserade regler**</span><span class="sxs-lookup"><span data-stu-id="02a4c-135">**Path-based rules**</span></span>

* <span data-ttu-id="02a4c-136">**Namnet** -värdet är ett eget namn toopath-baserade regler.</span><span class="sxs-lookup"><span data-stu-id="02a4c-136">**Name** - This value is a friendly name toopath-based rule.</span></span>
* <span data-ttu-id="02a4c-137">**Sökvägar** -inställningen definierar hello sökväg hello regeln söker efter när vidarebefordrar trafik</span><span class="sxs-lookup"><span data-stu-id="02a4c-137">**Paths** - This setting defines hello path hello rule looks for when forwarding traffic</span></span>
* <span data-ttu-id="02a4c-138">**Serverdelspool** -inställningen är hello-inställning som definierar hello backend-toobe används för hello regel</span><span class="sxs-lookup"><span data-stu-id="02a4c-138">**Backend Pool** - This setting is hello setting that defines hello back-end toobe used for hello rule</span></span>
* <span data-ttu-id="02a4c-139">**HTTP-inställningen** -inställningen är hello-inställning som definierar hello HTTP inställningar toobe används för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="02a4c-139">**HTTP setting** - This setting is hello setting that defines hello HTTP settings toobe used for hello rule.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="02a4c-140">Sökvägar: hello lista över sökvägen mönster toomatch.</span><span class="sxs-lookup"><span data-stu-id="02a4c-140">Paths: hello list of path patterns toomatch.</span></span> <span data-ttu-id="02a4c-141">Var och en måste börja med / och hello endast placera en ”\*” tillåts är hello slutet.</span><span class="sxs-lookup"><span data-stu-id="02a4c-141">Each must start with / and hello only place a "\*" is allowed is at hello end.</span></span> <span data-ttu-id="02a4c-142">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="02a4c-142">Valid examples are /xyz, /xyz* or /xyz/*.</span></span>  

![Lägg till sökväg-baserade regler bladet med information som har fyllt i][2]

<span data-ttu-id="02a4c-144">Lägga till en sökväg-baserade regler tooan är befintliga Programgateway en enkel process hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="02a4c-144">Adding a path-based rule tooan existing application gateway is an easy process through hello portal.</span></span> <span data-ttu-id="02a4c-145">När en sökväg-baserade regler har skapats, kan det vara redigerade tooadd ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="02a4c-145">After a path-based rule has been created, it can be edited tooadd additional rules.</span></span> 

![lägga till ytterligare sökväg-baserade regler][3]

<span data-ttu-id="02a4c-147">Det här steget konfigurerar en väg baserat på sökvägen.</span><span class="sxs-lookup"><span data-stu-id="02a4c-147">This step configures a path-based route.</span></span> <span data-ttu-id="02a4c-148">Det är viktigt toounderstand att begäranden inte skrivas om, som begäran kommer i Programgateway inspektera hello begäran och basic på hello url mönster skickar hello begäran toohello lämplig backend.</span><span class="sxs-lookup"><span data-stu-id="02a4c-148">It is important toounderstand that requests are not rewritten, as requests come in application gateway inspects hello request and basic on hello url pattern sends hello request toohello appropriate back-end.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02a4c-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02a4c-149">Next steps</span></span>

<span data-ttu-id="02a4c-150">hur tooconfigure SSL-avlastning med Azure Application Gateway, se toolearn [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)</span><span class="sxs-lookup"><span data-stu-id="02a4c-150">toolearn how tooconfigure SSL Offloading with Azure Application Gateway, see [Configure SSL Offload](application-gateway-ssl-portal.md)</span></span>

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
