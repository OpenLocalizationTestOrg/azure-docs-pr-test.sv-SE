---
title: aaaCreate en Programgateway Azure - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur toocreate en Programgateway med hjälp av hello Azure CLI 2.0 i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 952065586cd87d253882438bb779b768d9fd59fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a><span data-ttu-id="d6a25-103">Skapa en Programgateway med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d6a25-103">Create an application gateway by using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d6a25-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d6a25-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="d6a25-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d6a25-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="d6a25-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d6a25-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="d6a25-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="d6a25-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="d6a25-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d6a25-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="d6a25-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d6a25-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="d6a25-110">Programgateway är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 läsa in belastningsutjämning funktioner för ditt program.</span><span class="sxs-lookup"><span data-stu-id="d6a25-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="d6a25-111">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="d6a25-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="d6a25-112">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="d6a25-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="d6a25-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.</span><span class="sxs-lookup"><span data-stu-id="d6a25-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="d6a25-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="d6a25-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisite-install-hello-azure-cli-20"></a><span data-ttu-id="d6a25-115">Förutsättning: Installera hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d6a25-115">Prerequisite: Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="d6a25-116">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="d6a25-116">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="d6a25-117">Om du inte har ett Azure-konto behöver du en.</span><span class="sxs-lookup"><span data-stu-id="d6a25-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="d6a25-118">Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="d6a25-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="d6a25-119">Scenario</span><span class="sxs-lookup"><span data-stu-id="d6a25-119">Scenario</span></span>

<span data-ttu-id="d6a25-120">I det här scenariot kan du lära dig hur toocreate som en gateway som använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6a25-120">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="d6a25-121">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="d6a25-121">This scenario will:</span></span>

* <span data-ttu-id="d6a25-122">Skapa en medelhög Programgateway med två instanser.</span><span class="sxs-lookup"><span data-stu-id="d6a25-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="d6a25-123">Skapa ett virtuellt nätverk med namnet AdatumAppGatewayVNET med en 10.0.0.0/16 reserverade CIDR-blocket.</span><span class="sxs-lookup"><span data-stu-id="d6a25-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="d6a25-124">Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="d6a25-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="d6a25-125">Ytterligare konfiguration av hello Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när hello Programgateway har konfigurerats och inte under första distributionen.</span><span class="sxs-lookup"><span data-stu-id="d6a25-125">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d6a25-126">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d6a25-126">Before you begin</span></span>

<span data-ttu-id="d6a25-127">Azure Application Gateway kräver sin egen undernät.</span><span class="sxs-lookup"><span data-stu-id="d6a25-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="d6a25-128">När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät.</span><span class="sxs-lookup"><span data-stu-id="d6a25-128">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="d6a25-129">När du distribuerar ett program gateway tooa undernät kan bara ytterligare programgatewayer läggas toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="d6a25-129">Once you deploy an application gateway tooa subnet, only additional application gateways can be added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="d6a25-130">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="d6a25-130">Log in tooAzure</span></span>

<span data-ttu-id="d6a25-131">Öppna hello **Kommandotolken för Microsoft Azure**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="d6a25-131">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="d6a25-132">Du kan också använda `az login` utan hello växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="d6a25-132">You can also use `az login` without hello switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="d6a25-133">När du skriver hello föregående exempel, har en kod angetts.</span><span class="sxs-lookup"><span data-stu-id="d6a25-133">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="d6a25-134">Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="d6a25-134">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd visar enhet inloggning][1]

<span data-ttu-id="d6a25-136">Ange hello koden du fått i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d6a25-136">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="d6a25-137">Du kan omdirigerade tooa inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="d6a25-137">You are redirected tooa sign-in page.</span></span>

![webbläsaren tooenter kod][2]

<span data-ttu-id="d6a25-139">När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.</span><span class="sxs-lookup"><span data-stu-id="d6a25-139">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![loggat in][3]

## <a name="create-hello-resource-group"></a><span data-ttu-id="d6a25-141">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d6a25-141">Create hello resource group</span></span>

<span data-ttu-id="d6a25-142">Innan du skapar hello Programgateway skapas en resursgrupp toocontain hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d6a25-142">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="d6a25-143">hello följande visar hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="d6a25-143">hello following shows hello command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="d6a25-144">Skapa hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="d6a25-144">Create hello application gateway</span></span>

<span data-ttu-id="d6a25-145">hello IP-adresser som används för hello serverdel är hello IP-adresser för backend-servern.</span><span class="sxs-lookup"><span data-stu-id="d6a25-145">hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="d6a25-146">Dessa värden kan vara antingen privat IP-adresser i hello virtuellt nätverk, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="d6a25-146">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="d6a25-147">hello följande exempel skapas en Programgateway med ytterligare konfigurationsinställningar för http-inställningar, portar och regler.</span><span class="sxs-lookup"><span data-stu-id="d6a25-147">hello following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

<span data-ttu-id="d6a25-148">hello visas föregående exempel ett antal egenskaper som inte krävs under hello skapandet av en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d6a25-148">hello preceding example shows many properties that are not required during hello creation of an application gateway.</span></span> <span data-ttu-id="d6a25-149">Följande kodexempel hello skapar en Programgateway med hello krävs information.</span><span class="sxs-lookup"><span data-stu-id="d6a25-149">hello following code example creates an application gateway with hello required information.</span></span>

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> <span data-ttu-id="d6a25-150">En lista över parametrar som kan anges under hello för att skapa kör följande kommando: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="d6a25-150">For a list of parameters that can be provided during creation run hello following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="d6a25-151">Det här exemplet skapar en basic-Programgateway med standardinställningarna för hello lyssnare, serverdelspool, serverdelens http-inställningar och regler.</span><span class="sxs-lookup"><span data-stu-id="d6a25-151">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="d6a25-152">Du kan ändra dessa inställningar toosuit distributionen när hello etablering har lyckats.</span><span class="sxs-lookup"><span data-stu-id="d6a25-152">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="d6a25-153">Om du redan har ditt webbprogram som definierats med hello serverdelspool i föregående steg, när skapat hello börjar belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="d6a25-153">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="d6a25-154">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="d6a25-154">Get application gateway DNS name</span></span>

<span data-ttu-id="d6a25-155">När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="d6a25-155">Once hello gateway is created, hello next step is tooconfigure hello front end for communication.</span></span> <span data-ttu-id="d6a25-156">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="d6a25-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="d6a25-157">tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d6a25-157">tooensure end users can hit hello application gateway, a CNAME record can be used toopoint toohello public endpoint of hello application gateway.</span></span> <span data-ttu-id="d6a25-158">[Konfigurera ett eget domännamn i Azure](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d6a25-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="d6a25-159">tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d6a25-159">tooconfigure an alias, retrieve details of hello application gateway and its associated IP/DNS name using hello PublicIPAddress element attached toohello application gateway.</span></span> <span data-ttu-id="d6a25-160">hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="d6a25-160">hello application gateway's DNS name should be used toocreate a CNAME record, which points hello two web applications toothis DNS name.</span></span> <span data-ttu-id="d6a25-161">hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.</span><span class="sxs-lookup"><span data-stu-id="d6a25-161">hello use of A-records is not recommended since hello VIP may change on restart of application gateway.</span></span>


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a><span data-ttu-id="d6a25-162">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="d6a25-162">Delete all resources</span></span>

<span data-ttu-id="d6a25-163">toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d6a25-163">toodelete all resources created in this article, complete hello following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="d6a25-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6a25-164">Next steps</span></span>

<span data-ttu-id="d6a25-165">Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d6a25-165">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="d6a25-166">Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="d6a25-166">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
