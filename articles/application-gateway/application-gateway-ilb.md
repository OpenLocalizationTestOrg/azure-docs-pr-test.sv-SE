---
title: "aaaUsing Azure Programgateway med interna belastningsutjämnare | Microsoft Docs"
description: "Den här sidan finns instruktioner tooconfigure en Programgateway i Azure med en intern belastningsutjämnade slutpunkt"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="54543-103">Skapa en Application Gateway med en intern belastningsutjämnare (ILB)</span><span class="sxs-lookup"><span data-stu-id="54543-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="54543-104">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="54543-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="54543-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="54543-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="54543-106">Programgateway kan konfigureras med en internetuppkopplad virtuell IP-adress eller med en intern slutpunkt exponeras inte toohello internet, även kallat inre belastningen belastningsutjämnare (ILB) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="54543-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed toohello internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="54543-107">Konfigurera hello gateway med en ILB är användbart för toointernet interna line-of-business-program som inte visas.</span><span class="sxs-lookup"><span data-stu-id="54543-107">Configuring hello gateway with an ILB is useful for internal line-of-business applications not exposed toointernet.</span></span> <span data-ttu-id="54543-108">Det är också användbart för servicenivåerna inom en flernivåapp som placeras i en gräns som exponeras inte toointernet för säkerhet, men kräver fortfarande resursallokering belastningsdistribution, varaktighet för sessionen eller SSL-avslutning.</span><span class="sxs-lookup"><span data-stu-id="54543-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed toointernet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="54543-109">Den här artikeln vägleder dig genom hello steg tooconfigure en Programgateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="54543-109">This article walks you through hello steps tooconfigure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="54543-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="54543-110">Before you begin</span></span>

1. <span data-ttu-id="54543-111">Installera senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="54543-111">Install latest version of hello Azure PowerShell cmdlets using hello Web Platform Installer.</span></span> <span data-ttu-id="54543-112">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="54543-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="54543-113">Kontrollera att du har ett virtuellt nätverk fungerar med giltig nätmask.</span><span class="sxs-lookup"><span data-stu-id="54543-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="54543-114">Kontrollera att du har backend-servrar i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="54543-114">Verify that you have backend servers either in hello virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="54543-115">toocreate en Programgateway utföra hello följa stegen i ordning hello.</span><span class="sxs-lookup"><span data-stu-id="54543-115">toocreate an application gateway, perform hello following steps in hello order listed.</span></span> 

1. [<span data-ttu-id="54543-116">Skapa en Programgateway</span><span class="sxs-lookup"><span data-stu-id="54543-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="54543-117">Konfigurera hello-gateway</span><span class="sxs-lookup"><span data-stu-id="54543-117">Configure hello gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="54543-118">Ange hello gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="54543-118">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="54543-119">Starta hello gateway</span><span class="sxs-lookup"><span data-stu-id="54543-119">Start hello gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="54543-120">Kontrollera hello gateway</span><span class="sxs-lookup"><span data-stu-id="54543-120">Verify hello gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="54543-121">Skapa en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="54543-121">Create an application gateway:</span></span>

<span data-ttu-id="54543-122">**toocreate hello gateway**, använda hello `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="54543-122">**toocreate hello gateway**, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="54543-123">Observera att faktureringen för hello gatewayen inte startar nu.</span><span class="sxs-lookup"><span data-stu-id="54543-123">Note that billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="54543-124">Fakturering börjar i ett senare steg när hello gateway har startat.</span><span class="sxs-lookup"><span data-stu-id="54543-124">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

<span data-ttu-id="54543-125">**toovalidate** att hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="54543-125">**toovalidate** that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="54543-126">I exemplet hello *beskrivning*, *InstanceCount*, och *GatewaySize* är valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="54543-126">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="54543-127">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="54543-127">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="54543-128">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="54543-128">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="54543-129">Små och stora är andra tillgängliga värden.</span><span class="sxs-lookup"><span data-stu-id="54543-129">Small and Large are other available values.</span></span> <span data-ttu-id="54543-130">*VIP* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="54543-130">*Vip* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="54543-131">Dessa skapas när hello gateway är i hello körs.</span><span class="sxs-lookup"><span data-stu-id="54543-131">These are created once hello gateway is in hello running state.</span></span> 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a><span data-ttu-id="54543-132">Konfigurera hello-gateway</span><span class="sxs-lookup"><span data-stu-id="54543-132">Configure hello gateway</span></span>
<span data-ttu-id="54543-133">En gateway programkonfigurationen består av flera värden.</span><span class="sxs-lookup"><span data-stu-id="54543-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="54543-134">hello-värden kan knytas samman tooconstruct hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="54543-134">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="54543-135">hello-värden är:</span><span class="sxs-lookup"><span data-stu-id="54543-135">hello values are:</span></span>

* <span data-ttu-id="54543-136">**Backend-serverpoolen:** hello lista över IP-adresser på hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="54543-136">**Backend server pool:** hello list of IP addresses of hello backend servers.</span></span> <span data-ttu-id="54543-137">hello IP-adresser som anges antingen ska tillhöra toohello VNet subnet eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="54543-137">hello IP addresses listed should either belong toohello VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="54543-138">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookie-baserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="54543-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="54543-139">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="54543-139">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="54543-140">**Klientdelsport:** den här porten är hello offentliga öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="54543-140">**Frontend Port:** This port is hello public port opened on hello application gateway.</span></span> <span data-ttu-id="54543-141">Trafik träffar den här porten och hämtar omdirigerade tooone på hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="54543-141">Traffic hits this port, and then gets redirected tooone of hello backend servers.</span></span>
* <span data-ttu-id="54543-142">**Lyssnare:** hello-lyssnare har en klientdelsport, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="54543-142">**Listener:** hello listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="54543-143">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-servern poolen hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="54543-143">**Rule:** hello rule binds hello listener and hello backend server pool and defines which backend server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="54543-144">För närvarande endast hello *grundläggande* regeln stöds.</span><span class="sxs-lookup"><span data-stu-id="54543-144">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="54543-145">Hej *grundläggande* regeln är resursallokering belastningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="54543-145">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="54543-146">Du kan skapa din konfiguration genom att skapa ett konfigurationsobjekt eller genom att använda en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="54543-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="54543-147">tooconstruct konfigurationen med hjälp av en XML-konfigurationsfilen, Använd hello exempel nedan.</span><span class="sxs-lookup"><span data-stu-id="54543-147">tooconstruct your configuration by using a configuration XML file, use hello sample below.</span></span>

<span data-ttu-id="54543-148">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="54543-148">Note hello following:</span></span>

* <span data-ttu-id="54543-149">Hej *FrontendIPConfigurations* element beskriver hello ILB uppgifter för att konfigurera Application Gateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="54543-149">hello *FrontendIPConfigurations* element describes hello ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="54543-150">hello klientdel IP *typen* ska anges too'Private'</span><span class="sxs-lookup"><span data-stu-id="54543-150">hello Frontend IP *Type* should be set too'Private'</span></span>
* <span data-ttu-id="54543-151">Hej *StaticIPAddress* ska anges toohello önskad intern IP-adress på vilken hello gateway tar emot trafik.</span><span class="sxs-lookup"><span data-stu-id="54543-151">hello *StaticIPAddress* should be set toohello desired internal IP on which hello gateway receives traffic.</span></span> <span data-ttu-id="54543-152">Observera att hello *StaticIPAddress* element är valfria.</span><span class="sxs-lookup"><span data-stu-id="54543-152">Note that hello *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="54543-153">Om du inte kan en tillgänglig intern IP-adress från hello distribueras undernät väljs.</span><span class="sxs-lookup"><span data-stu-id="54543-153">If not set, an available internal IP from hello deployed subnet is chosen.</span></span> 
* <span data-ttu-id="54543-154">Hej värdet för hello *namn* element som anges i *FrontendIPConfiguration* ska användas i hello HTTPListener *FrontendIP* elementet toorefer toohello FrontendIPConfiguration.</span><span class="sxs-lookup"><span data-stu-id="54543-154">hello value of hello *Name* element specified in *FrontendIPConfiguration* should be used in hello HTTPListener's *FrontendIP* element toorefer toohello FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="54543-155">**Konfiguration av XML-exempel**</span><span class="sxs-lookup"><span data-stu-id="54543-155">**Configuration XML sample**</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="54543-156">Ange hello gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="54543-156">Set hello gateway configuration</span></span>
<span data-ttu-id="54543-157">Sedan ställer du hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="54543-157">Next, you'll set hello application gateway.</span></span> <span data-ttu-id="54543-158">Du kan använda hello `Set-AzureApplicationGatewayConfig` cmdlet med ett konfigurationsobjekt eller med en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="54543-158">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a><span data-ttu-id="54543-159">Starta hello gateway</span><span class="sxs-lookup"><span data-stu-id="54543-159">Start hello gateway</span></span>

<span data-ttu-id="54543-160">När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span><span class="sxs-lookup"><span data-stu-id="54543-160">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="54543-161">Fakturering för en Programgateway startar när hello gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="54543-161">Billing for an application gateway begins after hello gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="54543-162">Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toocomplete too15 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="54543-162">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toocomplete.</span></span> 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="54543-163">Verifiera hello gatewayens status</span><span class="sxs-lookup"><span data-stu-id="54543-163">Verify hello gateway status</span></span>

<span data-ttu-id="54543-164">Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello statusen för gateway.</span><span class="sxs-lookup"><span data-stu-id="54543-164">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of gateway.</span></span> <span data-ttu-id="54543-165">Om `Start-AzureApplicationGateway` lyckades hello föregående steg, hello-tillståndet ska vara *kör*, hello Vip och DNS-namn ska ha giltiga poster.</span><span class="sxs-lookup"><span data-stu-id="54543-165">If `Start-AzureApplicationGateway` succeeded in hello previous step, hello State should be *Running*, and hello Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="54543-166">Det här exemplet visar hello-cmdleten på hello första rad, följt av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="54543-166">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span> <span data-ttu-id="54543-167">I det här exemplet hello gateway körs och är redo tootake trafik.</span><span class="sxs-lookup"><span data-stu-id="54543-167">In this sample, hello gateway is running, and is ready tootake traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="54543-168">hello Programgateway konfigureras tooaccept trafik i hello konfigurerad ILB-slutpunkten för 10.0.0.10 i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="54543-168">hello application gateway is configured tooaccept traffic at hello configured ILB endpoint of 10.0.0.10 in this example.</span></span>

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="54543-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54543-169">Next steps</span></span>
<span data-ttu-id="54543-170">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="54543-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="54543-171">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="54543-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="54543-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="54543-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

