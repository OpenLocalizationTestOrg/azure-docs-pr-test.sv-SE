---
title: "Med intern belastningsutjämnare Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att konfigurera en Gateway för Azure-program med en intern belastningsutjämnade slutpunkt"
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
ms.openlocfilehash: d6f3af61934c8c645be1f2c6b4c056fc7ee2e3aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a><span data-ttu-id="caf72-103">Skapa en Application Gateway med en intern belastningsutjämnare (ILB)</span><span class="sxs-lookup"><span data-stu-id="caf72-103">Create an Application Gateway with an Internal Load Balancer (ILB)</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="caf72-104">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="caf72-104">Azure Classic PowerShell</span></span>](application-gateway-ilb.md)
> * [<span data-ttu-id="caf72-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="caf72-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ilb-arm.md)

<span data-ttu-id="caf72-106">Programgateway kan konfigureras med en internetuppkopplad virtuell IP-adress eller en intern slutpunkt exponeras inte till internet, även kallat inre belastningen belastningsutjämnare (ILB) slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="caf72-106">Application Gateway can be configured with an internet facing virtual IP or with an internal end-point not exposed to the internet, also known as Internal Load Balancer (ILB) endpoint.</span></span> <span data-ttu-id="caf72-107">Konfigurera gateway med en ILB är användbart för interna av branschspecifika program inte utsätts för internet.</span><span class="sxs-lookup"><span data-stu-id="caf72-107">Configuring the gateway with an ILB is useful for internal line-of-business applications not exposed to internet.</span></span> <span data-ttu-id="caf72-108">Det är också användbart för servicenivåerna inom en flernivåapp som placeras i en säkerhetsgräns inte utsätts för internet, men kräver fortfarande resursallokering belastningsdistribution, varaktighet för sessionen eller SSL-avslutning.</span><span class="sxs-lookup"><span data-stu-id="caf72-108">It's also useful for services/tiers within a multi-tier application, which sits in a security boundary not exposed to internet, but still require round robin load distribution, session stickiness, or SSL termination.</span></span> <span data-ttu-id="caf72-109">Den här artikeln beskriver steg för steg hur du konfigurerar en programgateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="caf72-109">This article walks you through the steps to configure an application gateway with an ILB.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="caf72-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="caf72-110">Before you begin</span></span>

1. <span data-ttu-id="caf72-111">Installera senaste versionen av Azure PowerShell-cmdlets med hjälp av Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="caf72-111">Install latest version of the Azure PowerShell cmdlets using the Web Platform Installer.</span></span> <span data-ttu-id="caf72-112">Du kan hämta och installera den senaste versionen från den **Windows PowerShell** avsnitt i den [hämtningssidan](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="caf72-112">You can download and install the latest version from the **Windows PowerShell** section of the [Download page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="caf72-113">Kontrollera att du har ett virtuellt nätverk fungerar med giltig nätmask.</span><span class="sxs-lookup"><span data-stu-id="caf72-113">Verify that you have a working virtual network with valid subnet.</span></span>
3. <span data-ttu-id="caf72-114">Kontrollera att du har backend-servrar i det virtuella nätverket eller med en offentlig IP-adress/VIP tilldelad.</span><span class="sxs-lookup"><span data-stu-id="caf72-114">Verify that you have backend servers either in the virtual network, or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="caf72-115">Utför följande steg för att skapa en Programgateway i angiven ordning.</span><span class="sxs-lookup"><span data-stu-id="caf72-115">To create an application gateway, perform the following steps in the order listed.</span></span> 

1. [<span data-ttu-id="caf72-116">Skapa en Programgateway</span><span class="sxs-lookup"><span data-stu-id="caf72-116">Create an application gateway</span></span>](#create-a-new-application-gateway)
2. [<span data-ttu-id="caf72-117">Konfigurera gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-117">Configure the gateway</span></span>](#configure-the-gateway)
3. [<span data-ttu-id="caf72-118">Ange gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="caf72-118">Set the gateway configuration</span></span>](#set-the-gateway-configuration)
4. [<span data-ttu-id="caf72-119">Starta gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-119">Start the gateway</span></span>](#start-the-gateway)
5. [<span data-ttu-id="caf72-120">Kontrollera gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-120">Verify the gateway</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="caf72-121">Skapa en Programgateway:</span><span class="sxs-lookup"><span data-stu-id="caf72-121">Create an application gateway:</span></span>

<span data-ttu-id="caf72-122">**Att skapa gatewayen**, använda den `New-AzureApplicationGateway` cmdlet, där du ersätter värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="caf72-122">**To create the gateway**, use the `New-AzureApplicationGateway` cmdlet, replacing the values with your own.</span></span> <span data-ttu-id="caf72-123">Observera att faktureringen för gatewayen inte startar i det här läget.</span><span class="sxs-lookup"><span data-stu-id="caf72-123">Note that billing for the gateway does not start at this point.</span></span> <span data-ttu-id="caf72-124">Faktureringen börjar i ett senare skede när gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="caf72-124">Billing begins in a later step, when the gateway is successfully started.</span></span>

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

<span data-ttu-id="caf72-125">**Validera** att gatewayen har skapats kan du använda den `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="caf72-125">**To validate** that the gateway was created, you can use the `Get-AzureApplicationGateway` cmdlet.</span></span> 

<span data-ttu-id="caf72-126">I det här exemplet *beskrivning*, *InstanceCount*, och *GatewaySize* är valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="caf72-126">In the sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="caf72-127">Standardvärdet för *InstanceCount* är 2, och det högsta värdet är 10.</span><span class="sxs-lookup"><span data-stu-id="caf72-127">The default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="caf72-128">Standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="caf72-128">The default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="caf72-129">Små och stora är andra tillgängliga värden.</span><span class="sxs-lookup"><span data-stu-id="caf72-129">Small and Large are other available values.</span></span> <span data-ttu-id="caf72-130">*VIP* och *DnsName* visas som tomt eftersom gatewayen inte har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="caf72-130">*Vip* and *DnsName* are shown as blank because the gateway has not started yet.</span></span> <span data-ttu-id="caf72-131">De skapas när gatewayen är i körläge.</span><span class="sxs-lookup"><span data-stu-id="caf72-131">These are created once the gateway is in the running state.</span></span> 

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

## <a name="configure-the-gateway"></a><span data-ttu-id="caf72-132">Konfigurera gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-132">Configure the gateway</span></span>
<span data-ttu-id="caf72-133">En gateway programkonfigurationen består av flera värden.</span><span class="sxs-lookup"><span data-stu-id="caf72-133">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="caf72-134">Värdena kan knytas samman för att konstruera konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="caf72-134">The values can be tied together to construct the configuration.</span></span>

<span data-ttu-id="caf72-135">Värdena är:</span><span class="sxs-lookup"><span data-stu-id="caf72-135">The values are:</span></span>

* <span data-ttu-id="caf72-136">**Backend-serverpoolen:** listan över IP-adresser på backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="caf72-136">**Backend server pool:** The list of IP addresses of the backend servers.</span></span> <span data-ttu-id="caf72-137">IP-adresser som anges antingen ska tillhöra VNet subnet eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="caf72-137">The IP addresses listed should either belong to the VNet subnet, or should be a public IP/VIP.</span></span> 
* <span data-ttu-id="caf72-138">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookie-baserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="caf72-138">**Backend server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="caf72-139">Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.</span><span class="sxs-lookup"><span data-stu-id="caf72-139">These settings are tied to a pool and are applied to all servers within the pool.</span></span>
* <span data-ttu-id="caf72-140">**Klientdelsport:** den här porten är den offentliga öppnas på programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="caf72-140">**Frontend Port:** This port is the public port opened on the application gateway.</span></span> <span data-ttu-id="caf72-141">Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.</span><span class="sxs-lookup"><span data-stu-id="caf72-141">Traffic hits this port, and then gets redirected to one of the backend servers.</span></span>
* <span data-ttu-id="caf72-142">**Lyssnare:** lyssnaren har en klientdelsport, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och namnet på SSL (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="caf72-142">**Listener:** The listener has a frontend port, a protocol (Http or Https, these are case-sensitive), and the SSL certificate name (if configuring SSL offload).</span></span> 
* <span data-ttu-id="caf72-143">**Regel:** regeln Binder lyssnaren och backend-serverpoolen och definierar vilka backend-serverpoolen trafiken ska dirigeras till när den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="caf72-143">**Rule:** The rule binds the listener and the backend server pool and defines which backend server pool the traffic should be directed to when it hits a particular listener.</span></span> <span data-ttu-id="caf72-144">För närvarande stöds endast regeln *basic*.</span><span class="sxs-lookup"><span data-stu-id="caf72-144">Currently, only the *basic* rule is supported.</span></span> <span data-ttu-id="caf72-145">Regeln *basic* använder belastningsutjämning med resursallokering.</span><span class="sxs-lookup"><span data-stu-id="caf72-145">The *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="caf72-146">Du kan skapa din konfiguration genom att skapa ett konfigurationsobjekt eller genom att använda en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="caf72-146">You can construct your configuration either by creating a configuration object, or by using a configuration XML file.</span></span> <span data-ttu-id="caf72-147">Använd exemplet nedan för att skapa konfigurationen med hjälp av en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="caf72-147">To construct your configuration by using a configuration XML file, use the sample below.</span></span>

<span data-ttu-id="caf72-148">Observera följande:</span><span class="sxs-lookup"><span data-stu-id="caf72-148">Note the following:</span></span>

* <span data-ttu-id="caf72-149">Den *FrontendIPConfigurations* element beskriver vad ILB som är relevanta för att konfigurera Application Gateway med en ILB.</span><span class="sxs-lookup"><span data-stu-id="caf72-149">The *FrontendIPConfigurations* element describes the ILB details relevant for configuring Application Gateway with an ILB.</span></span> 
* <span data-ttu-id="caf72-150">Frontend IP *typen* ska anges till ”privat”</span><span class="sxs-lookup"><span data-stu-id="caf72-150">The Frontend IP *Type* should be set to 'Private'</span></span>
* <span data-ttu-id="caf72-151">Den *StaticIPAddress* ska anges till den önskade intern IP-adress som gatewayen tar emot trafik.</span><span class="sxs-lookup"><span data-stu-id="caf72-151">The *StaticIPAddress* should be set to the desired internal IP on which the gateway receives traffic.</span></span> <span data-ttu-id="caf72-152">Observera att den *StaticIPAddress* element är valfria.</span><span class="sxs-lookup"><span data-stu-id="caf72-152">Note that the *StaticIPAddress* element is optional.</span></span> <span data-ttu-id="caf72-153">Om du inte kan en tillgänglig intern IP-adress från undernätet som är distribuerade väljs.</span><span class="sxs-lookup"><span data-stu-id="caf72-153">If not set, an available internal IP from the deployed subnet is chosen.</span></span> 
* <span data-ttu-id="caf72-154">Värdet för den *namn* element som anges i *FrontendIPConfiguration* ska användas i HTTPListener *FrontendIP* element att referera till FrontendIPConfiguration.</span><span class="sxs-lookup"><span data-stu-id="caf72-154">The value of the *Name* element specified in *FrontendIPConfiguration* should be used in the HTTPListener's *FrontendIP* element to refer to the FrontendIPConfiguration.</span></span>
  
  <span data-ttu-id="caf72-155">**Konfiguration av XML-exempel**</span><span class="sxs-lookup"><span data-stu-id="caf72-155">**Configuration XML sample**</span></span>
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


## <a name="set-the-gateway-configuration"></a><span data-ttu-id="caf72-156">Ange gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="caf72-156">Set the gateway configuration</span></span>
<span data-ttu-id="caf72-157">Sedan ställer du programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="caf72-157">Next, you'll set the application gateway.</span></span> <span data-ttu-id="caf72-158">Du kan använda den `Set-AzureApplicationGatewayConfig` cmdlet med ett konfigurationsobjekt eller med en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="caf72-158">You can use the `Set-AzureApplicationGatewayConfig` cmdlet with a configuration object, or with a configuration XML file.</span></span> 

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

## <a name="start-the-gateway"></a><span data-ttu-id="caf72-159">Starta gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-159">Start the gateway</span></span>

<span data-ttu-id="caf72-160">När gatewayen har konfigurerats kan du använda cmdleten `Start-AzureApplicationGateway` för att starta gatewayen.</span><span class="sxs-lookup"><span data-stu-id="caf72-160">Once the gateway has been configured, use the `Start-AzureApplicationGateway` cmdlet to start the gateway.</span></span> <span data-ttu-id="caf72-161">Faktureringen för en programgateway börjar när gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="caf72-161">Billing for an application gateway begins after the gateway has been successfully started.</span></span> 

> [!NOTE]
> <span data-ttu-id="caf72-162">Den `Start-AzureApplicationGateway` cmdlet kan ta upp till 15-20 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="caf72-162">The `Start-AzureApplicationGateway` cmdlet might take up to 15-20 minutes to complete.</span></span> 
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

## <a name="verify-the-gateway-status"></a><span data-ttu-id="caf72-163">Kontrollera statusen för gatewayen</span><span class="sxs-lookup"><span data-stu-id="caf72-163">Verify the gateway status</span></span>

<span data-ttu-id="caf72-164">Använd den `Get-AzureApplicationGateway` för att kontrollera status för gateway.</span><span class="sxs-lookup"><span data-stu-id="caf72-164">Use the `Get-AzureApplicationGateway` cmdlet to check the status of gateway.</span></span> <span data-ttu-id="caf72-165">Om `Start-AzureApplicationGateway` lyckades i föregående steg, tillståndet ska vara *kör*, och Vip och DNS-namn ska ha giltiga poster.</span><span class="sxs-lookup"><span data-stu-id="caf72-165">If `Start-AzureApplicationGateway` succeeded in the previous step, the State should be *Running*, and the Vip and DnsName should have valid entries.</span></span> <span data-ttu-id="caf72-166">Det här exemplet visar cmdleten på den första raden, följt av utdata.</span><span class="sxs-lookup"><span data-stu-id="caf72-166">This sample shows the cmdlet on the first line, followed by the output.</span></span> <span data-ttu-id="caf72-167">I det här exemplet gatewayen körs och är redo att ta trafik.</span><span class="sxs-lookup"><span data-stu-id="caf72-167">In this sample, the gateway is running, and is ready to take traffic.</span></span> 

> [!NOTE]
> <span data-ttu-id="caf72-168">Programgatewayen är konfigurerad att ta emot trafik på den konfigurerade ILB-slutpunkten för 10.0.0.10 i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="caf72-168">The application gateway is configured to accept traffic at the configured ILB endpoint of 10.0.0.10 in this example.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="caf72-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="caf72-169">Next steps</span></span>
<span data-ttu-id="caf72-170">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="caf72-170">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="caf72-171">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="caf72-171">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="caf72-172">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="caf72-172">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

