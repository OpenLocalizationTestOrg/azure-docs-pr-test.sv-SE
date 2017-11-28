---
title: aaaHost flera platser med Azure Programgateway | Microsoft Docs
description: "Den här sidan finns instruktioner tooconfigure en befintlig Azure-program-gateway som värd för flera webbprogram på hello samma gateway med hello Azure-portalen."
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
ms.openlocfilehash: 2172aa2c80720f6f1ab7dd91745b44654bcaee00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a><span data-ttu-id="a42b0-103">Konfigurera en befintlig Programgateway som värd för flera webbprogram</span><span class="sxs-lookup"><span data-stu-id="a42b0-103">Configure an existing application gateway for hosting multiple web applications</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a42b0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a42b0-104">Azure portal</span></span>](application-gateway-create-multisite-portal.md)
> * [<span data-ttu-id="a42b0-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="a42b0-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

<span data-ttu-id="a42b0-106">Värd för flera plats kan du toodeploy mer än ett webbprogram på hello samma Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a42b0-106">Multiple site hosting allows you toodeploy more than one web application on hello same application gateway.</span></span> <span data-ttu-id="a42b0-107">Det är beroende av förekomsten av värdhuvudet i hello inkommande HTTP-begäran, toodetermine vilka lyssnare skulle ta emot trafik.</span><span class="sxs-lookup"><span data-stu-id="a42b0-107">It relies on presence of host header in hello incoming HTTP request, toodetermine which listener would receive traffic.</span></span> <span data-ttu-id="a42b0-108">hello lyssnare dirigerar sedan trafik tooappropriate serverdelspool som konfigurerats i hello regler definition av hello gateway.</span><span class="sxs-lookup"><span data-stu-id="a42b0-108">hello listener then directs traffic tooappropriate backend pool as configured in hello rules definition of hello gateway.</span></span> <span data-ttu-id="a42b0-109">I SSL aktiverat webbprogram beroende Programgateway hello Server Servernamnsindikation (SNI)-tillägget toochoose hello rätt lyssnare för hello webbtrafik.</span><span class="sxs-lookup"><span data-stu-id="a42b0-109">In SSL enabled web applications, application gateway relies on hello Server Name Indication (SNI) extension toochoose hello correct listener for hello web traffic.</span></span> <span data-ttu-id="a42b0-110">Ett vanligt användningsområde för värd för flera plats är tooload begäranden för annan webbplats domäner toodifferent backend-serverpooler.</span><span class="sxs-lookup"><span data-stu-id="a42b0-110">A common use for multiple site hosting is tooload balance requests for different web domains toodifferent back-end server pools.</span></span> <span data-ttu-id="a42b0-111">På liknande sätt hello flera underdomäner i hello samma rotdomänen kan också finnas på samma Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a42b0-111">Similarly multiple subdomains of hello same root domain could also be hosted on hello same application gateway.</span></span>

## <a name="scenario"></a><span data-ttu-id="a42b0-112">Scenario</span><span class="sxs-lookup"><span data-stu-id="a42b0-112">Scenario</span></span>

<span data-ttu-id="a42b0-113">I följande exempel hello, Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-113">In hello following example, application gateway is serving traffic for contoso.com and fabrikam.com with two back-end server pools: contoso server pool and fabrikam server pool.</span></span> <span data-ttu-id="a42b0-114">Liknande installationsprogrammet kan vara används toohost underdomäner som app.contoso.com och blog.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="a42b0-114">Similar setup could be used toohost subdomains like app.contoso.com and blog.contoso.com.</span></span>

![Multisite scenario][multisite]

## <a name="before-you-begin"></a><span data-ttu-id="a42b0-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a42b0-116">Before you begin</span></span>

<span data-ttu-id="a42b0-117">Det här scenariot lägger till stöd för flera platser tooan befintliga Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a42b0-117">This scenario adds multi-site support tooan existing application gateway.</span></span> <span data-ttu-id="a42b0-118">toocomplete det här scenariot kan en befintlig Programgateway måste toobe tillgängliga tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="a42b0-118">toocomplete this scenario, an existing application gateway needs toobe available tooconfigure.</span></span> <span data-ttu-id="a42b0-119">Besök [skapar en Programgateway med hello portal](application-gateway-create-gateway-portal.md) toolearn hur toocreate en basic-Programgateway hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-119">Visit [Create an application gateway by using hello portal](application-gateway-create-gateway-portal.md) toolearn how toocreate a basic application gateway in hello portal.</span></span>

<span data-ttu-id="a42b0-120">hello följande är hello steg behövs tooupdate hello Programgateway:</span><span class="sxs-lookup"><span data-stu-id="a42b0-120">hello following are hello steps needed tooupdate hello application gateway:</span></span>

1. <span data-ttu-id="a42b0-121">Skapa backend-pooler toouse för varje plats.</span><span class="sxs-lookup"><span data-stu-id="a42b0-121">Create back-end pools toouse for each site.</span></span>
2. <span data-ttu-id="a42b0-122">Skapa en lyssnare för varje plats Programgateway stöder.</span><span class="sxs-lookup"><span data-stu-id="a42b0-122">Create a listener for each site application gateway supports.</span></span>
3. <span data-ttu-id="a42b0-123">Skapa regler toomap varje lyssnare med hello lämpliga serverdel.</span><span class="sxs-lookup"><span data-stu-id="a42b0-123">Create rules toomap each listener with hello appropriate back-end.</span></span>

## <a name="requirements"></a><span data-ttu-id="a42b0-124">Krav</span><span class="sxs-lookup"><span data-stu-id="a42b0-124">Requirements</span></span>

* <span data-ttu-id="a42b0-125">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="a42b0-125">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="a42b0-126">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="a42b0-126">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span> <span data-ttu-id="a42b0-127">FQDN kan också användas.</span><span class="sxs-lookup"><span data-stu-id="a42b0-127">FQDN can also be used.</span></span>
* <span data-ttu-id="a42b0-128">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="a42b0-128">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="a42b0-129">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-129">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="a42b0-130">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="a42b0-130">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="a42b0-131">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="a42b0-131">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="a42b0-132">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="a42b0-132">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> <span data-ttu-id="a42b0-133">För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.</span><span class="sxs-lookup"><span data-stu-id="a42b0-133">For multi-site enabled application gateways, host name and SNI indicators are also added.</span></span>
* <span data-ttu-id="a42b0-134">**Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen, och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="a42b0-134">**Rule:** hello rule binds hello listener, hello back-end server pool, and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="a42b0-135">Regler bearbetas i hello ordning och trafik dirigeras via hello första regeln som överensstämmer oavsett särskilda egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a42b0-135">Rules are processed in hello order they are listed, and traffic will be directed via hello first rule that matches regardless of specificity.</span></span> <span data-ttu-id="a42b0-136">Till exempel om du har en regel med en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port hello regel med hello måste hello flera platser lyssnare anges innan hello regeln med grundläggande hello-lyssnare för hello flera platser regeln toofunction som förväntades.</span><span class="sxs-lookup"><span data-stu-id="a42b0-136">For example, if you have a rule using a basic listener and a rule using a multi-site listener both on hello same port, hello rule with hello multi-site listener must be listed before hello rule with hello basic listener in order for hello multi-site rule toofunction as expected.</span></span> 
* <span data-ttu-id="a42b0-137">**Certifikat:** varje lyssnare kräver ett unikt certifikat, i det här exemplet skapas 2-lyssnare för flera platser.</span><span class="sxs-lookup"><span data-stu-id="a42b0-137">**Certificates:** Each listener requires a unique certificate, in this example 2 listeners are created for multi-site.</span></span> <span data-ttu-id="a42b0-138">Två PFX-certifikat och hello lösenord för dem måste toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="a42b0-138">Two .pfx certificates and hello passwords for them need toobe created.</span></span>

## <a name="create-back-end-pools-for-each-site"></a><span data-ttu-id="a42b0-139">Skapa backend-pooler för varje plats</span><span class="sxs-lookup"><span data-stu-id="a42b0-139">Create back-end pools for each site</span></span>

<span data-ttu-id="a42b0-140">En backend-poolen för varje plats att programmet gateway stöder krävs, i det här fallet 2 är att skapa en för contoso11.com och en för fabrikam11.com.</span><span class="sxs-lookup"><span data-stu-id="a42b0-140">A back-end pool for each site that application gateway supports is needed, in this case 2 are be created, one for contoso11.com and one for fabrikam11.com.</span></span>

### <a name="step-1"></a><span data-ttu-id="a42b0-141">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a42b0-141">Step 1</span></span>

<span data-ttu-id="a42b0-142">Navigera tooan befintliga Programgateway i hello Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a42b0-142">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="a42b0-143">Välj **serverdelspooler** och på **Lägg till**</span><span class="sxs-lookup"><span data-stu-id="a42b0-143">Select **Backend pools** and click **Add**</span></span>

![Lägg till serverdelspooler][7]

### <a name="step-2"></a><span data-ttu-id="a42b0-145">Steg 2</span><span class="sxs-lookup"><span data-stu-id="a42b0-145">Step 2</span></span>

<span data-ttu-id="a42b0-146">Fyll i hello för hello backend-adresspool **pool1**, lägger till hello IP-adresser eller FQDN för hello backend-servrar och på **OK**</span><span class="sxs-lookup"><span data-stu-id="a42b0-146">Fill in hello information for hello back-end pool **pool1**, adding hello ip addresses or FQDNs for hello back-end servers and click **OK**</span></span>

![backend pool1 poolinställningarna][8]

### <a name="step-3"></a><span data-ttu-id="a42b0-148">Steg 3</span><span class="sxs-lookup"><span data-stu-id="a42b0-148">Step 3</span></span>

<span data-ttu-id="a42b0-149">På hello serverdelspooler bladet klickar du på **Lägg till** tooadd en ytterligare backend-adresspool **pool2**, lägger till hello IP-adresser eller FQDN för hello backend-servrar och på **OK**</span><span class="sxs-lookup"><span data-stu-id="a42b0-149">On hello backend-pools blade click **Add** tooadd an additional back-end pool **pool2**, adding hello ip addresses or FQDNS for hello back-end servers and click **OK**</span></span>

![backend pool2 poolinställningarna][9]

## <a name="create-listeners-for-each-back-end"></a><span data-ttu-id="a42b0-151">Skapa lyssnare för varje serverdel</span><span class="sxs-lookup"><span data-stu-id="a42b0-151">Create listeners for each back-end</span></span>

<span data-ttu-id="a42b0-152">Programgateway är beroende av HTTP 1.1 värden huvuden toohost mer än en webbplats på hello samma offentliga IP-adress och port.</span><span class="sxs-lookup"><span data-stu-id="a42b0-152">Application Gateway relies on HTTP 1.1 host headers toohost more than one website on hello same public IP address and port.</span></span> <span data-ttu-id="a42b0-153">grundläggande hello lyssnare har skapats i hello portal innehåller inte den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-153">hello basic listener created in hello portal does not contain this property.</span></span>

### <a name="step-1"></a><span data-ttu-id="a42b0-154">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a42b0-154">Step 1</span></span>

<span data-ttu-id="a42b0-155">Klicka på **lyssnare** hello befintliga Programgateway och klicka på **flera platser** tooadd hello första lyssnare.</span><span class="sxs-lookup"><span data-stu-id="a42b0-155">Click **Listeners** on hello existing application gateway and click **Multi-site** tooadd hello first listener.</span></span>

![lyssnare översikt bladet][1]

### <a name="step-2"></a><span data-ttu-id="a42b0-157">Steg 2</span><span class="sxs-lookup"><span data-stu-id="a42b0-157">Step 2</span></span>

<span data-ttu-id="a42b0-158">Fyll i hello information för hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="a42b0-158">Fill out hello information for hello listener.</span></span> <span data-ttu-id="a42b0-159">I det här exemplet SSL-avslutning har konfigurerats, skapa en ny frontend-port.</span><span class="sxs-lookup"><span data-stu-id="a42b0-159">In this example SSL termination is configured, create a new frontend port.</span></span> <span data-ttu-id="a42b0-160">Överför hello PFX-certifikat toobe används för SSL-avslutning.</span><span class="sxs-lookup"><span data-stu-id="a42b0-160">Upload hello .pfx certificate toobe used for SSL termination.</span></span> <span data-ttu-id="a42b0-161">hello enda skillnaden på det här bladet jämfört med toohello standard grundläggande lyssnare bladet är hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a42b0-161">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span>

![lyssnare egenskapsbladet][2]

### <a name="step-3"></a><span data-ttu-id="a42b0-163">Steg 3</span><span class="sxs-lookup"><span data-stu-id="a42b0-163">Step 3</span></span>

<span data-ttu-id="a42b0-164">Klicka på **flera platser** och skapa en annan lyssnare enligt beskrivningen i hello föregående steg för hello andra platsen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-164">Click **Multi-site** and create another listener as described in hello previous step for hello second site.</span></span> <span data-ttu-id="a42b0-165">Se till att toouse ett annat certifikat för andra hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="a42b0-165">Make sure toouse a different certificate for hello second listener.</span></span> <span data-ttu-id="a42b0-166">hello enda skillnaden på det här bladet jämfört med toohello standard grundläggande lyssnare bladet är hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="a42b0-166">hello only difference on this blade compared toohello standard basic listener blade is hello hostname.</span></span> <span data-ttu-id="a42b0-167">Fylla i hello information för hello-lyssnaren och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a42b0-167">Fill out hello information for hello listener and click **OK**.</span></span>

![lyssnare egenskapsbladet][3]

> [!NOTE]
> <span data-ttu-id="a42b0-169">Skapa lyssnare i hello Azure-portalen för Programgateway är en tidskrävande uppgift kan det ta några tid toocreate hello två lyssnare i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="a42b0-169">Creation of listeners in hello Azure portal for application gateway is a long running task, it may take some time toocreate hello two listeners in this scenario.</span></span> <span data-ttu-id="a42b0-170">När fullständig hello lyssnare visas i hello portal som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="a42b0-170">When complete hello listeners show in hello portal as seen in hello following image:</span></span>

![Översikt över lyssnare][4]

## <a name="create-rules-toomap-listeners-toobackend-pools"></a><span data-ttu-id="a42b0-172">Skapa regler toomap lyssnare toobackend pooler</span><span class="sxs-lookup"><span data-stu-id="a42b0-172">Create rules toomap listeners toobackend pools</span></span>

### <a name="step-1"></a><span data-ttu-id="a42b0-173">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a42b0-173">Step 1</span></span>

<span data-ttu-id="a42b0-174">Navigera tooan befintliga Programgateway i hello Azure-portalen (https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a42b0-174">Navigate tooan existing application gateway in hello Azure portal (https://portal.azure.com).</span></span> <span data-ttu-id="a42b0-175">Välj **regler** och välj hello befintliga standardregel **regel 1** och på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="a42b0-175">Select **Rules** and choose hello existing default rule **rule1** and click **Edit**.</span></span>

### <a name="step-2"></a><span data-ttu-id="a42b0-176">Steg 2</span><span class="sxs-lookup"><span data-stu-id="a42b0-176">Step 2</span></span>

<span data-ttu-id="a42b0-177">Fyll i hello regler bladet som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="a42b0-177">Fill out hello rules blade as seen in hello following image.</span></span> <span data-ttu-id="a42b0-178">Välja hello första lyssnare och första poolen och klicka på **spara** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="a42b0-178">Choosing hello first listener and first pool and clicking **Save** when complete.</span></span>

![Redigera en befintlig regel][6]

### <a name="step-3"></a><span data-ttu-id="a42b0-180">Steg 3</span><span class="sxs-lookup"><span data-stu-id="a42b0-180">Step 3</span></span>

<span data-ttu-id="a42b0-181">Klicka på **regel** toocreate hello andra regeln.</span><span class="sxs-lookup"><span data-stu-id="a42b0-181">Click **Basic rule** toocreate hello second rule.</span></span> <span data-ttu-id="a42b0-182">Fyll i formuläret hello med hello andra lyssnare och andra serverdelspool och klicka på **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="a42b0-182">Fill out hello form with hello second listener and second backend pool and click **OK** toosave.</span></span>

![Lägg till regel bladet][10]

<span data-ttu-id="a42b0-184">Det här scenariot har slutförts konfigurerar en Programgateway med befintliga med stöd för flera platser via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a42b0-184">This scenario completes configuring an existing application gateway with multi-site support through hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a42b0-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a42b0-185">Next steps</span></span>

<span data-ttu-id="a42b0-186">Lär dig hur tooprotect dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a42b0-186">Learn how tooprotect your websites with [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md)</span></span>

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
