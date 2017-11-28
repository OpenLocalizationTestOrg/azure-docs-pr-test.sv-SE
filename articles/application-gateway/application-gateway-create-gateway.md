---
title: aaaCreate, starta, eller ta bort en Programgateway | Microsoft Docs
description: "Den här sidan innehåller instruktioner toocreate, konfigurera, starta och ta bort en Azure Programgateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a><span data-ttu-id="00670-103">Skapa, starta eller ta bort en programgateway med PowerShell</span><span class="sxs-lookup"><span data-stu-id="00670-103">Create, start, or delete an application gateway with PowerShell</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="00670-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00670-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="00670-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="00670-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="00670-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="00670-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="00670-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="00670-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="00670-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="00670-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="00670-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="00670-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="00670-110">Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="00670-110">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="00670-111">Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="00670-111">Application Gateway provides many Application Delivery Controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="00670-112">toofind en fullständig lista över funktioner som stöds finns [Gateway Programöversikt](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="00670-112">toofind a complete list of supported features, visit [Application Gateway Overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="00670-113">Den här artikeln vägleder dig genom hello steg toocreate, konfigurera, starta och ta bort en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-113">This article walks you through hello steps toocreate, configure, start, and delete an application gateway.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="00670-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="00670-114">Before you begin</span></span>

1. <span data-ttu-id="00670-115">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="00670-115">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="00670-116">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="00670-116">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="00670-117">Om du har ett befintligt virtuellt nätverk, Välj ett befintligt tomma undernät eller skapa ett nytt undernät i ett befintligt virtuellt nätverk enbart för användning med hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-117">If you have an existing virtual network, either select an existing empty subnet or create a new subnet in your existing virtual network solely for use by hello application gateway.</span></span> <span data-ttu-id="00670-118">Du kan inte distribuera hello programmet gateway tooa annat virtuellt nätverk än hello resurser du avser toodeploy bakom hello Programgateway såvida inte vnet-peering används.</span><span class="sxs-lookup"><span data-stu-id="00670-118">You cannot deploy hello application gateway tooa different virtual network than hello resources you intend toodeploy behind hello application gateway unless vnet peering is used.</span></span> <span data-ttu-id="00670-119">toolearn finns mer [Vnet-Peering](../virtual-network/virtual-network-peering-overview.md)</span><span class="sxs-lookup"><span data-stu-id="00670-119">toolearn more visit [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)</span></span>
3. <span data-ttu-id="00670-120">Kontrollera att du har ett fungerande virtuellt nätverk med ett giltigt undernät.</span><span class="sxs-lookup"><span data-stu-id="00670-120">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="00670-121">Se till att inga virtuella datorer eller molndistributioner använder hello undernät.</span><span class="sxs-lookup"><span data-stu-id="00670-121">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="00670-122">hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="00670-122">hello application gateway must be by itself in a virtual network subnet.</span></span>
4. <span data-ttu-id="00670-123">hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="00670-123">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

## <a name="what-is-required-toocreate-an-application-gateway"></a><span data-ttu-id="00670-124">Vad är obligatoriska toocreate en Programgateway?</span><span class="sxs-lookup"><span data-stu-id="00670-124">What is required toocreate an application gateway?</span></span>

<span data-ttu-id="00670-125">När du använder hello `New-AzureApplicationGateway` kommandot toocreate hello Programgateway, ingen konfiguration är nu och hello nyligen skapade resursen är konfigurerade antingen med hjälp av XML- eller ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-125">When you use hello `New-AzureApplicationGateway` command toocreate hello application gateway, no configuration is set at this point and hello newly created resource are configured either by using XML or a configuration object.</span></span>

<span data-ttu-id="00670-126">hello-värden är:</span><span class="sxs-lookup"><span data-stu-id="00670-126">hello values are:</span></span>

* <span data-ttu-id="00670-127">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="00670-127">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="00670-128">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="00670-128">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="00670-129">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="00670-129">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="00670-130">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="00670-130">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="00670-131">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-131">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="00670-132">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="00670-132">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="00670-133">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="00670-133">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="00670-134">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="00670-134">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span>

## <a name="create-an-application-gateway"></a><span data-ttu-id="00670-135">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="00670-135">Create an application gateway</span></span>

<span data-ttu-id="00670-136">toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="00670-136">toocreate an application gateway:</span></span>

1. <span data-ttu-id="00670-137">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-137">Create an application gateway resource.</span></span>
2. <span data-ttu-id="00670-138">Skapa en XML-konfigurationsfil eller ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-138">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="00670-139">Bekräfta hello configuration toohello nyskapad programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-139">Commit hello configuration toohello newly created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="00670-140">Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="00670-140">If you need tooconfigure a custom probe for your application gateway, see [Create an application gateway with custom probes by using PowerShell](application-gateway-create-probe-classic-ps.md).</span></span> <span data-ttu-id="00670-141">Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).</span><span class="sxs-lookup"><span data-stu-id="00670-141">Check out [custom probes and health monitoring](application-gateway-probe-overview.md) for more information.</span></span>

![Exempel på ett scenario][scenario]

### <a name="create-an-application-gateway-resource"></a><span data-ttu-id="00670-143">Skapa en resurs för en programgateway</span><span class="sxs-lookup"><span data-stu-id="00670-143">Create an application gateway resource</span></span>

<span data-ttu-id="00670-144">Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="00670-144">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="00670-145">Fakturering för hello gatewayen startar inte nu.</span><span class="sxs-lookup"><span data-stu-id="00670-145">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="00670-146">Fakturering börjar i ett senare steg när hello gateway har startat.</span><span class="sxs-lookup"><span data-stu-id="00670-146">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="00670-147">hello följande exempel skapas en Programgateway med hjälp av ett virtuellt nätverk kallas ”testvnet1” och ett undernät som kallas ”Undernät 1”:</span><span class="sxs-lookup"><span data-stu-id="00670-147">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1":</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="00670-148">*Description*, *InstanceCount* och *GatewaySize* är valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="00670-148">*Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span>

<span data-ttu-id="00670-149">toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00670-149">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> <span data-ttu-id="00670-150">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="00670-150">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="00670-151">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="00670-151">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="00670-152">Du kan välja mellan Small, Medium eller Large.</span><span class="sxs-lookup"><span data-stu-id="00670-152">You can choose between Small, Medium and Large.</span></span>

<span data-ttu-id="00670-153">*Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="00670-153">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="00670-154">Dessa skapas när hello gateway är i hello körs.</span><span class="sxs-lookup"><span data-stu-id="00670-154">These are created once hello gateway is in hello running state.</span></span>

## <a name="configure-hello-application-gateway"></a><span data-ttu-id="00670-155">Konfigurera hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="00670-155">Configure hello application gateway</span></span>

<span data-ttu-id="00670-156">Du kan konfigurera hello Programgateway med hjälp av XML- eller ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-156">You can configure hello application gateway by using XML or a configuration object.</span></span>

### <a name="configure-hello-application-gateway-by-using-xml"></a><span data-ttu-id="00670-157">Konfigurera hello Programgateway genom att använda XML</span><span class="sxs-lookup"><span data-stu-id="00670-157">Configure hello application gateway by using XML</span></span>

<span data-ttu-id="00670-158">I följande exempel hello, och använda en XML-filen tooconfigure alla inställningar för Programgateway och bekräfta dem toohello programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-158">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

#### <a name="step-1"></a><span data-ttu-id="00670-159">Steg 1</span><span class="sxs-lookup"><span data-stu-id="00670-159">Step 1</span></span>

<span data-ttu-id="00670-160">Kopiera följande text tooNotepad hello.</span><span class="sxs-lookup"><span data-stu-id="00670-160">Copy hello following text tooNotepad.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="00670-161">Redigera hello värden mellan hello parenteser för hello konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-161">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="00670-162">Spara hello-filen med filnamnstillägget .xml.</span><span class="sxs-lookup"><span data-stu-id="00670-162">Save hello file with extension .xml.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="00670-163">hello protokollet objektet Http eller Https är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="00670-163">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="00670-164">hello som följande exempel visar hur toouse en konfiguration filen tooset in hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-164">hello following example shows how toouse a configuration file tooset up hello application gateway.</span></span> <span data-ttu-id="00670-165">hello exempel belastningen balanserar HTTP-trafik på offentlig port 80 och skickar nätverkstrafik tooback avslutande port 80 mellan två IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="00670-165">hello example load balances HTTP traffic on public port 80 and sends network traffic tooback-end port 80 between two IP addresses.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a><span data-ttu-id="00670-166">Steg 2</span><span class="sxs-lookup"><span data-stu-id="00670-166">Step 2</span></span>

<span data-ttu-id="00670-167">Ange därefter hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="00670-167">Next, set hello application gateway.</span></span> <span data-ttu-id="00670-168">Använd hello `Set-AzureApplicationGatewayConfig` med en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="00670-168">Use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a><span data-ttu-id="00670-169">Konfigurera hello Programgateway med hjälp av ett konfigurationsobjekt</span><span class="sxs-lookup"><span data-stu-id="00670-169">Configure hello application gateway by using a configuration object</span></span>

<span data-ttu-id="00670-170">hello som följande exempel visar hur tooconfigure hello Programgateway med hjälp av konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-170">hello following example shows how tooconfigure hello application gateway by using configuration objects.</span></span> <span data-ttu-id="00670-171">Alla konfigurationsobjekt måste konfigureras individuellt och sedan läggs tooan programobjektet gateway-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="00670-171">All configuration items must be configured individually and then added tooan application gateway configuration object.</span></span> <span data-ttu-id="00670-172">Efter att hello konfigurationsobjekt kan du använda hello `Set-AzureApplicationGateway` kommandot toocommit hello configuration toohello skapat programmet gatewayresursen.</span><span class="sxs-lookup"><span data-stu-id="00670-172">After creating hello configuration object, you use hello `Set-AzureApplicationGateway` command toocommit hello configuration toohello previously created application gateway resource.</span></span>

> [!NOTE]
> <span data-ttu-id="00670-173">Innan du tilldelar ett värde tooeach konfigurationsobjekt, måste du toodeclare vilken typ av objekt PowerShell använder för lagring.</span><span class="sxs-lookup"><span data-stu-id="00670-173">Before assigning a value tooeach configuration object, you need toodeclare what kind of object PowerShell uses for storage.</span></span> <span data-ttu-id="00670-174">hello första rad toocreate hello enskilda objekt som definierar vad `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` används.</span><span class="sxs-lookup"><span data-stu-id="00670-174">hello first line toocreate hello individual items defines what `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` are used.</span></span>

#### <a name="step-1"></a><span data-ttu-id="00670-175">Steg 1</span><span class="sxs-lookup"><span data-stu-id="00670-175">Step 1</span></span>

<span data-ttu-id="00670-176">Skapa alla enskilda konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="00670-176">Create all individual configuration items.</span></span>

<span data-ttu-id="00670-177">Skapa hello frontend IP som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="00670-177">Create hello front-end IP as shown in hello following example.</span></span>

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

<span data-ttu-id="00670-178">Skapa hello frontend-port som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="00670-178">Create hello front-end port as shown in hello following example.</span></span>

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

<span data-ttu-id="00670-179">Skapa hello backend-servern i serverpoolen.</span><span class="sxs-lookup"><span data-stu-id="00670-179">Create hello back-end server pool.</span></span>

<span data-ttu-id="00670-180">Definiera hello IP-adresser som har lagts till toohello backend-serverpoolen enligt hello nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="00670-180">Define hello IP addresses that are added toohello back-end server pool as shown in hello next example.</span></span>

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

<span data-ttu-id="00670-181">Använda hello $server tooadd hello värden toohello backend-adresspool objekt ($pool).</span><span class="sxs-lookup"><span data-stu-id="00670-181">Use hello $server object tooadd hello values toohello back-end pool object ($pool).</span></span>

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

<span data-ttu-id="00670-182">Skapa hello backend-server pool inställning.</span><span class="sxs-lookup"><span data-stu-id="00670-182">Create hello back-end server pool setting.</span></span>

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

<span data-ttu-id="00670-183">Skapa hello lyssnare.</span><span class="sxs-lookup"><span data-stu-id="00670-183">Create hello listener.</span></span>

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

<span data-ttu-id="00670-184">Skapa hello regel.</span><span class="sxs-lookup"><span data-stu-id="00670-184">Create hello rule.</span></span>

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a><span data-ttu-id="00670-185">Steg 2</span><span class="sxs-lookup"><span data-stu-id="00670-185">Step 2</span></span>

<span data-ttu-id="00670-186">Tilldela alla enskilda objekt tooan programmet gateway konfiguration konfigurationsobjektet ($appgwconfig).</span><span class="sxs-lookup"><span data-stu-id="00670-186">Assign all individual configuration items tooan application gateway configuration object ($appgwconfig).</span></span>

<span data-ttu-id="00670-187">Lägg till hello frontend IP toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="00670-187">Add hello front-end IP toohello configuration.</span></span>

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

<span data-ttu-id="00670-188">Lägg till hello frontend toohello portkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="00670-188">Add hello front-end port toohello configuration.</span></span>

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
<span data-ttu-id="00670-189">Lägg till hello backend-adresspool toohello serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="00670-189">Add hello back-end server pool toohello configuration.</span></span>

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

<span data-ttu-id="00670-190">Lägg till hello backend-adresspool inställningen toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="00670-190">Add hello back-end pool setting toohello configuration.</span></span>

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

<span data-ttu-id="00670-191">Lägg till hello lyssnare toohello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="00670-191">Add hello listener toohello configuration.</span></span>

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

<span data-ttu-id="00670-192">Lägg till hello toohello regelkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="00670-192">Add hello rule toohello configuration.</span></span>

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a><span data-ttu-id="00670-193">Steg 3</span><span class="sxs-lookup"><span data-stu-id="00670-193">Step 3</span></span>
<span data-ttu-id="00670-194">Genomför hello configuration-objekt toohello gateway programresursen med hjälp av `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="00670-194">Commit hello configuration object toohello application gateway resource by using `Set-AzureApplicationGatewayConfig`.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a><span data-ttu-id="00670-195">Starta hello gateway</span><span class="sxs-lookup"><span data-stu-id="00670-195">Start hello gateway</span></span>

<span data-ttu-id="00670-196">När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-196">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="00670-197">Fakturering för en Programgateway startar när hello gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="00670-197">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="00670-198">Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toofinish too15 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="00670-198">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="00670-199">Verifiera hello gatewayens status</span><span class="sxs-lookup"><span data-stu-id="00670-199">Verify hello gateway status</span></span>

<span data-ttu-id="00670-200">Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello status för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-200">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="00670-201">Om `Start-AzureApplicationGateway` lyckades hello föregående steg, *tillstånd* ska köras och *Vip* och *DnsName* ska ha giltiga poster.</span><span class="sxs-lookup"><span data-stu-id="00670-201">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *Vip* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="00670-202">hello följande exempel visas en Programgateway som är tillgängliga, körs och är redo tootake trafik avsedd för `http://<generated-dns-name>.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="00670-202">hello following example shows an application gateway that is up, running, and ready tootake traffic destined for `http://<generated-dns-name>.cloudapp.net`.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-hello-application-gateway"></a><span data-ttu-id="00670-203">Ta bort hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="00670-203">Delete hello application gateway</span></span>

<span data-ttu-id="00670-204">toodelete hello Programgateway:</span><span class="sxs-lookup"><span data-stu-id="00670-204">toodelete hello application gateway:</span></span>

1. <span data-ttu-id="00670-205">Använd hello `Stop-AzureApplicationGateway` cmdlet toostop hello gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-205">Use hello `Stop-AzureApplicationGateway` cmdlet toostop hello gateway.</span></span>
2. <span data-ttu-id="00670-206">Använd hello `Remove-AzureApplicationGateway` cmdlet tooremove hello gateway.</span><span class="sxs-lookup"><span data-stu-id="00670-206">Use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello gateway.</span></span>
3. <span data-ttu-id="00670-207">Kontrollera att hello-gateway har tagits bort med hjälp av hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00670-207">Verify that hello gateway has been removed by using hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="00670-208">hello följande exempel visar hello `Stop-AzureApplicationGateway` cmdlet på hello första rad, följt av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="00670-208">hello following example shows hello `Stop-AzureApplicationGateway` cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

<span data-ttu-id="00670-209">När hello Programgateway är i ett stoppat tillstånd, använder hello `Remove-AzureApplicationGateway` cmdlet tooremove hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00670-209">Once hello application gateway is in a stopped state, use hello `Remove-AzureApplicationGateway` cmdlet tooremove hello service.</span></span>

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

<span data-ttu-id="00670-210">tooverify som hello tjänsten har tagits bort kan du använda hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00670-210">tooverify that hello service has been removed, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> <span data-ttu-id="00670-211">Det här steget är inte obligatoriskt.</span><span class="sxs-lookup"><span data-stu-id="00670-211">This step is not required.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a><span data-ttu-id="00670-212">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="00670-212">Next steps</span></span>

<span data-ttu-id="00670-213">Om du vill tooconfigure SSL-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="00670-213">If you want tooconfigure SSL offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="00670-214">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare, se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="00670-214">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="00670-215">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="00670-215">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="00670-216">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="00670-216">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="00670-217">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="00670-217">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
