---
title: "Värd för flera platser med Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att konfigurera en befintlig Azure-program-gateway som värd för flera webbprogram på samma gateway med Azure-portalen."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 84bd62ae17b7f7ba4cd815ef1f9880679607ebce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="2e5f1-103">Konfigurera en befintlig Programgateway som värd för flera webbprogram</span><span class="sxs-lookup"><span data-stu-id="2e5f1-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2e5f1-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2e5f1-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="2e5f1-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2e5f1-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="2e5f1-106">Värd för flera plats kan du distribuera flera webbprogram på samma programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-106">Multiple site hosting allows you to deploy more than one web application on the same application gateway.</span></span> <span data-ttu-id="2e5f1-107">Det är beroende av förekomsten av värdhuvudet i den inkommande HTTP-begäranden att avgöra vilka lyssnare skulle ta emot trafik.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-107">It relies on presence of host header in the incoming HTTP request, to determine which listener would receive traffic.</span></span> <span data-ttu-id="2e5f1-108">Lyssnaren dirigerar sedan trafik till lämplig serverdelspool som konfigurerats i regler definitionen av gatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-108">The listener then directs traffic to appropriate backend pool as configured in the rules definition of the gateway.</span></span> <span data-ttu-id="2e5f1-109">I SSL aktiverat webbprogram Programgateway förlitar sig på servern Servernamnsindikation (SNI)-tillägg för att välja rätt lyssnaren för Internet-trafik.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-109">In SSL enabled web applications, application gateway relies on the Server Name Indication (SNI) extension to choose the correct listener for the web traffic.</span></span> <span data-ttu-id="2e5f1-110">Ett vanligt användningsområde för värd för flera plats är att belastningsutjämna förfrågningar för olika webbdomäner till olika backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-110">A common use for multiple site hosting is to load balance requests for different web domains to different back-end server pools.</span></span> <span data-ttu-id="2e5f1-111">På samma sätt kan flera underdomäner på samma rotdomänen också finnas på samma programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-111">Similarly multiple subdomains of the same root domain could also be hosted on the same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="2e5f1-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="2e5f1-112">Scenario</span></span>

<span data-ttu-id="2e5f1-113">I följande exempel Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-113">In the following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="2e5f1-114">Liknande installationsprogrammet kan användas för att värden underdomäner som app.contoso.com och blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-114">Similar setup could be used to host subdomains like app.contoso.com and blog.contoso.com.</span></span>

![Multisite scenario][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="2e5f1-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2e5f1-116">Before you begin</span></span>

<span data-ttu-id="2e5f1-117">Det här scenariot lägger till stöd för flera platser i en befintlig Programgateway.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-117">This scenario adds multi-site support to an existing application gateway.</span></span> <span data-ttu-id="2e5f1-118">En befintlig Programgateway måste kunna konfigureras för att slutföra det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-118">To complete this scenario, an existing application gateway needs to be available to configure.</span></span> <span data-ttu-id="2e5f1-119">Besök [skapa en Programgateway med hjälp av portalen](application-gateway-create-gateway-portal.md) att lära dig hur du skapar en basic-Programgateway i portalen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-119">Visit [Create an application gateway by using the portal](application-gateway-create-gateway-portal.md) to learn how to create a basic application gateway in the portal.</span></span>

<span data-ttu-id="2e5f1-120">Här följer de steg som krävs för att uppdatera programgatewayen:</span><span class="sxs-lookup"><span data-stu-id="2e5f1-120">The following are the steps needed to update the application gateway:</span></span>

1. <span data-ttu-id="2e5f1-121">Skapa backend-pooler för varje plats.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-121">Create back-end pools to use for each site.</span></span>
2. <span data-ttu-id="2e5f1-122">Skapa en lyssnare för varje plats Programgateway stöder.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="2e5f1-123">Skapa regler för att mappa varje lyssnare med lämpliga serverdel.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-123">Create rules to map each listener with the appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="2e5f1-124">Krav</span><span class="sxs-lookup"><span data-stu-id="2e5f1-124">Requirements</span></span>

* <span data-ttu-id="2e5f1-125">**Backend-serverpool:** Listan med IP-adresser för backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-125">**Back-end server pool:** The list of IP addresses of the back-end servers.</span></span> <span data-ttu-id="2e5f1-126">IP-adresserna som anges bör antingen tillhöra det virtuella undernätet eller vara en offentlig IP-/VIP-adress.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-126">The IP addresses listed should either belong to the virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="2e5f1-127">FQDN kan också användas.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-127">FQDN can also be used.</span></span>
* <span data-ttu-id="2e5f1-128">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="2e5f1-129">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-129">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="2e5f1-130">**Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-130">**Front-end port:** This port is the public port that is opened on the application gateway.</span></span> <span data-ttu-id="2e5f1-131">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-131">Traffic hits this port, and then gets redirected to one of the back-end servers.</span></span>
* <span data-ttu-id="2e5f1-132">**Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="2e5f1-132">**Listener:** The listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="2e5f1-133">För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="2e5f1-134">**Regel:** regeln Binder lyssnaren poolen backend-server och definierar vilka backend-serverpoolen trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-134">**Rule:** The rule binds the listener, the back-end server pool, and defines which back-end server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="2e5f1-135">Regler bearbetas i angiven ordning och trafik dirigeras via den första regeln som matchar oavsett särskilda egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-135">Rules are processed in the order they are listed, and traffic will be directed via the first rule that matches regardless of specificity.</span></span> <span data-ttu-id="2e5f1-136">Till exempel om du har en regel med hjälp av en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port måste regeln med flera platser lyssnaren anges innan en regel med grundläggande lyssnare för flera platser regeln för att fungera som förväntat.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on the same port, the rule with the multi-site listener must be listed before the rule with the basic listener in order for the multi-site rule to function as expected.</span></span> 
* <span data-ttu-id="2e5f1-137">**Certifikat:** varje lyssnare kräver ett unikt certifikat, i det här exemplet skapas 2-lyssnare för flera platser.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="2e5f1-138">Två PFX-certifikat och lösenord för att de måste skapas.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-138">Two .pfx certificates and the passwords for them need to be created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="2e5f1-139">Skapa backend-pooler för varje plats</span><span class="sxs-lookup"><span data-stu-id="2e5f1-139">Create back-end pools for each site</span></span>

<span data-ttu-id="2e5f1-140">En backend-poolen för varje plats att programmet gateway stöder krävs, i det här fallet 2 är att skapa en för contoso11.com och en för fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e5f1-141">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2e5f1-141">Step 1</span></span>

<span data-ttu-id="2e5f1-142">Gå till en befintlig Programgateway i Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e5f1-142">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="2e5f1-143">Välj **serverdelspooler** och på **Lägg till**</span><span class="sxs-lookup"><span data-stu-id="2e5f1-143">Select **Backend pools** and click **Add**</span></span>

![Lägg till serverdelspooler][7]

### <a name="step-2"></a><span data-ttu-id="2e5f1-145">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2e5f1-145">Step 2</span></span>

<span data-ttu-id="2e5f1-146">Fyll i information om backend-poolen **pool1**, lägger till ip-adresser eller FQDN för backend-servrar och på **OK**</span><span class="sxs-lookup"><span data-stu-id="2e5f1-146">Fill in the information for the back-end pool **pool1**, adding the ip addresses or FQDNs for the back-end servers and click **OK**</span></span>

![backend pool1 poolinställningarna][8]

### <a name="step-3"></a><span data-ttu-id="2e5f1-148">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2e5f1-148">Step 3</span></span>

<span data-ttu-id="2e5f1-149">Klicka på bladet serverdelspooler **Lägg till** att lägga till en ytterligare backend-adresspool **pool2**, lägga till ip-adresser eller FQDN för backend-servrar och klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="2e5f1-149">On the backend-pools blade click **Add** to add an additional back-end pool **pool2**, adding the ip addresses or FQDNS for the back-end servers and click **OK**</span></span>

![backend pool2 poolinställningarna][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="2e5f1-151">Skapa lyssnare för varje serverdel</span><span class="sxs-lookup"><span data-stu-id="2e5f1-151">Create listeners for each back-end</span></span>

<span data-ttu-id="2e5f1-152">Application Gateway förlitar sig på HTTP 1.1 värdhuvuden för att ha mer än en webbplats på samma offentliga IP-adress och port.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-152">Application Gateway relies on HTTP 1.1 host headers to host more than one website on the same public IP address and port.</span></span> <span data-ttu-id="2e5f1-153">Den grundläggande lyssnare har skapats i portalen innehåller inte den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-153">The basic listener created in the portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="2e5f1-154">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2e5f1-154">Step 1</span></span>

<span data-ttu-id="2e5f1-155">Klicka på **lyssnare** i befintliga Programgateway och på **flera platser** att lägga till den första lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-155">Click **Listeners** on the existing application gateway and click **Multi-site** to add the first listener.</span></span>

![lyssnare översikt bladet][1]

### <a name="step-2"></a><span data-ttu-id="2e5f1-157">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2e5f1-157">Step 2</span></span>

<span data-ttu-id="2e5f1-158">Fyll i informationen för lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-158">Fill out the information for the listener.</span></span> <span data-ttu-id="2e5f1-159">I det här exemplet SSL-avslutning har konfigurerats, skapa en ny frontend-port.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="2e5f1-160">Ladda upp PFX-certifikat som ska användas för SSL-avslutning.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-160">Upload the .pfx certificate to be used for SSL termination.</span></span> <span data-ttu-id="2e5f1-161">Den enda skillnaden på det här bladet jämfört med standard grundläggande lyssnare bladet är värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-161">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span>

![lyssnare egenskapsbladet][2]

### <a name="step-3"></a><span data-ttu-id="2e5f1-163">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2e5f1-163">Step 3</span></span>

<span data-ttu-id="2e5f1-164">Klicka på **flera platser** och skapa en annan lyssnare enligt beskrivningen i föregående steg för den andra platsen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-164">Click **Multi-site** and create another listener as described in the previous step for the second site.</span></span> <span data-ttu-id="2e5f1-165">Se till att använda ett annat certifikat för andra lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-165">Make sure to use a different certificate for the second listener.</span></span> <span data-ttu-id="2e5f1-166">Den enda skillnaden på det här bladet jämfört med standard grundläggande lyssnare bladet är värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-166">The only difference on this blade compared to the standard basic listener blade is the hostname.</span></span> <span data-ttu-id="2e5f1-167">Fyll i informationen för lyssnaren och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-167">Fill out the information for the listener and click **OK**.</span></span>

![lyssnare egenskapsbladet][3]

> [!NOTE]
> <span data-ttu-id="2e5f1-169">Skapa lyssnare i Azure-portalen för Programgateway är en tidskrävande uppgift kan det ta lite tid att skapa två lyssnare i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-169">Creation of listeners in the Azure portal for application gateway is a long running task, it may take some time to create the two listeners in this scenario.</span></span> <span data-ttu-id="2e5f1-170">När du är klar visas lyssnare i portalen som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="2e5f1-170">When complete the listeners show in the portal as seen in the following image:</span></span>

![Översikt över lyssnare][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a><span data-ttu-id="2e5f1-172">Skapa regler för att mappa lyssnare till serverdelspooler</span><span class="sxs-lookup"><span data-stu-id="2e5f1-172">Create rules to map listeners to backend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="2e5f1-173">Steg 1</span><span class="sxs-lookup"><span data-stu-id="2e5f1-173">Step 1</span></span>

<span data-ttu-id="2e5f1-174">Gå till en befintlig Programgateway i Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2e5f1-174">Navigate to an existing application gateway in the Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="2e5f1-175">Välj **regler** och välj befintlig Standardregeln **regel 1** och på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-175">Select **Rules** and choose the existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="2e5f1-176">Steg 2</span><span class="sxs-lookup"><span data-stu-id="2e5f1-176">Step 2</span></span>

<span data-ttu-id="2e5f1-177">Fyll i bladet regler som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-177">Fill out the rules blade as seen in the following image.</span></span> <span data-ttu-id="2e5f1-178">Välja första lyssnare och första poolen och klicka på **spara** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-178">Choosing the first listener and first pool and clicking **Save** when complete.</span></span>

![Redigera en befintlig regel][6]

### <a name="step-3"></a><span data-ttu-id="2e5f1-180">Steg 3</span><span class="sxs-lookup"><span data-stu-id="2e5f1-180">Step 3</span></span>

<span data-ttu-id="2e5f1-181">Klicka på **regel** att skapa den andra regeln.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-181">Click **Basic rule** to create the second rule.</span></span> <span data-ttu-id="2e5f1-182">Fyll i formuläret med andra lyssnare och andra serverdelspoolen och klicka på **OK** att spara.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-182">Fill out the form with the second listener and second backend pool and click **OK** to save.</span></span>

![Lägg till regel bladet][10]

<span data-ttu-id="2e5f1-184">Det här scenariot har slutförts konfigurerar en Programgateway med befintliga med stöd för flera platser via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e5f1-184">This scenario completes configuring an existing application gateway with multi-site support through the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e5f1-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e5f1-185">Next steps</span></span>

<span data-ttu-id="2e5f1-186">Lär dig att skydda dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2e5f1-186">Learn how to protect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png
