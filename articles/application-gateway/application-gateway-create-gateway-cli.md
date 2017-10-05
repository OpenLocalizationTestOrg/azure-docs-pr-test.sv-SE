---
title: Skapa en Azure Programgateway - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur du skapar en Programgateway med hjälp av Azure CLI 2.0 i Resource Manager"
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
ms.openlocfilehash: 052410db8c7619c7990dc319951a55663f2c2ba1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a><span data-ttu-id="e983e-103">Skapa en Programgateway med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e983e-103">Create an application gateway by using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e983e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e983e-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="e983e-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e983e-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="e983e-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e983e-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="e983e-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="e983e-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="e983e-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e983e-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="e983e-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e983e-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="e983e-110">Programgateway är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 läsa in belastningsutjämning funktioner för ditt program.</span><span class="sxs-lookup"><span data-stu-id="e983e-110">Application Gateway is a dedicated virtual appliance that provides application delivery controller (ADC) as a service, offering various layer 7 load balancing capabilities for your application.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="e983e-111">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="e983e-111">CLI versions to complete the task</span></span>

<span data-ttu-id="e983e-112">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="e983e-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="e983e-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering.</span><span class="sxs-lookup"><span data-stu-id="e983e-113">[Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="e983e-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="e983e-114">[Azure CLI 2.0](application-gateway-create-gateway-cli.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="prerequisite-install-the-azure-cli-20"></a><span data-ttu-id="e983e-115">Förutsättning: Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e983e-115">Prerequisite: Install the Azure CLI 2.0</span></span>

<span data-ttu-id="e983e-116">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e983e-116">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

> [!NOTE]
> <span data-ttu-id="e983e-117">Om du inte har ett Azure-konto behöver du en.</span><span class="sxs-lookup"><span data-stu-id="e983e-117">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="e983e-118">Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="e983e-118">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="e983e-119">Scenario</span><span class="sxs-lookup"><span data-stu-id="e983e-119">Scenario</span></span>

<span data-ttu-id="e983e-120">Lär dig hur du skapar en Programgateway med Azure-portalen i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="e983e-120">In this scenario, you learn how to create an application gateway using the Azure portal.</span></span>

<span data-ttu-id="e983e-121">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="e983e-121">This scenario will:</span></span>

* <span data-ttu-id="e983e-122">Skapa en medelhög Programgateway med två instanser.</span><span class="sxs-lookup"><span data-stu-id="e983e-122">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="e983e-123">Skapa ett virtuellt nätverk med namnet AdatumAppGatewayVNET med en 10.0.0.0/16 reserverade CIDR-blocket.</span><span class="sxs-lookup"><span data-stu-id="e983e-123">Create a virtual network named AdatumAppGatewayVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="e983e-124">Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="e983e-124">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="e983e-125">Ytterligare konfiguration för Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när programgatewayen har konfigurerats och inte under första distributionen.</span><span class="sxs-lookup"><span data-stu-id="e983e-125">Additional configuration of the application gateway, including custom health probes, backend pool addresses, and additional rules are configured after the application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e983e-126">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e983e-126">Before you begin</span></span>

<span data-ttu-id="e983e-127">Azure Application Gateway kräver sin egen undernät.</span><span class="sxs-lookup"><span data-stu-id="e983e-127">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="e983e-128">När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme för att du har flera undernät.</span><span class="sxs-lookup"><span data-stu-id="e983e-128">When creating a virtual network, ensure that you leave enough address space to have multiple subnets.</span></span> <span data-ttu-id="e983e-129">När du distribuerar en Programgateway till ett undernät kan bara ytterligare programgatewayer läggas till undernätet.</span><span class="sxs-lookup"><span data-stu-id="e983e-129">Once you deploy an application gateway to a subnet, only additional application gateways can be added to the subnet.</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="e983e-130">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="e983e-130">Log in to Azure</span></span>

<span data-ttu-id="e983e-131">Öppna den **Kommandotolken för Microsoft Azure**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="e983e-131">Open the **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> <span data-ttu-id="e983e-132">Du kan också använda `az login` utan att växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.</span><span class="sxs-lookup"><span data-stu-id="e983e-132">You can also use `az login` without the switch for device login that requires entering a code at aka.ms/devicelogin.</span></span>

<span data-ttu-id="e983e-133">När du skriver i föregående exempel, har en kod angetts.</span><span class="sxs-lookup"><span data-stu-id="e983e-133">Once you type the preceding example, a code is provided.</span></span> <span data-ttu-id="e983e-134">Navigera till https://aka.ms/devicelogin i en webbläsare för att fortsätta inloggningen.</span><span class="sxs-lookup"><span data-stu-id="e983e-134">Navigate to https://aka.ms/devicelogin in a browser to continue the login process.</span></span>

![cmd visar enhet inloggning][1]

<span data-ttu-id="e983e-136">Ange den kod som du fick i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e983e-136">In the browser, enter the code you received.</span></span> <span data-ttu-id="e983e-137">Du omdirigeras till en inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e983e-137">You are redirected to a sign-in page.</span></span>

![webbläsare för att ange koden][2]

<span data-ttu-id="e983e-139">När du har angett koden du är inloggad, Stäng webbläsaren för att fortsätta scenariot.</span><span class="sxs-lookup"><span data-stu-id="e983e-139">Once the code has been entered you are signed in, close the browser to continue on with the scenario.</span></span>

![loggat in][3]

## <a name="create-the-resource-group"></a><span data-ttu-id="e983e-141">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e983e-141">Create the resource group</span></span>

<span data-ttu-id="e983e-142">Innan du skapar programgatewayen, skapas en resursgrupp för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="e983e-142">Before creating the application gateway, a resource group is created to contain the application gateway.</span></span> <span data-ttu-id="e983e-143">Nedan visas kommandot.</span><span class="sxs-lookup"><span data-stu-id="e983e-143">The following shows the command.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a><span data-ttu-id="e983e-144">Skapa programgatewayen</span><span class="sxs-lookup"><span data-stu-id="e983e-144">Create the application gateway</span></span>

<span data-ttu-id="e983e-145">IP-adresser som används för serverdelen är IP-adresser för backend-servern.</span><span class="sxs-lookup"><span data-stu-id="e983e-145">The IP addresses used for the backend are the IP addresses for your backend server.</span></span> <span data-ttu-id="e983e-146">Dessa värden kan vara privata IP-adresser i det virtuella nätverket, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="e983e-146">These values can be either private IPs in the virtual network, public ips, or fully qualified domain names for your backend servers.</span></span> <span data-ttu-id="e983e-147">I följande exempel skapas en Programgateway med ytterligare konfigurationsinställningar för http-inställningar, portar och regler.</span><span class="sxs-lookup"><span data-stu-id="e983e-147">The following example creates an application gateway with additional configuration settings for http settings, ports, and rules.</span></span>

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

<span data-ttu-id="e983e-148">I föregående exempel visas ett antal egenskaper som inte krävs under genereringen av en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="e983e-148">The preceding example shows many properties that are not required during the creation of an application gateway.</span></span> <span data-ttu-id="e983e-149">Följande exempel skapar en Programgateway med informationen som krävs.</span><span class="sxs-lookup"><span data-stu-id="e983e-149">The following code example creates an application gateway with the required information.</span></span>

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
> <span data-ttu-id="e983e-150">En lista över parametrar som kan tillhandahållas vid skapandet kör följande kommando: `az network application-gateway create --help`.</span><span class="sxs-lookup"><span data-stu-id="e983e-150">For a list of parameters that can be provided during creation run the following command: `az network application-gateway create --help`.</span></span>

<span data-ttu-id="e983e-151">Det här exemplet skapar en basic-Programgateway med standardinställningarna för lyssnare, serverdelspool, serverdelens http-inställningar och regler.</span><span class="sxs-lookup"><span data-stu-id="e983e-151">This example creates a basic application gateway with default settings for the listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="e983e-152">Du kan ändra dessa inställningar efter distributionen när etableringen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="e983e-152">You can modify these settings to suit your deployment once the provisioning is successful.</span></span>
<span data-ttu-id="e983e-153">Om du redan har ditt webbprogram som definierats med serverdelspoolen i föregående steg, skapas en gång, börjar belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="e983e-153">If you already have your web application defined with the backend pool in the preceding steps, once created, load balancing begins.</span></span>

## <a name="get-application-gateway-dns-name"></a><span data-ttu-id="e983e-154">Hämta DNS-namn för programgatewayen</span><span class="sxs-lookup"><span data-stu-id="e983e-154">Get application gateway DNS name</span></span>

<span data-ttu-id="e983e-155">När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e983e-155">Once the gateway is created, the next step is to configure the front end for communication.</span></span> <span data-ttu-id="e983e-156">När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt.</span><span class="sxs-lookup"><span data-stu-id="e983e-156">When using a public IP, application gateway requires a dynamically assigned DNS name, which is not friendly.</span></span> <span data-ttu-id="e983e-157">För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="e983e-157">To ensure end users can hit the application gateway, a CNAME record can be used to point to the public endpoint of the application gateway.</span></span> <span data-ttu-id="e983e-158">[Konfigurera ett eget domännamn i Azure](../dns/dns-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="e983e-158">[Configuring a custom domain name for in Azure](../dns/dns-custom-domain.md).</span></span> <span data-ttu-id="e983e-159">Hämta information om programgatewayen och dess tillhörande IP DNS-namn med PublicIPAddress-element som bifogas programgatewayen om du vill konfigurera ett alias.</span><span class="sxs-lookup"><span data-stu-id="e983e-159">To configure an alias, retrieve details of the application gateway and its associated IP/DNS name using the PublicIPAddress element attached to the application gateway.</span></span> <span data-ttu-id="e983e-160">programgatewayens DNS-namn ska användas för att skapa en CNAME-post som leder de två webbapparna till detta DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="e983e-160">The application gateway's DNS name should be used to create a CNAME record, which points the two web applications to this DNS name.</span></span> <span data-ttu-id="e983e-161">Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="e983e-161">The use of A-records is not recommended since the VIP may change on restart of application gateway.</span></span>


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

## <a name="delete-all-resources"></a><span data-ttu-id="e983e-162">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="e983e-162">Delete all resources</span></span>

<span data-ttu-id="e983e-163">Så här tar du bort alla resurser som skapats i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="e983e-163">To delete all resources created in this article, complete the following steps:</span></span>

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a><span data-ttu-id="e983e-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e983e-164">Next steps</span></span>

<span data-ttu-id="e983e-165">Lär dig hur du skapar anpassade hälsoavsökningar genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e983e-165">Learn how to create custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="e983e-166">Lär dig hur du konfigurerar SSL-avlastning och ta kostsamma SSL-dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="e983e-166">Learn how to configure SSL Offloading and take the costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
