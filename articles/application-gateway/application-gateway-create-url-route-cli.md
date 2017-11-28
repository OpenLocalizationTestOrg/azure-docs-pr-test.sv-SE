---
title: "regler för en Programgateway med hjälp av Routning av URL - aaaCreate Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som använder routningsregler för URL"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a><span data-ttu-id="c2644-103">Skapa en Programgateway använder sökväg-baserade routning med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c2644-103">Create an application gateway using Path-based routing with Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c2644-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c2644-104">Azure portal</span></span>](application-gateway-create-url-route-portal.md)
> * [<span data-ttu-id="c2644-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c2644-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-url-route-arm-ps.md)
> * [<span data-ttu-id="c2644-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c2644-106">Azure CLI 2.0</span></span>](application-gateway-create-url-route-cli.md)

<span data-ttu-id="c2644-107">URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för en Http-begäran.</span><span class="sxs-lookup"><span data-stu-id="c2644-107">URL Path-based routing enables you tooassociate routes based on hello URL path of an Http request.</span></span> <span data-ttu-id="c2644-108">Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som visas i hello Programgateway och skickar hello nätverket trafik toohello definierats backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c2644-108">It checks if there is a route tooa back-end pool configured for hello URL presented in hello Application Gateway and sends hello network traffic toohello defined back-end pool.</span></span> <span data-ttu-id="c2644-109">Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="c2644-109">A common use for URL-based routing is tooload balance requests for different content types toodifferent back-end server pools.</span></span>

<span data-ttu-id="c2644-110">URL-baserade routning introducerar en ny regel typen tooapplication gateway.</span><span class="sxs-lookup"><span data-stu-id="c2644-110">URL-based routing introduces a new rule type tooapplication gateway.</span></span> <span data-ttu-id="c2644-111">Programgateway har två regeltyper: basic och PathBasedRouting.</span><span class="sxs-lookup"><span data-stu-id="c2644-111">Application gateway has two rule types: basic and PathBasedRouting.</span></span> <span data-ttu-id="c2644-112">Grundläggande regeltyp tillhandahåller resursallokering tjänst för hello backend-pooler när PathBasedRouting dessutom tooround robin distribution kan också beaktas sökvägar för hello URL-begäran vid val hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c2644-112">Basic rule type provides round-robin service for hello back-end pools while PathBasedRouting in addition tooround robin distribution, also takes path pattern of hello request URL into account while choosing hello back-end pool.</span></span>

## <a name="scenario"></a><span data-ttu-id="c2644-113">Scenario</span><span class="sxs-lookup"><span data-stu-id="c2644-113">Scenario</span></span>

<span data-ttu-id="c2644-114">I följande exempel hello, Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: en standard-serverpoolen och en bild serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="c2644-114">In hello following example, Application Gateway is serving traffic for contoso.com with two back-end server pools: a default server pool and an image server pool.</span></span>

<span data-ttu-id="c2644-115">Begäranden för http://contoso.com/image * är dirigeras tooimage serverpoolen (imagesBackendPool), om hello sökvägar matchar inte, poolen server standard (appGatewayBackendPool) är markerad.</span><span class="sxs-lookup"><span data-stu-id="c2644-115">Requests for http://contoso.com/image* are routed tooimage server pool (imagesBackendPool), if hello path pattern does not match, a default server pool (appGatewayBackendPool) is selected.</span></span>

![URL-väg](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a><span data-ttu-id="c2644-117">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="c2644-117">Log in tooAzure</span></span>

<span data-ttu-id="c2644-118">Öppna hello **Kommandotolken för Microsoft Azure**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="c2644-118">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="c2644-119">Du kan också använda `az login` utan hello växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="c2644-119">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="c2644-120">När du skriver hello föregående exempel, har en kod angetts.</span><span class="sxs-lookup"><span data-stu-id="c2644-120">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="c2644-121">Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="c2644-121">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd visar enhet inloggning][1]

<span data-ttu-id="c2644-123">Ange hello koden du fått i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c2644-123">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="c2644-124">Du kan omdirigerade tooa inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="c2644-124">You are redirected tooa sign-in page.</span></span>

![webbläsaren tooenter kod][2]

<span data-ttu-id="c2644-126">När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.</span><span class="sxs-lookup"><span data-stu-id="c2644-126">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![loggat in][3]

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a><span data-ttu-id="c2644-128">Lägg till en sökväg-baserade regler tooan befintliga Programgateway</span><span class="sxs-lookup"><span data-stu-id="c2644-128">Add a path-based rule tooan existing application gateway</span></span>

<span data-ttu-id="c2644-129">Skapa en Programgateway med en regel för en definierad</span><span class="sxs-lookup"><span data-stu-id="c2644-129">Create an application gateway with a path rule defined</span></span>

### <a name="create-a-new-back-end-pool"></a><span data-ttu-id="c2644-130">Skapa en ny backend-pool</span><span class="sxs-lookup"><span data-stu-id="c2644-130">Create a new back-end pool</span></span>

<span data-ttu-id="c2644-131">Konfigurera gateway programinställning **imagesBackendPool** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c2644-131">Configure application gateway setting **imagesBackendPool** for hello load-balanced network traffic in hello back-end pool.</span></span> <span data-ttu-id="c2644-132">I det här exemplet kan du konfigurera olika backend-processpool-inställningar för hello ny backend-pool.</span><span class="sxs-lookup"><span data-stu-id="c2644-132">In this example, you configure different back-end pool settings for hello new back-end pool.</span></span> <span data-ttu-id="c2644-133">Varje serverdelspool kan ha sin egen serverdelspoolinställning.</span><span class="sxs-lookup"><span data-stu-id="c2644-133">Each back-end pool can have its own back-end pool setting.</span></span>  <span data-ttu-id="c2644-134">Serverdelens HTTP-inställningar som används av regler tooroute trafik toohello rätt backend poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="c2644-134">Backend HTTP settings are used by rules tooroute traffic toohello correct backend pool members.</span></span> <span data-ttu-id="c2644-135">Detta avgör hello protokoll och port som används när en skickar trafik toohello backend poolmedlemmar.</span><span class="sxs-lookup"><span data-stu-id="c2644-135">This determines hello protocol and port that is used when sending traffic toohello backend pool members.</span></span> <span data-ttu-id="c2644-136">Cookie-baserad sessioner också bestäms av hello serverdelens HTTP-inställningar.</span><span class="sxs-lookup"><span data-stu-id="c2644-136">Cookie-based sessions are also determined by hello backend HTTP settings.</span></span>  <span data-ttu-id="c2644-137">Om aktiverad, cookie-baserad session tillhörighet skickar trafik toohello samma backend som tidigare begäranden för varje paket.</span><span class="sxs-lookup"><span data-stu-id="c2644-137">If enabled, cookie-based session affinity sends traffic toohello same backend as previous requests for each packet.</span></span>

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a><span data-ttu-id="c2644-138">Skapa en ny frontend-port</span><span class="sxs-lookup"><span data-stu-id="c2644-138">Create a new front-end port</span></span>

<span data-ttu-id="c2644-139">Konfigurera hello frontend-port för en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="c2644-139">Configure hello front-end port for an application gateway.</span></span> <span data-ttu-id="c2644-140">konfigurationsobjekt för hello frontend-port används av en lyssnare toodefine vad port hello Programgateway lyssnar efter trafik på hello lyssnare.</span><span class="sxs-lookup"><span data-stu-id="c2644-140">hello front-end port configuration object is used by a listener toodefine what port hello Application Gateway listens for traffic on hello listener.</span></span>

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a><span data-ttu-id="c2644-141">Skapa en ny lyssnare</span><span class="sxs-lookup"><span data-stu-id="c2644-141">Create a new listener</span></span>

<span data-ttu-id="c2644-142">Konfigurera hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c2644-142">Configure hello listener.</span></span> <span data-ttu-id="c2644-143">Det här steget konfigurerar hello-lyssnare för hello offentlig IP-adress och port används tooreceive inkommande nätverkstrafik.</span><span class="sxs-lookup"><span data-stu-id="c2644-143">This step configures hello listener for hello public IP address and port used tooreceive incoming network traffic.</span></span> <span data-ttu-id="c2644-144">hello följande exempel tar hello som tidigare har konfigurerat frontend IP-konfiguration, frontend portkonfiguration och ett protokoll (http eller https) och konfigurerar hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="c2644-144">hello following example takes hello previously configured front-end IP configuration,  front-end port configuration, and a protocol (http or https) and configures hello listener.</span></span> <span data-ttu-id="c2644-145">I det här exemplet lyssnar hello lyssnare tooHTTP trafik på port 82 på hello offentlig IP-adress som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c2644-145">In this example, hello listener listens tooHTTP traffic on port 82 on hello public IP address that was created earlier.</span></span>

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a><span data-ttu-id="c2644-146">Skapa hello Url-sökväg karta</span><span class="sxs-lookup"><span data-stu-id="c2644-146">Create hello Url path map</span></span>

<span data-ttu-id="c2644-147">Konfigurera URL: en regel sökvägar för hello backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="c2644-147">Configure URL rule paths for hello back-end pools.</span></span> <span data-ttu-id="c2644-148">Det här steget konfigurerar hello relativ sökväg används av gateway toodefine hello programmappning mellan URL-sökväg och vilka backend-adresspool har tilldelats toohandle hello inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="c2644-148">This step configures hello relative path used by application gateway toodefine hello mapping between URL path and which back-end pool is assigned toohandle hello incoming traffic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2644-149">Varje sökväg måste börja med / och hello endast placera en ”\*” tillåts, är hello slutet.</span><span class="sxs-lookup"><span data-stu-id="c2644-149">Each path must start with / and hello only place a "\*" is allowed, is at hello end.</span></span> <span data-ttu-id="c2644-150">Giltiga exempel är /xyz, /xyz* eller/xyz / *.</span><span class="sxs-lookup"><span data-stu-id="c2644-150">Valid examples are /xyz, /xyz* or /xyz/*.</span></span> <span data-ttu-id="c2644-151">hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först ””? eller ”#”, och dessa tecken är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="c2644-151">hello string fed toohello path matcher does not include any text after hello first "?" or "#", and those characters are not allowed.</span></span> 

<span data-ttu-id="c2644-152">hello följande exempel skapas en regel för ”/ bilder / *” sökväg routning trafik tooback slutpunkt ”imagesBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="c2644-152">hello following example creates one rule for "/images/*" path routing traffic tooback-end "imagesBackendPool."</span></span> <span data-ttu-id="c2644-153">Den här regeln garanterar att trafik för varje uppsättning URL: er är routade toohello backend.</span><span class="sxs-lookup"><span data-stu-id="c2644-153">This rule ensures that traffic for each set of urls is routed toohello backend.</span></span> <span data-ttu-id="c2644-154">Till exempel http://adatum.com/images/figure1.jpg går för ”imagesBackendPool”.</span><span class="sxs-lookup"><span data-stu-id="c2644-154">For example, http://adatum.com/images/figure1.jpg goes too"imagesBackendPool."</span></span> <span data-ttu-id="c2644-155">Om hello sökväg inte matchar någon av hello fördefinierade sökvägsregler, konfigurerar hello regelkonfigurationen sökväg kartan även en standard backend-adresspool.</span><span class="sxs-lookup"><span data-stu-id="c2644-155">If hello path doesn't match any of hello pre-defined path rules, hello rule path map configuration also configures a default back-end address pool.</span></span> <span data-ttu-id="c2644-156">Till exempel blir http://adatum.com/shoppingcart/test.html toopool1 som har definierats som hello Standardpool för omatchade trafik.</span><span class="sxs-lookup"><span data-stu-id="c2644-156">For example, http://adatum.com/shoppingcart/test.html goes toopool1 as it is defined as hello default pool for unmatched traffic.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c2644-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2644-157">Next steps</span></span>

<span data-ttu-id="c2644-158">Om du vill toolearn om Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c2644-158">If you want toolearn about Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl-cli.md).</span></span>


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
