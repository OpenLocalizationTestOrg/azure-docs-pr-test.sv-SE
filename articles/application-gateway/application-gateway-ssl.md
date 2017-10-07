---
title: aaaConfigure SSL-avlastning klassiska - Azure Application Gateway - PowerShell | Microsoft Docs
description: "Den här artikeln innehåller instruktioner som toocreate en Programgateway med SSL avlasta med hjälp av hello Azure klassiska distributionsmodellen."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 5cb128015747ed4b71802cf751c80b60634601a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a><span data-ttu-id="94957-103">Konfigurera en Programgateway för SSL-avlastning med hjälp av hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="94957-103">Configure an application gateway for SSL offload by using hello classic deployment model</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="94957-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94957-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="94957-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="94957-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="94957-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="94957-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="94957-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="94957-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="94957-108">Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="94957-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="94957-109">SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="94957-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="94957-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="94957-110">Before you begin</span></span>

1. <span data-ttu-id="94957-111">Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="94957-111">Install hello latest version of hello Azure PowerShell cmdlets by using hello Web Platform Installer.</span></span> <span data-ttu-id="94957-112">Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="94957-112">You can download and install hello latest version from hello **Windows PowerShell** section of hello [Downloads page](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="94957-113">Kontrollera att du har ett fungerande virtuellt nätverk med ett giltigt undernät.</span><span class="sxs-lookup"><span data-stu-id="94957-113">Verify that you have a working virtual network with a valid subnet.</span></span> <span data-ttu-id="94957-114">Se till att inga virtuella datorer eller molndistributioner använder hello undernät.</span><span class="sxs-lookup"><span data-stu-id="94957-114">Make sure that no virtual machines or cloud deployments are using hello subnet.</span></span> <span data-ttu-id="94957-115">hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="94957-115">hello application gateway must be by itself in a virtual network subnet.</span></span>
3. <span data-ttu-id="94957-116">hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="94957-116">hello servers that you configure toouse hello application gateway must exist or have their endpoints created either in hello virtual network or with a public IP/VIP assigned.</span></span>

<span data-ttu-id="94957-117">tooconfigure SSL avlasta på en Programgateway, hello följa stegen i ordning hello:</span><span class="sxs-lookup"><span data-stu-id="94957-117">tooconfigure SSL offload on an application gateway, do hello following steps in hello order listed:</span></span>

1. [<span data-ttu-id="94957-118">Skapa en Programgateway</span><span class="sxs-lookup"><span data-stu-id="94957-118">Create an application gateway</span></span>](#create-an-application-gateway)
2. [<span data-ttu-id="94957-119">Överför SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="94957-119">Upload SSL certificates</span></span>](#upload-ssl-certificates)
3. [<span data-ttu-id="94957-120">Konfigurera hello-gateway</span><span class="sxs-lookup"><span data-stu-id="94957-120">Configure hello gateway</span></span>](#configure-the-gateway)
4. [<span data-ttu-id="94957-121">Ange hello gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="94957-121">Set hello gateway configuration</span></span>](#set-the-gateway-configuration)
5. [<span data-ttu-id="94957-122">Starta hello gateway</span><span class="sxs-lookup"><span data-stu-id="94957-122">Start hello gateway</span></span>](#start-the-gateway)
6. [<span data-ttu-id="94957-123">Verifiera hello gatewayens status</span><span class="sxs-lookup"><span data-stu-id="94957-123">Verify hello gateway status</span></span>](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a><span data-ttu-id="94957-124">Skapa en programgateway</span><span class="sxs-lookup"><span data-stu-id="94957-124">Create an application gateway</span></span>

<span data-ttu-id="94957-125">Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="94957-125">toocreate hello gateway, use hello `New-AzureApplicationGateway` cmdlet, replacing hello values with your own.</span></span> <span data-ttu-id="94957-126">Fakturering för hello gatewayen startar inte nu.</span><span class="sxs-lookup"><span data-stu-id="94957-126">Billing for hello gateway does not start at this point.</span></span> <span data-ttu-id="94957-127">Fakturering börjar i ett senare steg när hello gateway har startat.</span><span class="sxs-lookup"><span data-stu-id="94957-127">Billing begins in a later step, when hello gateway is successfully started.</span></span>

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

<span data-ttu-id="94957-128">toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94957-128">toovalidate that hello gateway was created, you can use hello `Get-AzureApplicationGateway` cmdlet.</span></span>

<span data-ttu-id="94957-129">I exemplet hello *beskrivning*, *InstanceCount*, och *GatewaySize* är valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="94957-129">In hello sample, *Description*, *InstanceCount*, and *GatewaySize* are optional parameters.</span></span> <span data-ttu-id="94957-130">Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10.</span><span class="sxs-lookup"><span data-stu-id="94957-130">hello default value for *InstanceCount* is 2, with a maximum value of 10.</span></span> <span data-ttu-id="94957-131">Hej standardvärdet för *GatewaySize* är Medium.</span><span class="sxs-lookup"><span data-stu-id="94957-131">hello default value for *GatewaySize* is Medium.</span></span> <span data-ttu-id="94957-132">Små och stora är andra tillgängliga värden.</span><span class="sxs-lookup"><span data-stu-id="94957-132">Small and Large are other available values.</span></span> <span data-ttu-id="94957-133">*Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu.</span><span class="sxs-lookup"><span data-stu-id="94957-133">*VirtualIPs* and *DnsName* are shown as blank because hello gateway has not started yet.</span></span> <span data-ttu-id="94957-134">Dessa värden skapas när hello gateway är i hello körs.</span><span class="sxs-lookup"><span data-stu-id="94957-134">These values are created once hello gateway is in hello running state.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a><span data-ttu-id="94957-135">Överför SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="94957-135">Upload SSL certificates</span></span>

<span data-ttu-id="94957-136">Använd `Add-AzureApplicationGatewaySslCertificate` tooupload hello servercertifikat i *pfx* format toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="94957-136">Use `Add-AzureApplicationGatewaySslCertificate` tooupload hello server certificate in *pfx* format toohello application gateway.</span></span> <span data-ttu-id="94957-137">hello certifikatets namn är ett namn som valts av användaren och måste vara unika inom hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="94957-137">hello certificate name is a user-chosen name and must be unique within hello application gateway.</span></span> <span data-ttu-id="94957-138">Det här certifikatet är refererad tooby det här namnet i alla certifikat hanteringsåtgärder på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="94957-138">This certificate is referred tooby this name in all certificate management operations on hello application gateway.</span></span>

<span data-ttu-id="94957-139">Det här exemplet nedan visar hello cmdleten Ersätt hello värden i hello exemplet med dina egna.</span><span class="sxs-lookup"><span data-stu-id="94957-139">This following sample shows hello cmdlet, replace hello values in hello sample with your own.</span></span>

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

<span data-ttu-id="94957-140">Därefter Validera hello certifikat överföringen.</span><span class="sxs-lookup"><span data-stu-id="94957-140">Next, validate hello certificate upload.</span></span> <span data-ttu-id="94957-141">Använd hello `Get-AzureApplicationGatewayCertificate` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="94957-141">Use hello `Get-AzureApplicationGatewayCertificate` cmdlet.</span></span>

<span data-ttu-id="94957-142">Det här exemplet visar hello-cmdleten på hello första rad, följt av hello utdata.</span><span class="sxs-lookup"><span data-stu-id="94957-142">This sample shows hello cmdlet on hello first line, followed by hello output.</span></span>

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> <span data-ttu-id="94957-143">hello certifikatlösenord har toobe mellan 4 too12 tecken, bokstäver eller siffror.</span><span class="sxs-lookup"><span data-stu-id="94957-143">hello certificate password has toobe between 4 too12 characters, letters, or numbers.</span></span> <span data-ttu-id="94957-144">Specialtecken godkänns inte.</span><span class="sxs-lookup"><span data-stu-id="94957-144">Special characters are not accepted.</span></span>

## <a name="configure-hello-gateway"></a><span data-ttu-id="94957-145">Konfigurera hello-gateway</span><span class="sxs-lookup"><span data-stu-id="94957-145">Configure hello gateway</span></span>

<span data-ttu-id="94957-146">En gateway programkonfigurationen består av flera värden.</span><span class="sxs-lookup"><span data-stu-id="94957-146">An application gateway configuration consists of multiple values.</span></span> <span data-ttu-id="94957-147">hello-värden kan knytas samman tooconstruct hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="94957-147">hello values can be tied together tooconstruct hello configuration.</span></span>

<span data-ttu-id="94957-148">hello-värden är:</span><span class="sxs-lookup"><span data-stu-id="94957-148">hello values are:</span></span>

* <span data-ttu-id="94957-149">**Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="94957-149">**Back-end server pool:** hello list of IP addresses of hello back-end servers.</span></span> <span data-ttu-id="94957-150">hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.</span><span class="sxs-lookup"><span data-stu-id="94957-150">hello IP addresses listed should either belong toohello virtual network subnet or should be a public IP/VIP.</span></span>
* <span data-ttu-id="94957-151">**Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet.</span><span class="sxs-lookup"><span data-stu-id="94957-151">**Back-end server pool settings:** Every pool has settings like port, protocol, and cookie-based affinity.</span></span> <span data-ttu-id="94957-152">De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.</span><span class="sxs-lookup"><span data-stu-id="94957-152">These settings are tied tooa pool and are applied tooall servers within hello pool.</span></span>
* <span data-ttu-id="94957-153">**Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="94957-153">**Front-end port:** This port is hello public port that is opened on hello application gateway.</span></span> <span data-ttu-id="94957-154">Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="94957-154">Traffic hits this port, and then gets redirected tooone of hello back-end servers.</span></span>
* <span data-ttu-id="94957-155">**Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).</span><span class="sxs-lookup"><span data-stu-id="94957-155">**Listener:** hello listener has a front-end port, a protocol (Http or Https, these values are case-sensitive), and hello SSL certificate name (if configuring SSL offload).</span></span>
* <span data-ttu-id="94957-156">**Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.</span><span class="sxs-lookup"><span data-stu-id="94957-156">**Rule:** hello rule binds hello listener and hello back-end server pool and defines which back-end server pool hello traffic should be directed toowhen it hits a particular listener.</span></span> <span data-ttu-id="94957-157">För närvarande endast hello *grundläggande* regeln stöds.</span><span class="sxs-lookup"><span data-stu-id="94957-157">Currently, only hello *basic* rule is supported.</span></span> <span data-ttu-id="94957-158">Hej *grundläggande* regeln är resursallokering belastningsdistribution.</span><span class="sxs-lookup"><span data-stu-id="94957-158">hello *basic* rule is round-robin load distribution.</span></span>

<span data-ttu-id="94957-159">**Information om ytterligare konfiguration**</span><span class="sxs-lookup"><span data-stu-id="94957-159">**Additional configuration notes**</span></span>

<span data-ttu-id="94957-160">Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt).</span><span class="sxs-lookup"><span data-stu-id="94957-160">For SSL certificates configuration, hello protocol in **HttpListener** should change too*Https* (case sensitive).</span></span> <span data-ttu-id="94957-161">Hej **SslCert** läggs elementet för**HttpListener** med hello värdet toohello samma namn som används i hello överföringen av föregående avsnitt för SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="94957-161">hello **SslCert** element is added too**HttpListener** with hello value set toohello same name as used in hello upload of preceding SSL certificates section.</span></span> <span data-ttu-id="94957-162">hello frontend-porten ska vara uppdaterade too443.</span><span class="sxs-lookup"><span data-stu-id="94957-162">hello front-end port should be updated too443.</span></span>

<span data-ttu-id="94957-163">**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="94957-163">**tooenable cookie-based affinity**: An application gateway can be configured tooensure that a request from a client session is always directed toohello same VM in hello web farm.</span></span> <span data-ttu-id="94957-164">Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="94957-164">This scenario is done by injection of a session cookie that allows hello gateway toodirect traffic appropriately.</span></span> <span data-ttu-id="94957-165">Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.</span><span class="sxs-lookup"><span data-stu-id="94957-165">tooenable cookie-based affinity, set **CookieBasedAffinity** too*Enabled* in hello **BackendHttpSettings** element.</span></span>

<span data-ttu-id="94957-166">Du kan skapa din konfiguration genom att skapa ett konfigurationsobjekt eller med hjälp av en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="94957-166">You can construct your configuration either by creating a configuration object or by using a configuration XML file.</span></span>
<span data-ttu-id="94957-167">tooconstruct konfigurationen med hjälp av en konfiguration för XML-fil, använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="94957-167">tooconstruct your configuration by using a configuration XML file, use hello following sample:</span></span>

<span data-ttu-id="94957-168">**Konfiguration av XML-exempel**</span><span class="sxs-lookup"><span data-stu-id="94957-168">**Configuration XML sample**</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
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
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
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

## <a name="set-hello-gateway-configuration"></a><span data-ttu-id="94957-169">Ange hello gateway-konfiguration</span><span class="sxs-lookup"><span data-stu-id="94957-169">Set hello gateway configuration</span></span>

<span data-ttu-id="94957-170">Därefter måste ange du hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="94957-170">Next, you set hello application gateway.</span></span> <span data-ttu-id="94957-171">Du kan använda hello `Set-AzureApplicationGatewayConfig` cmdlet med ett konfigurationsobjekt eller med en XML-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="94957-171">You can use hello `Set-AzureApplicationGatewayConfig` cmdlet with either a configuration object or with a configuration XML file.</span></span>

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a><span data-ttu-id="94957-172">Starta hello gateway</span><span class="sxs-lookup"><span data-stu-id="94957-172">Start hello gateway</span></span>

<span data-ttu-id="94957-173">När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span><span class="sxs-lookup"><span data-stu-id="94957-173">Once hello gateway has been configured, use hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway.</span></span> <span data-ttu-id="94957-174">Fakturering för en Programgateway startar när hello gatewayen har startats.</span><span class="sxs-lookup"><span data-stu-id="94957-174">Billing for an application gateway begins after hello gateway has been successfully started.</span></span>

> [!NOTE]
> <span data-ttu-id="94957-175">Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toofinish too15 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="94957-175">hello `Start-AzureApplicationGateway` cmdlet might take up too15-20 minutes toofinish.</span></span>
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a><span data-ttu-id="94957-176">Verifiera hello gatewayens status</span><span class="sxs-lookup"><span data-stu-id="94957-176">Verify hello gateway status</span></span>

<span data-ttu-id="94957-177">Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello status för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="94957-177">Use hello `Get-AzureApplicationGateway` cmdlet toocheck hello status of hello gateway.</span></span> <span data-ttu-id="94957-178">Om `Start-AzureApplicationGateway` lyckades hello föregående steg, *tillstånd* ska köras och *virtualip:* och *DnsName* ska ha giltiga poster.</span><span class="sxs-lookup"><span data-stu-id="94957-178">If `Start-AzureApplicationGateway` succeeded in hello previous step, *State* should be Running, and *VirtualIPs* and *DnsName* should have valid entries.</span></span>

<span data-ttu-id="94957-179">Det här exemplet visar en Programgateway som är igång, körs och är redo tootake trafik.</span><span class="sxs-lookup"><span data-stu-id="94957-179">This sample shows an application gateway that is up, running, and is ready tootake traffic.</span></span>

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a><span data-ttu-id="94957-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94957-180">Next steps</span></span>

<span data-ttu-id="94957-181">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="94957-181">If you want more information about load balancing options in general, see:</span></span>

* [<span data-ttu-id="94957-182">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="94957-182">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="94957-183">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="94957-183">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

