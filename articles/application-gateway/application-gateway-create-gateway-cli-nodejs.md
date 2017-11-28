---
title: aaaCreate en Programgateway Azure - Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur toocreate en Programgateway med hjälp av hello Azure CLI 1.0 i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3c0d2d96b6be404d0372d00f0deb2a32959ca419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a><span data-ttu-id="80131-103">Skapa en Programgateway med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80131-103">Create an application gateway by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="80131-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80131-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="80131-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="80131-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="80131-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="80131-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="80131-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="80131-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="80131-108">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="80131-108">Azure CLI 1.0</span></span>](application-gateway-create-gateway-cli.md)
> * [<span data-ttu-id="80131-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="80131-109">Azure CLI 2.0</span></span>](application-gateway-create-gateway-cli.md)
> 
> 

<span data-ttu-id="80131-110">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="80131-110">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="80131-111">Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="80131-111">It provides failover, performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="80131-112">Programgateway har hello efter leverans tillämpningsfunktioner: HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet och Secure Sockets Layer (SSL)-avlastning, anpassade hälsoavsökningar och stöd för flera platser.</span><span class="sxs-lookup"><span data-stu-id="80131-112">Application gateway has hello following application delivery features: HTTP load balancing, cookie-based session affinity, and Secure Sockets Layer (SSL) offload, custom health probes, and support for multi-site.</span></span>

## <a name="prerequisite-install-hello-azure-cli"></a><span data-ttu-id="80131-113">Förutsättning: Installera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80131-113">Prerequisite: Install hello Azure CLI</span></span>

<span data-ttu-id="80131-114">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](../xplat-cli-install.md) och du behöver för[inloggning tooAzure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="80131-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](../xplat-cli-install.md) and you need too[log on tooAzure](../xplat-cli-connect.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="80131-115">Om du inte har ett Azure-konto behöver du en.</span><span class="sxs-lookup"><span data-stu-id="80131-115">If you don't have an Azure account, you need one.</span></span> <span data-ttu-id="80131-116">Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="80131-116">Go sign up for a [free trial here](../active-directory/sign-up-organization.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="80131-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="80131-117">Scenario</span></span>

<span data-ttu-id="80131-118">I det här scenariot kan du lära dig hur toocreate som en gateway som använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80131-118">In this scenario, you learn how toocreate an application gateway using hello Azure portal.</span></span>

<span data-ttu-id="80131-119">Det här scenariot kommer:</span><span class="sxs-lookup"><span data-stu-id="80131-119">This scenario will:</span></span>

* <span data-ttu-id="80131-120">Skapa en medelhög Programgateway med två instanser.</span><span class="sxs-lookup"><span data-stu-id="80131-120">Create a medium application gateway with two instances.</span></span>
* <span data-ttu-id="80131-121">Skapa ett virtuellt nätverk med namnet ContosoVNET med en 10.0.0.0/16 reserverade CIDR-blocket.</span><span class="sxs-lookup"><span data-stu-id="80131-121">Create a virtual network named ContosoVNET with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="80131-122">Skapa ett undernät som kallas subnet01 som använder 10.0.0.0/28 som sitt CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="80131-122">Create a subnet called subnet01 that uses 10.0.0.0/28 as its CIDR block.</span></span>

> [!NOTE]
> <span data-ttu-id="80131-123">Ytterligare konfiguration av hello Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när hello Programgateway har konfigurerats och inte under första distributionen.</span><span class="sxs-lookup"><span data-stu-id="80131-123">Additional configuration of hello application gateway, including custom health probes, backend pool addresses, and additional rules are configured after hello application gateway is configured and not during initial deployment.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="80131-124">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="80131-124">Before you begin</span></span>

<span data-ttu-id="80131-125">Azure Application Gateway kräver sin egen undernät.</span><span class="sxs-lookup"><span data-stu-id="80131-125">Azure Application Gateway requires its own subnet.</span></span> <span data-ttu-id="80131-126">När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät.</span><span class="sxs-lookup"><span data-stu-id="80131-126">When creating a virtual network, ensure that you leave enough address space toohave multiple subnets.</span></span> <span data-ttu-id="80131-127">När du distribuerar ett program tooa gatewayundernät endast ytterligare programgatewayer är kan toobe läggs toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="80131-127">Once you deploy an application gateway tooa subnet, only additional application gateways are able toobe added toohello subnet.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="80131-128">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="80131-128">Log in tooAzure</span></span>

<span data-ttu-id="80131-129">Öppna hello **Kommandotolken för Microsoft Azure**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="80131-129">Open hello **Microsoft Azure Command Prompt**, and log in.</span></span> 

```azurecli-interactive
azure login
```

<span data-ttu-id="80131-130">När du skriver hello föregående exempel, har en kod angetts.</span><span class="sxs-lookup"><span data-stu-id="80131-130">Once you type hello preceding example, a code is provided.</span></span> <span data-ttu-id="80131-131">Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="80131-131">Navigate toohttps://aka.ms/devicelogin in a browser toocontinue hello login process.</span></span>

![cmd visar enhet inloggning][1]

<span data-ttu-id="80131-133">Ange hello koden du fått i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="80131-133">In hello browser, enter hello code you received.</span></span> <span data-ttu-id="80131-134">Du kan omdirigerade tooa inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="80131-134">You are redirected tooa sign-in page.</span></span>

![webbläsaren tooenter kod][2]

<span data-ttu-id="80131-136">När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.</span><span class="sxs-lookup"><span data-stu-id="80131-136">Once hello code has been entered you are signed in, close hello browser toocontinue on with hello scenario.</span></span>

![loggat in][3]

## <a name="switch-tooresource-manager-mode"></a><span data-ttu-id="80131-138">Växla tooResource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="80131-138">Switch tooResource Manager Mode</span></span>

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a><span data-ttu-id="80131-139">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="80131-139">Create hello resource group</span></span>

<span data-ttu-id="80131-140">Innan du skapar hello Programgateway skapas en resursgrupp toocontain hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="80131-140">Before creating hello application gateway, a resource group is created toocontain hello application gateway.</span></span> <span data-ttu-id="80131-141">hello följande visar hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="80131-141">hello following shows hello command.</span></span>

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="80131-142">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="80131-142">Create a virtual network</span></span>

<span data-ttu-id="80131-143">När du har skapat hello resursgruppen skapas ett virtuellt nätverk för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="80131-143">Once hello resource group is created, a virtual network is created for hello application gateway.</span></span>  <span data-ttu-id="80131-144">I följande exempel hello, var hello-adressutrymmet som 10.0.0.0/16 enligt definitionen i hello föregående scenariot anteckningar.</span><span class="sxs-lookup"><span data-stu-id="80131-144">In hello following example, hello address space was as 10.0.0.0/16 as defined in hello preceding scenario notes.</span></span>

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a><span data-ttu-id="80131-145">Skapa ett undernät</span><span class="sxs-lookup"><span data-stu-id="80131-145">Create a subnet</span></span>

<span data-ttu-id="80131-146">När hello virtuella nätverket har skapats läggs ett undernät för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="80131-146">After hello virtual network is created, a subnet is added for hello application gateway.</span></span>  <span data-ttu-id="80131-147">Om du planerar toouse Programgateway med ett webbprogram finns i hello samma virtuella nätverk som hello Programgateway, och vara säker på att tooleave tillräckligt med utrymme för ett annat undernät.</span><span class="sxs-lookup"><span data-stu-id="80131-147">If you plan toouse application gateway with a web app hosted in hello same virtual network as hello application gateway, be sure tooleave enough room for another subnet.</span></span>

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a><span data-ttu-id="80131-148">Skapa hello Programgateway</span><span class="sxs-lookup"><span data-stu-id="80131-148">Create hello application gateway</span></span>

<span data-ttu-id="80131-149">När hello virtuellt nätverk och undernät skapas, är hello förutsättningar för hello Programgateway klar.</span><span class="sxs-lookup"><span data-stu-id="80131-149">Once hello virtual network and subnet are created, hello pre-requisites for hello application gateway are complete.</span></span> <span data-ttu-id="80131-150">En tidigare exporterade PFX-certifikat och hello lösenordet för hello certifikatet krävs dessutom hello följande steg: hello IP-adresser som används för hello serverdel är hello IP-adresser för backend-servern.</span><span class="sxs-lookup"><span data-stu-id="80131-150">Additionally a previously exported .pfx certificate and hello password for hello certificate are required for hello following step: hello IP addresses used for hello backend are hello IP addresses for your backend server.</span></span> <span data-ttu-id="80131-151">Dessa värden kan vara antingen privat IP-adresser i hello virtuellt nätverk, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="80131-151">These values can be either private IPs in hello virtual network, public ips, or fully qualified domain names for your backend servers.</span></span>

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> <span data-ttu-id="80131-152">En lista över parametrar som kan tillhandahållas vid skapandet kör följande kommando hello: **azure-nätverk Programgateway skapa--hjälpa**.</span><span class="sxs-lookup"><span data-stu-id="80131-152">For a list of parameters that can be provided during creation run hello following command: **azure network application-gateway create --help**.</span></span>

<span data-ttu-id="80131-153">Det här exemplet skapar en basic-Programgateway med standardinställningarna för hello lyssnare, serverdelspool, serverdelens http-inställningar och regler.</span><span class="sxs-lookup"><span data-stu-id="80131-153">This example creates a basic application gateway with default settings for hello listener, backend pool, backend http settings, and rules.</span></span> <span data-ttu-id="80131-154">Du kan ändra dessa inställningar toosuit distributionen när hello etablering har lyckats.</span><span class="sxs-lookup"><span data-stu-id="80131-154">You can modify these settings toosuit your deployment once hello provisioning is successful.</span></span>
<span data-ttu-id="80131-155">Om du redan har ditt webbprogram som definierats med hello serverdelspool i föregående steg, när skapat hello börjar belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="80131-155">If you already have your web application defined with hello backend pool in hello preceding steps, once created, load balancing begins.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80131-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80131-156">Next steps</span></span>

<span data-ttu-id="80131-157">Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)</span><span class="sxs-lookup"><span data-stu-id="80131-157">Learn how toocreate custom health probes by visiting [Create a custom health probe](application-gateway-create-probe-portal.md)</span></span>

<span data-ttu-id="80131-158">Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)</span><span class="sxs-lookup"><span data-stu-id="80131-158">Learn how tooconfigure SSL Offloading and take hello costly SSL decryption off your web servers by visiting [Configure SSL Offload](application-gateway-ssl-arm.md)</span></span>

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
