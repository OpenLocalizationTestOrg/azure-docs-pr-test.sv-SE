---
title: "aaaCreate klassiska en anpassad avsökningsåtgärd - Azure Application Gateway - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avsökning för Programgateway med PowerShell i hello klassiska distributionsmodellen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="ef6ab-103">Skapa en anpassad avsökningsåtgärd för Azure-Programgateway (klassiskt) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef6ab-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef6ab-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ef6ab-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="ef6ab-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ef6ab-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="ef6ab-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ef6ab-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="ef6ab-107">Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway med PowerShell i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-107">In this article, you add a custom probe tooan existing application gateway with PowerShell.</span></span> <span data-ttu-id="ef6ab-108">Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på hello standardwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on hello default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef6ab-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ab-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ef6ab-110">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-110">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="ef6ab-111">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-111">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="ef6ab-112">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ab-112">Learn how too[perform these steps using hello Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="ef6ab-113">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="ef6ab-113">Create an application gateway</span></span>

<span data-ttu-id="ef6ab-114">toocreate en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="ef6ab-114">toocreate an application gateway:</span></span>

1. <span data-ttu-id="ef6ab-115">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="ef6ab-116">Skapa en XML-konfigurationsfil eller ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="ef6ab-117">Bekräfta hello configuration toohello nyskapad programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-117">Commit hello configuration toohello newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="ef6ab-118">Skapa ett program gateway med en anpassad avsökningsåtgärd</span><span class="sxs-lookup"><span data-stu-id="ef6ab-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="ef6ab-119">Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-119">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="ef6ab-120">Fakturering för hello gatewayen startar inte nu.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-120">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="ef6ab-121">Fakturering börjar i ett senare steg när hello gateway har startat.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-121">Billing begins in a later step, when hello gateway is successfully started.</span></span>

<span data-ttu-id="ef6ab-122">hello följande exempel skapas en Programgateway med hjälp av ett virtuellt nätverk kallas ”testvnet1” och ett undernät som kallas ”Undernät 1”.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-122">hello following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="ef6ab-123">toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-123">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="ef6ab-124">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-124">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="ef6ab-125">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-125">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="ef6ab-126">Du kan välja mellan små, medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="ef6ab-127">*Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-127">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="ef6ab-128">Dessa värden skapas när hello gateway är i hello körs.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-128">These values are created once hello gateway is in hello running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="ef6ab-129">Konfigurera en Programgateway genom att använda XML</span><span class="sxs-lookup"><span data-stu-id="ef6ab-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="ef6ab-130">I följande exempel hello, och använda en XML-filen tooconfigure alla inställningar för Programgateway och bekräfta dem toohello programresursen gateway.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-130">In hello following example, you use an XML file tooconfigure all application gateway settings and commit them toohello application gateway resource.</span></span>  

<span data-ttu-id="ef6ab-131">Kopiera följande text tooNotepad hello.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-131">Copy hello following text tooNotepad.</span></span>

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

<span data-ttu-id="ef6ab-132">Redigera hello värden mellan hello parenteser för hello konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-132">Edit hello values between hello parentheses for hello configuration items.</span></span> <span data-ttu-id="ef6ab-133">Spara hello-filen med filnamnstillägget .xml.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-133">Save hello file with extension .xml.</span></span>

<span data-ttu-id="ef6ab-134">hello följande exempel visas hur toouse en konfiguration filen tooset in hello programmet gateway tooload balansera HTTP-trafik på offentlig port 80 och skicka nätverkstrafik tooback avslutande port 80 mellan två IP-adresser med hjälp av en anpassad avsökning.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-134">hello following example shows how toouse a configuration file tooset up hello application gateway tooload balance HTTP traffic on public port 80 and send network traffic tooback-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef6ab-135">hello protokollet objektet Http eller Https är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-135">hello protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="ef6ab-136">Ett nytt konfigurationsobjekt \<avsökning\> läggs anpassade tooconfigure-avsökningar.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-136">A new configuration item \<Probe\> is added tooconfigure custom probes.</span></span>

<span data-ttu-id="ef6ab-137">hello konfigurationsparametrar är:</span><span class="sxs-lookup"><span data-stu-id="ef6ab-137">hello configuration parameters are:</span></span>

|<span data-ttu-id="ef6ab-138">Parameter</span><span class="sxs-lookup"><span data-stu-id="ef6ab-138">Parameter</span></span>|<span data-ttu-id="ef6ab-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ef6ab-139">Description</span></span>|
|---|---|
|<span data-ttu-id="ef6ab-140">**Namn**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-140">**Name**</span></span> |<span data-ttu-id="ef6ab-141">Referensnamn för anpassade avsökningen.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="ef6ab-142">* **Protokollet**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-142">* **Protocol**</span></span> | <span data-ttu-id="ef6ab-143">Protokoll som används (möjliga värden är HTTP eller HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ef6ab-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="ef6ab-144">**Värden** och **sökväg**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-144">**Host** and **Path**</span></span> | <span data-ttu-id="ef6ab-145">Fullständig URL-sökväg som anropas av hello gateway toodetermine hello programhälsan av hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-145">Complete URL path that is invoked by hello application gateway toodetermine hello health of hello instance.</span></span> <span data-ttu-id="ef6ab-146">Om du har en webbplats http://contoso.com/ hello anpassad avsökningsåtgärd kan konfigureras för kontrollerar ”http://contoso.com/path/custompath.htm” för avsökningen toohave ett lyckat HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-146">For example, if you have a website http://contoso.com/, then hello custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks toohave a successful HTTP response.</span></span>|
| <span data-ttu-id="ef6ab-147">**Intervall**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-147">**Interval**</span></span> | <span data-ttu-id="ef6ab-148">Konfigurerar hello avsökningen intervall kontroller i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-148">Configures hello probe interval checks in seconds.</span></span>|
| <span data-ttu-id="ef6ab-149">**Timeout**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-149">**Timeout**</span></span> | <span data-ttu-id="ef6ab-150">Definierar hello avsökningen timeout-värdet för en kontroll för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-150">Defines hello probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="ef6ab-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="ef6ab-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="ef6ab-152">Hej antalet misslyckade HTTP-svar krävs tooflag hello backend-instans som *ohälsosamt*.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-152">hello number of failed HTTP responses needed tooflag hello back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="ef6ab-153">Hej avsökningsnamnet refereras i hello \<BackendHttpSettings\> configuration tooassign vilka backend-adresspool använder anpassad avsökningsåtgärd inställningar.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-153">hello probe name is referenced in hello \<BackendHttpSettings\> configuration tooassign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a><span data-ttu-id="ef6ab-154">Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway</span><span class="sxs-lookup"><span data-stu-id="ef6ab-154">Add a custom probe tooan existing application gateway</span></span>

<span data-ttu-id="ef6ab-155">Ändra hello aktuella konfigurationen för en Programgateway kräver tre steg: hämta hello aktuella XML-konfigurationsfilen, ändra toohave en anpassad avsökning och konfigurera hello Programgateway med hello nya XML-inställningar.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-155">Changing hello current configuration of an application gateway requires three steps: Get hello current XML configuration file, modify toohave a custom probe, and configure hello application gateway with hello new XML settings.</span></span>

1. <span data-ttu-id="ef6ab-156">Hämta hello XML-filen genom att använda `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-156">Get hello XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="ef6ab-157">Den här cmdleten export hello configuration XML toobe ändrade tooadd någon avsökningsinställning.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-157">This cmdlet exports hello configuration XML toobe modified tooadd a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. <span data-ttu-id="ef6ab-158">Öppna hello XML-filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-158">Open hello XML file in a text editor.</span></span> <span data-ttu-id="ef6ab-159">Lägg till en `<probe>` avsnittet efter `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-159">Add a `<probe>` section after `<frontendport>`.</span></span>

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  <span data-ttu-id="ef6ab-160">Lägg till hello avsökningsnamnet i hello backendHttpSettings avsnittet hello XML, som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="ef6ab-160">In hello backendHttpSettings section of hello XML, add hello probe name as shown in hello following example:</span></span>

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  <span data-ttu-id="ef6ab-161">Spara hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-161">Save hello XML file.</span></span>

1. <span data-ttu-id="ef6ab-162">Uppdatera hello programkonfigurationen gateway med hello nya XML-filen med hjälp av `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-162">Update hello application gateway configuration with hello new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="ef6ab-163">Denna cmdlet uppdaterar din Programgateway med hello ny konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ef6ab-163">This cmdlet updates your application gateway with hello new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a><span data-ttu-id="ef6ab-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef6ab-164">Next steps</span></span>

<span data-ttu-id="ef6ab-165">Om du vill tooconfigure Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ab-165">If you want tooconfigure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="ef6ab-166">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare, se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="ef6ab-166">If you want tooconfigure an application gateway toouse with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

