---
title: "Skapa en Programgateway med hjälp av Routning regler för URL - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att skapa, konfigurera en gateway för Azure-program som använder routningsregler för URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="e596e-103">Skapa en Programgateway använder sökväg-baserade routning med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e596e-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e596e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e596e-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="e596e-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e596e-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="e596e-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e596e-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="e596e-107">URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för en Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="e596e-107">URL Path-based routing enables you to associate routes based on the URL path of an Http request.</span></span> <span data-ttu-id="e596e-108">Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen och skickar nätverkstrafiken till definierade backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-108">It checks if there is a route to a back-end pool configured for the URL presented in the Application Gateway and sends the network traffic to the defined back-end pool.</span></span> <span data-ttu-id="e596e-109">Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="e596e-109">A common use for URL-based routing is to load balance requests for different content types to different back-end server pools.</span></span>

<span data-ttu-id="e596e-110">URL-baserade routning introducerar en ny regeltyp av i Programgateway.</span><span class="sxs-lookup"><span data-stu-id="e596e-110">URL-based routing introduces a new rule type to application gateway.</span></span> <span data-ttu-id="e596e-111">Programgateway har två regeltyper: basic och PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="e596e-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="e596e-112">Grundläggande regeltyp resursallokering tjänst för backend-pooler när PathBasedRouting förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen vid val av backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-112">Basic rule type provides round-robin service for the back-end pools while PathBasedRouting in addition to round robin distribution, also takes path pattern of the request URL into account while choosing the back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="e596e-113">Scenario</span><span class="sxs-lookup"><span data-stu-id="e596e-113">Scenario</span></span>

<span data-ttu-id="e596e-114">I följande exempel Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: en standard-serverpoolen och en bild serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-114">In the following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="e596e-115">Begäranden för http://contoso.com/image * dirigeras till bilden serverpoolen (imagesBackendPool), om inte matchar sökvägar, poolen server standard (appGatewayBackendPool) är markerad.</span><span class="sxs-lookup"><span data-stu-id="e596e-115">Requests for http://contoso.com/image* are routed to image server pool (imagesBackendPool), if the path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![URL-väg](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a><span data-ttu-id="e596e-117">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="e596e-117">Log in to Azure</span></span>

<span data-ttu-id="e596e-118">Öppna den **Kommandotolken för Microsoft Azure**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="e596e-118">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="e596e-119">Du kan också använda `az login` utan att växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="e596e-119">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="e596e-120">När du skriver i föregående exempel, har en kod angetts.</span><span class="sxs-lookup"><span data-stu-id="e596e-120">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="e596e-121">Navigera till https://aka.ms/devicelogin i en webbläsare för att fortsätta inloggningen.</span><span class="sxs-lookup"><span data-stu-id="e596e-121">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd visar enhet inloggning][1]

<span data-ttu-id="e596e-123">Ange den kod som du fick i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e596e-123">In the browser, enter the code you received.</span></span> <span data-ttu-id="e596e-124">Du omdirigeras till en inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e596e-124">You are redirected to a sign-in page.</span></span>

![webbläsare för att ange koden][2]

<span data-ttu-id="e596e-126">När du har angett koden du är inloggad, Stäng webbläsaren för att fortsätta scenariot.</span><span class="sxs-lookup"><span data-stu-id="e596e-126">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![loggat in][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a><span data-ttu-id="e596e-128">Lägga till en sökväg-baserade regel till en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="e596e-128">Add a path-based rule to an existing application gateway</span></span>

<span data-ttu-id="e596e-129">Skapa en Programgateway med en regel för en definierad</span><span class="sxs-lookup"><span data-stu-id="e596e-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="e596e-130">Skapa en ny backend-pool</span><span class="sxs-lookup"><span data-stu-id="e596e-130">Create a new back-end pool</span></span>

<span data-ttu-id="e596e-131">Konfigurera gateway programinställning **imagesBackendPool** för nätverkstrafik Utjämning av nätverksbelastning i backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-131">Configure application gateway setting **imagesBackendPool** for the load-balanced network traffic in the back-end pool.</span></span> <span data-ttu-id="e596e-132">I det här exemplet kan du konfigurera olika backend-processpool-inställningar för den nya backend-poolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-132">In this example, you configure different back-end pool settings for the new back-end pool.</span></span> <span data-ttu-id="e596e-133">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="e596e-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="e596e-134">Serverdelens HTTP-inställningar används av regler för att dirigera trafik till rätt medlemmar i serverdelspoolen.</span><span class="sxs-lookup"><span data-stu-id="e596e-134">Backend HTTP settings are used by rules to route traffic to the correct backend pool members.</span></span> <span data-ttu-id="e596e-135">Anger protokoll och port som används när du skickar trafik till backend-poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="e596e-135">This determines the protocol and port that is used when sending traffic to the backend pool members.</span></span> <span data-ttu-id="e596e-136">Cookie-baserade sessioner bestäms också av serverdelens HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="e596e-136">Cookie-based sessions are also determined by the backend HTTP settings.</span></span>  <span data-ttu-id="e596e-137">Om cookie-baserad sessionstillhörighet är aktiverat skickas trafiken till samma serverdel som tidigare begäranden för varje paket.</span><span class="sxs-lookup"><span data-stu-id="e596e-137">If enabled, cookie-based session affinity sends traffic to the same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="e596e-138">Skapa en ny frontend-port</span><span class="sxs-lookup"><span data-stu-id="e596e-138">Create a new front-end port</span></span>

<span data-ttu-id="e596e-139">Konfigurera klientdelsporten för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="e596e-139">Configure the front-end port for an application gateway.</span></span> <span data-ttu-id="e596e-140">Konfigurationsobjektet för klientdelsporten används av en lyssnare för att definiera vilken port som Application Gateway lyssnar efter trafik på.</span><span class="sxs-lookup"><span data-stu-id="e596e-140">The front-end port configuration object is used by a listener to define what port the Application Gateway listens for traffic on the listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="e596e-141">Skapa en ny lyssnare</span><span class="sxs-lookup"><span data-stu-id="e596e-141">Create a new listener</span></span>

<span data-ttu-id="e596e-142">Konfigurera lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e596e-142">Configure the listener.</span></span> <span data-ttu-id="e596e-143">Det här steget konfigurerar lyssnaren för den offentliga IP-adressen och porten som används för att ta emot inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="e596e-143">This step configures the listener for the public IP address and port used to receive incoming network traffic.</span></span> <span data-ttu-id="e596e-144">I följande exempel tar tidigare konfigurerade frontend IP-konfigurationen och frontend portkonfiguration ett protokoll (http eller https) och konfigurerar lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="e596e-144">The following example takes the previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures the listener.</span></span> <span data-ttu-id="e596e-145">I det här exemplet lyssnaren lyssnar efter HTTP-trafik på port 82 på den offentliga IP-adressen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e596e-145">In this example, the listener listens to HTTP traffic on port 82 on the public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a><span data-ttu-id="e596e-146">Skapa Url-sökväg karta</span><span class="sxs-lookup"><span data-stu-id="e596e-146">Create the Url path map</span></span>

<span data-ttu-id="e596e-147">Konfigurera URL: en regel sökvägar för backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="e596e-147">Configure URL rule paths for the back-end pools.</span></span> <span data-ttu-id="e596e-148">Det här steget konfigurerar du den relativa sökvägen som används av Programgateway för att definiera mappning mellan en URL-sökväg och vilka backend-adresspool har tilldelats för den inkommande trafiken.</span><span class="sxs-lookup"><span data-stu-id="e596e-148">This step configures the relative path used by application gateway to define the mapping between URL path and which back-end pool is assigned to handle the incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e596e-149">Varje sökväg måste börja med / och endast en ”\*” är tillåtna, finns i slutet.</span><span class="sxs-lookup"><span data-stu-id="e596e-149">Each path must start with / and the only place a "\*" is allowed, is at the end.</span></span> <span data-ttu-id="e596e-150">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="e596e-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="e596e-151">Strängen som skickas till sökvägen matcher innehåller inte någon text efter först ””? eller ”#”, och dessa tecken är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="e596e-151">The string fed to the path matcher does not include any text after the first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="e596e-152">I följande exempel skapas en regel för ”/ bilder / *” sökväg dirigera trafiken till backend-”imagesBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="e596e-152">The following example creates one rule for "/images/*" path routing traffic to back-end "imagesBackendPool."</span></span> <span data-ttu-id="e596e-153">Den här regeln innebär att trafik för varje uppsättning URL-adresser dirigeras till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="e596e-153">This rule ensures that traffic for each set of urls is routed to the backend.</span></span> <span data-ttu-id="e596e-154">Till exempel går http://adatum.com/images/figure1.jpg till ”imagesBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="e596e-154">For example, http://adatum.com/images/figure1.jpg goes to "imagesBackendPool."</span></span> <span data-ttu-id="e596e-155">Om sökvägen inte matchar någon av de fördefinierade sökvägsreglerna, konfigurerar regelkonfigurationen sökväg kartan även en standard backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="e596e-155">If the path doesn't match any of the pre-defined path rules, the rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="e596e-156">Till exempel går http://adatum.com/shoppingcart/test.html till pool1 som har definierats som standard för omatchade trafik.</span><span class="sxs-lookup"><span data-stu-id="e596e-156">For example, http://adatum.com/shoppingcart/test.html goes to pool1 as it is defined as the default pool for unmatched traffic.</span></span>

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a><span data-ttu-id="e596e-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e596e-157">Next steps</span></span>

<span data-ttu-id="e596e-158">Om du vill veta om Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e596e-158">If you want to learn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
