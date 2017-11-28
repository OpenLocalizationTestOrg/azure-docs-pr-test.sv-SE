---
title: "Skapa en anpassad avsökningsåtgärd - Azure Application Gateway - PowerShell klassisk | Microsoft Docs"
description: "Lär dig hur du skapar en anpassad avsökningsåtgärd för Programgateway med PowerShell i den klassiska distributionsmodellen"
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
ms.openlocfilehash: bf190741b10c10e885d927ad21a9f2b25107943f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a><span data-ttu-id="86510-103">Skapa en anpassad avsökningsåtgärd för Azure-Programgateway (klassiskt) med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="86510-103">Create a custom probe for Azure Application Gateway (classic) by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="86510-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="86510-104">Azure portal</span></span>](application-gateway-create-probe-portal.md)
> * [<span data-ttu-id="86510-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="86510-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-probe-ps.md)
> * [<span data-ttu-id="86510-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="86510-106">Azure Classic PowerShell</span></span>](application-gateway-create-probe-classic-ps.md)

<span data-ttu-id="86510-107">I den här artikeln, lägger du till en anpassad avsökning en befintlig Programgateway med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="86510-107">In this article, you add a custom probe to an existing application gateway with PowerShell.</span></span> <span data-ttu-id="86510-108">Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på standardwebbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="86510-108">Custom probes are useful for applications that have a specific health check page or for applications that do not provide a successful response on the default web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86510-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="86510-109">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="86510-110">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="86510-110">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="86510-111">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="86510-111">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="86510-112">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="86510-112">Learn how to [perform these steps using the Resource Manager model](application-gateway-create-probe-ps.md).</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a><span data-ttu-id="86510-113">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="86510-113">Create an application gateway</span></span>

<span data-ttu-id="86510-114">Så här skapar du en programgateway:</span><span class="sxs-lookup"><span data-stu-id="86510-114">To create an application gateway:</span></span>

1. <span data-ttu-id="86510-115">Skapa en resurs för en programgateway.</span><span class="sxs-lookup"><span data-stu-id="86510-115">Create an application gateway resource.</span></span>
2. <span data-ttu-id="86510-116">Skapa en XML-konfigurationsfil eller ett konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="86510-116">Create a configuration XML file or a configuration object.</span></span>
3. <span data-ttu-id="86510-117">Bekräfta konfigurationen för den nyligen skapade programgatewayresursen.</span><span class="sxs-lookup"><span data-stu-id="86510-117">Commit the configuration to the newly created application gateway resource.</span></span>

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a><span data-ttu-id="86510-118">Skapa ett program gateway med en anpassad avsökningsåtgärd</span><span class="sxs-lookup"><span data-stu-id="86510-118">Create an application gateway resource with a custom probe</span></span>

<span data-ttu-id="86510-119">Du skapar en gateway genom att köra cmdleten `New-AzureApplicationGateway`, där du ersätter värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="86510-119">To create the gateway, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="86510-120">Faktureringen för gatewayen startar inte i det här läget.</span><span class="sxs-lookup"><span data-stu-id="86510-120">Billing for the gateway does not start at this point.</span></span> <span data-ttu-id="86510-121">Faktureringen börjar i ett senare skede när gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="86510-121">Billing begins in a later step, when the gateway is successfully started.</span></span>

<span data-ttu-id="86510-122">I följande exempel skapar vi en programgateway genom att använda ett virtuellt nätverk med namnet ”testvnet1” och undernätet ”subnet-1”.</span><span class="sxs-lookup"><span data-stu-id="86510-122">The following example creates an application gateway by using a virtual network called "testvnet1" and a subnet called "subnet-1".</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="86510-123">Du kontrollerar att gatewayen har skapats genom att köra cmdleten `Get-AzureApplicationGateway`.</span><span class="sxs-lookup"><span data-stu-id="86510-123">To validate that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> <span data-ttu-id="86510-124">Standardvärdet för *InstanceCount* är 2, och det högsta värdet är 10.</span><span class="sxs-lookup"><span data-stu-id="86510-124">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="86510-125">Standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="86510-125">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="86510-126">Du kan välja mellan små, medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="86510-126">You can choose between Small, Medium, and Large.</span></span>
> 
> 

<span data-ttu-id="86510-127">*VirtualIPs* och *DnsName* är tomma eftersom gatewayen inte har startat än.</span><span class="sxs-lookup"><span data-stu-id="86510-127">*VirtualIPs* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="86510-128">Dessa värden skapas när den är i körläge.</span><span class="sxs-lookup"><span data-stu-id="86510-128">These values are created once the gateway is in the running state.</span></span>

### <a name="configure-an-application-gateway-by-using-xml"></a><span data-ttu-id="86510-129">Konfigurera en Programgateway genom att använda XML</span><span class="sxs-lookup"><span data-stu-id="86510-129">Configure an application gateway by using XML</span></span>

<span data-ttu-id="86510-130">I följande exempel använder vi en XML-fil för att konfigurera alla inställningar för programgatewayen och för att skicka dem till programgatewayresursen.</span><span class="sxs-lookup"><span data-stu-id="86510-130">In the following example, you use an XML file to configure all application gateway settings and commit them to the application gateway resource.</span></span>  

<span data-ttu-id="86510-131">Kopiera följande text till Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="86510-131">Copy the following text to Notepad.</span></span>

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

<span data-ttu-id="86510-132">Redigera värdena mellan parenteserna för konfigurationsobjekten.</span><span class="sxs-lookup"><span data-stu-id="86510-132">Edit the values between the parentheses for the configuration items.</span></span> <span data-ttu-id="86510-133">Spara filen med filnamnstillägget .xml.</span><span class="sxs-lookup"><span data-stu-id="86510-133">Save the file with extension .xml.</span></span>

<span data-ttu-id="86510-134">I följande exempel visas hur du använder en konfigurationsfil för att ställa in Programgateway belastningsutjämna HTTP-trafik på offentlig port 80 och skicka trafik till backend-port 80 mellan två IP-adresser med hjälp av en anpassad avsökning.</span><span class="sxs-lookup"><span data-stu-id="86510-134">The following example shows how to use a configuration file to set up the application gateway to load balance HTTP traffic on public port 80 and send network traffic to back-end port 80 between two IP addresses by using a custom probe.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86510-135">Protokollobjekten Http och Https är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="86510-135">The protocol item Http or Https is case-sensitive.</span></span>

<span data-ttu-id="86510-136">Ett nytt konfigurationsobjekt \<avsökning\> har lagts till i Konfigurera anpassade avsökningar.</span><span class="sxs-lookup"><span data-stu-id="86510-136">A new configuration item \<Probe\> is added to configure custom probes.</span></span>

<span data-ttu-id="86510-137">Konfigurationsparametrarna är:</span><span class="sxs-lookup"><span data-stu-id="86510-137">The configuration parameters are:</span></span>

|<span data-ttu-id="86510-138">Parameter</span><span class="sxs-lookup"><span data-stu-id="86510-138">Parameter</span></span>|<span data-ttu-id="86510-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="86510-139">Description</span></span>|
|---|---|
|<span data-ttu-id="86510-140">**Namn**</span><span class="sxs-lookup"><span data-stu-id="86510-140">**Name**</span></span> |<span data-ttu-id="86510-141">Referensnamn för anpassade avsökningen.</span><span class="sxs-lookup"><span data-stu-id="86510-141">Reference name for custom probe.</span></span> |
<span data-ttu-id="86510-142">* **Protokollet**</span><span class="sxs-lookup"><span data-stu-id="86510-142">* **Protocol**</span></span> | <span data-ttu-id="86510-143">Protokoll som används (möjliga värden är HTTP eller HTTPS).</span><span class="sxs-lookup"><span data-stu-id="86510-143">Protocol used (possible values are HTTP or HTTPS).</span></span>|
| <span data-ttu-id="86510-144">**Värden** och **sökväg**</span><span class="sxs-lookup"><span data-stu-id="86510-144">**Host** and **Path**</span></span> | <span data-ttu-id="86510-145">Fullständiga URL-sökväg som anropas av Programgateway fastställa hälsotillståndet för instansen.</span><span class="sxs-lookup"><span data-stu-id="86510-145">Complete URL path that is invoked by the application gateway to determine the health of the instance.</span></span> <span data-ttu-id="86510-146">Till exempel om du har en webbplats http://contoso.com/ kan sedan den anpassa avsökningen konfigureras för ”http://contoso.com/path/custompath.htm” för avsökning kontrollerar att ett lyckat HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="86510-146">For example, if you have a website http://contoso.com/, then the custom probe can be configured for "http://contoso.com/path/custompath.htm" for probe checks to have a successful HTTP response.</span></span>|
| <span data-ttu-id="86510-147">**Intervall**</span><span class="sxs-lookup"><span data-stu-id="86510-147">**Interval**</span></span> | <span data-ttu-id="86510-148">Konfigurerar kontroller för avsökning intervall i sekunder.</span><span class="sxs-lookup"><span data-stu-id="86510-148">Configures the probe interval checks in seconds.</span></span>|
| <span data-ttu-id="86510-149">**Timeout**</span><span class="sxs-lookup"><span data-stu-id="86510-149">**Timeout**</span></span> | <span data-ttu-id="86510-150">Definierar avsökningen timeout-värdet för en kontroll för HTTP-svar.</span><span class="sxs-lookup"><span data-stu-id="86510-150">Defines the probe time-out for an HTTP response check.</span></span>|
| <span data-ttu-id="86510-151">**UnhealthyThreshold**</span><span class="sxs-lookup"><span data-stu-id="86510-151">**UnhealthyThreshold**</span></span> | <span data-ttu-id="86510-152">Antal misslyckade HTTP-svar som behövs för att flaggan backend-instans som *ohälsosamt*.</span><span class="sxs-lookup"><span data-stu-id="86510-152">The number of failed HTTP responses needed to flag the back-end instance as *unhealthy*.</span></span>|

<span data-ttu-id="86510-153">Avsökningsnamnet refererar till den \<BackendHttpSettings\> konfigurationen att tilldela vilka backend-adresspoolen använder anpassad avsökningsåtgärd inställningar.</span><span class="sxs-lookup"><span data-stu-id="86510-153">The probe name is referenced in the \<BackendHttpSettings\> configuration to assign which back-end pool uses custom probe settings.</span></span>

## <a name="add-a-custom-probe-to-an-existing-application-gateway"></a><span data-ttu-id="86510-154">Lägg till en anpassad avsökning i en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="86510-154">Add a custom probe to an existing application gateway</span></span>

<span data-ttu-id="86510-155">Ändra den aktuella konfigurationen av en Programgateway kräver tre steg: hämta den aktuella XML-konfigurationsfilen, ändra om du vill att en anpassad avsökning och konfigurera programgatewayen med de nya inställningarna för XML.</span><span class="sxs-lookup"><span data-stu-id="86510-155">Changing the current configuration of an application gateway requires three steps: Get the current XML configuration file, modify to have a custom probe, and configure the application gateway with the new XML settings.</span></span>

1. <span data-ttu-id="86510-156">Hämta XML-filen genom att använda `Get-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="86510-156">Get the XML file by using `Get-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="86510-157">Denna cmdlet exporterar konfigurations-XML som ska ändras för att lägga till någon avsökningsinställning.</span><span class="sxs-lookup"><span data-stu-id="86510-157">This cmdlet exports the configuration XML to be modified to add a probe setting.</span></span>

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
  ```

1. <span data-ttu-id="86510-158">Öppna XML-filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="86510-158">Open the XML file in a text editor.</span></span> <span data-ttu-id="86510-159">Lägg till en `<probe>` avsnittet efter `<frontendport>`.</span><span class="sxs-lookup"><span data-stu-id="86510-159">Add a `<probe>` section after `<frontendport>`.</span></span>

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

  <span data-ttu-id="86510-160">Lägg till avsökningsnamnet i avsnittet backendHttpSettings i XML-koden som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="86510-160">In the backendHttpSettings section of the XML, add the probe name as shown in the following example:</span></span>

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

  <span data-ttu-id="86510-161">Spara XML-filen.</span><span class="sxs-lookup"><span data-stu-id="86510-161">Save the XML file.</span></span>

1. <span data-ttu-id="86510-162">Uppdatera gateway programkonfigurationen med den nya XML-filen genom att använda `Set-AzureApplicationGatewayConfig`.</span><span class="sxs-lookup"><span data-stu-id="86510-162">Update the application gateway configuration with the new XML file by using `Set-AzureApplicationGatewayConfig`.</span></span> <span data-ttu-id="86510-163">Denna cmdlet uppdaterar din Programgateway med den nya konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="86510-163">This cmdlet updates your application gateway with the new configuration.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a><span data-ttu-id="86510-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86510-164">Next steps</span></span>

<span data-ttu-id="86510-165">Om du vill konfigurera Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="86510-165">If you want to configure Secure Sockets Layer (SSL) offload, see [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="86510-166">Om du vill konfigurera en programgateway för användning med en intern belastningsutjämnare läser du [Skapa en programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="86510-166">If you want to configure an application gateway to use with an internal load balancer, see [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

