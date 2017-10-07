---
title: aaaCreate Programgateway en Azure - mallar | Microsoft Docs
description: "Den här sidan innehåller instruktioner toocreate en gateway för Azure-program med hello Azure Resource Manager-mall"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: fc18e553852551326d6a302abe2c7f8a08c2eb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a><span data-ttu-id="cc79f-103">Skapa en Programgateway med hello Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="cc79f-103">Create an application gateway by using hello Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cc79f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cc79f-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="cc79f-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="cc79f-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="cc79f-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cc79f-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="cc79f-107">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="cc79f-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="cc79f-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cc79f-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="cc79f-109">Azure Application Gateway är en Layer 7-belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="cc79f-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="cc79f-110">Det ger redundans och prestanda inkommande HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="cc79f-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="cc79f-111">Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="cc79f-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="cc79f-112">toofind en fullständig lista över funktioner som stöds finns [Programgateway översikt](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="cc79f-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="cc79f-113">Den här artikeln beskriver hur du hämtar och ändrar en befintlig Azure Resource Manager-mall från GitHub och distribuerar hello-mall från GitHub, PowerShell och hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cc79f-113">This article walks you through downloading and modifying an existing Azure Resource Manager template from GitHub and deploying hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="cc79f-114">Om du bara distribuerar hello Azure Resource Manager-mallen direkt från GitHub utan ändringar, kan du hoppa över toodeploy en mall från GitHub.</span><span class="sxs-lookup"><span data-stu-id="cc79f-114">If you are simply deploying hello Azure Resource Manager template directly from GitHub without any changes, skip toodeploy a template from GitHub.</span></span>

## <a name="scenario"></a><span data-ttu-id="cc79f-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="cc79f-115">Scenario</span></span>

<span data-ttu-id="cc79f-116">I det här scenariot ska du:</span><span class="sxs-lookup"><span data-stu-id="cc79f-116">In this scenario you will:</span></span>

* <span data-ttu-id="cc79f-117">Skapa en Programgateway med Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="cc79f-117">Create an application gateway with web application firewall.</span></span>
* <span data-ttu-id="cc79f-118">Skapa ett virtuellt nätverk med namnet VirtualNetwork1 med det reserverade CIDR-blocket 10.0.0.0/16.</span><span class="sxs-lookup"><span data-stu-id="cc79f-118">Create a virtual network named VirtualNetwork1 with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="cc79f-119">Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.</span><span class="sxs-lookup"><span data-stu-id="cc79f-119">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>
* <span data-ttu-id="cc79f-120">Ställa in två tidigare konfigurerade backend-IP-adresser för hello webbservrar du tooload Utjämna hello trafiken.</span><span class="sxs-lookup"><span data-stu-id="cc79f-120">Set up two previously configured back-end IPs for hello web servers you want tooload balance hello traffic.</span></span> <span data-ttu-id="cc79f-121">I det här exemplet mallen hello backend-IP-adresser är 10.0.1.10 och 10.0.1.11.</span><span class="sxs-lookup"><span data-stu-id="cc79f-121">In this template example, hello back-end IPs are 10.0.1.10 and 10.0.1.11.</span></span>

> [!NOTE]
> <span data-ttu-id="cc79f-122">Dessa inställningar är hello parametrar för den här mallen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-122">Those settings are hello parameters for this template.</span></span> <span data-ttu-id="cc79f-123">toocustomize hello mall som du kan ändra regler, hello lyssnare, SSL och andra alternativ i hello azuredeploy.json-filen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-123">toocustomize hello template, you can change rules, hello listener, SSL, and other options in hello azuredeploy.json file.</span></span>

![Scenario](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="cc79f-125">Hämta och förstå hello Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="cc79f-125">Download and understand hello Azure Resource Manager template</span></span>

<span data-ttu-id="cc79f-126">Du kan ladda ned hello befintliga Azure Resource Manager-mall toocreate ett virtuellt nätverk och två undernät från GitHub, göra ändringar du vill och återanvända den.</span><span class="sxs-lookup"><span data-stu-id="cc79f-126">You can download hello existing Azure Resource Manager template toocreate a virtual network and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="cc79f-127">toodo så Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc79f-127">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="cc79f-128">Navigera för[skapa Programgateway med Brandvägg för webbaserade program aktiverat](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="cc79f-128">Navigate too[Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="cc79f-129">Klicka på **azuredeploy.json** och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-129">Click **azuredeploy.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="cc79f-130">Spara hello tooa lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="cc79f-130">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="cc79f-131">Om du är bekant med Azure Resource Manager-mallar kan du hoppa över toostep 7.</span><span class="sxs-lookup"><span data-stu-id="cc79f-131">If you are familiar with Azure Resource Manager templates, skip toostep 7.</span></span>
1. <span data-ttu-id="cc79f-132">Öppna hello-fil som du har sparat och titta på hello innehållet under **parametrar** i rad</span><span class="sxs-lookup"><span data-stu-id="cc79f-132">Open hello file that you saved and look at hello contents under **parameters** in line</span></span>
1. <span data-ttu-id="cc79f-133">Azure Resource Manager-mallens parametrar fungerar som platshållare för värden som kan anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-133">Azure Resource Manager template parameters provide a placeholder for values that can be filled out during deployment.</span></span>

  | <span data-ttu-id="cc79f-134">Parameter</span><span class="sxs-lookup"><span data-stu-id="cc79f-134">Parameter</span></span> | <span data-ttu-id="cc79f-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cc79f-135">Description</span></span> |
  | --- | --- |
  | <span data-ttu-id="cc79f-136">**subnetPrefix**</span><span class="sxs-lookup"><span data-stu-id="cc79f-136">**subnetPrefix**</span></span> |<span data-ttu-id="cc79f-137">CIDR-block för hello programmet gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="cc79f-137">CIDR block for hello application gateway subnet.</span></span> |
  | <span data-ttu-id="cc79f-138">**applicationGatewaySize**</span><span class="sxs-lookup"><span data-stu-id="cc79f-138">**applicationGatewaySize**</span></span> | <span data-ttu-id="cc79f-139">Storleken på hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="cc79f-139">Size of hello application gateway.</span></span>  <span data-ttu-id="cc79f-140">Brandvägg kan bara medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="cc79f-140">WAF only allows medium and large.</span></span> |
  | <span data-ttu-id="cc79f-141">**backendIpaddress1**</span><span class="sxs-lookup"><span data-stu-id="cc79f-141">**backendIpaddress1**</span></span> |<span data-ttu-id="cc79f-142">IP-adress hello första webbserver.</span><span class="sxs-lookup"><span data-stu-id="cc79f-142">IP address of hello first web server.</span></span> |
  | <span data-ttu-id="cc79f-143">**backendIpaddress2**</span><span class="sxs-lookup"><span data-stu-id="cc79f-143">**backendIpaddress2**</span></span> |<span data-ttu-id="cc79f-144">IP-adress hello andra webbservern.</span><span class="sxs-lookup"><span data-stu-id="cc79f-144">IP address of hello second web server.</span></span> |
  | <span data-ttu-id="cc79f-145">**wafEnabled**</span><span class="sxs-lookup"><span data-stu-id="cc79f-145">**wafEnabled**</span></span> | <span data-ttu-id="cc79f-146">Ange toodetermine om Brandvägg är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cc79f-146">Setting toodetermine if WAF is enabled.</span></span>|
  | <span data-ttu-id="cc79f-147">**wafMode**</span><span class="sxs-lookup"><span data-stu-id="cc79f-147">**wafMode**</span></span> | <span data-ttu-id="cc79f-148">Läget för hello Brandvägg för webbaserade program.</span><span class="sxs-lookup"><span data-stu-id="cc79f-148">Mode of hello web application firewall.</span></span>  <span data-ttu-id="cc79f-149">Alternativen är **förebyggande** eller **identifiering**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-149">Available options are **prevention** or **detection**.</span></span>|
  | <span data-ttu-id="cc79f-150">**wafRuleSetType**</span><span class="sxs-lookup"><span data-stu-id="cc79f-150">**wafRuleSetType**</span></span> | <span data-ttu-id="cc79f-151">RuleSet-typ för Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="cc79f-151">Ruleset type for WAF.</span></span>  <span data-ttu-id="cc79f-152">OWASP är för närvarande hello stöds endast alternativet.</span><span class="sxs-lookup"><span data-stu-id="cc79f-152">Currently OWASP is hello only supported option.</span></span> |
  | <span data-ttu-id="cc79f-153">**wafRuleSetVersion**</span><span class="sxs-lookup"><span data-stu-id="cc79f-153">**wafRuleSetVersion**</span></span> |<span data-ttu-id="cc79f-154">RuleSet-version.</span><span class="sxs-lookup"><span data-stu-id="cc79f-154">Ruleset version.</span></span> <span data-ttu-id="cc79f-155">OWASP CRS 2.2.9 och 3.0 finns för närvarande hello stöds alternativ.</span><span class="sxs-lookup"><span data-stu-id="cc79f-155">OWASP CRS 2.2.9 and 3.0 are currently hello supported options.</span></span> |

1. <span data-ttu-id="cc79f-156">Kontrollera hello innehåll under **resurser** och meddelande hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="cc79f-156">Check hello content under **resources** and notice hello following properties:</span></span>

   * <span data-ttu-id="cc79f-157">**type**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-157">**type**.</span></span> <span data-ttu-id="cc79f-158">Typ av resurs som skapas av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-158">Type of resource being created by hello template.</span></span> <span data-ttu-id="cc79f-159">I det här fallet hello typen är `Microsoft.Network/applicationGateways`, som representerar en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="cc79f-159">In this case, hello type is `Microsoft.Network/applicationGateways`, which represents an application gateway.</span></span>
   * <span data-ttu-id="cc79f-160">**name**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-160">**name**.</span></span> <span data-ttu-id="cc79f-161">Namnet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="cc79f-161">Name for hello resource.</span></span> <span data-ttu-id="cc79f-162">Meddelande hello användning av `[parameters('applicationGatewayName')]`, vilket innebär att hello-namn har angetts som indata av dig eller en parameterfil under distributionen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-162">Notice hello use of `[parameters('applicationGatewayName')]`, which means that hello name is provided as input by you or by a parameter file during deployment.</span></span>
   * <span data-ttu-id="cc79f-163">**properties**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-163">**properties**.</span></span> <span data-ttu-id="cc79f-164">Lista över egenskaper för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="cc79f-164">List of properties for hello resource.</span></span> <span data-ttu-id="cc79f-165">Den här mallen använder hello virtuella nätverk och offentlig IP-adress när programmet gateway skapas.</span><span class="sxs-lookup"><span data-stu-id="cc79f-165">This template uses hello virtual network and public IP address during application gateway creation.</span></span>

   > [!NOTE]
   > <span data-ttu-id="cc79f-166">Mer information om mallar finns: [referens för Resource Manager-mallar](/templates/)</span><span class="sxs-lookup"><span data-stu-id="cc79f-166">For more information on templates visit: [Resource Manager templates reference](/templates/)</span></span>

1. <span data-ttu-id="cc79f-167">Gå tillbaka för[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="cc79f-167">Navigate back too[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="cc79f-168">Klicka på **azuredeploy-parameters.json**, och klicka sedan på **RAW**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-168">Click **azuredeploy-parameters.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="cc79f-169">Spara hello tooa lokal mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="cc79f-169">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="cc79f-170">Öppna hello-filen som du sparade och redigera hello värden för hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="cc79f-170">Open hello file that you saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="cc79f-171">Använd hello följande värden toodeploy hello Programgateway beskrivs i vårt scenario.</span><span class="sxs-lookup"><span data-stu-id="cc79f-171">Use hello following values toodeploy hello application gateway described in our scenario.</span></span>

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. <span data-ttu-id="cc79f-172">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-172">Save hello file.</span></span> <span data-ttu-id="cc79f-173">Du kan testa hello JSON-mall och parametern-mallen med hjälp av online-verifiering JSON-verktyg som [JSlint.com](http://www.jslint.com/).</span><span class="sxs-lookup"><span data-stu-id="cc79f-173">You can test hello JSON template and parameter template by using online JSON validation tools like [JSlint.com](http://www.jslint.com/).</span></span>

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a><span data-ttu-id="cc79f-174">Distribuera hello Azure Resource Manager-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="cc79f-174">Deploy hello Azure Resource Manager template by using PowerShell</span></span>

<span data-ttu-id="cc79f-175">Om du aldrig har använt Azure PowerShell gå: [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följ hello instruktioner toosign till Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cc79f-175">If you have never used Azure PowerShell, visit: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions toosign into Azure and select your subscription.</span></span>

1. <span data-ttu-id="cc79f-176">Inloggningen tooPowerShell</span><span class="sxs-lookup"><span data-stu-id="cc79f-176">Login tooPowerShell</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="cc79f-177">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="cc79f-177">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="cc79f-178">Du kan ange tooauthenticate med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cc79f-178">You are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="cc79f-179">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="cc79f-179">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. <span data-ttu-id="cc79f-180">Om det behövs kan du skapa en resursgrupp med hjälp av hello **New-AzureResourceGroup** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cc79f-180">If needed, create a resource group by using hello **New-AzureResourceGroup** cmdlet.</span></span> <span data-ttu-id="cc79f-181">I följande exempel hello, skapar du en resursgrupp med namnet AppgatewayRG i östra USA plats.</span><span class="sxs-lookup"><span data-stu-id="cc79f-181">In hello following example, you create a resource group called AppgatewayRG in East US location.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. <span data-ttu-id="cc79f-182">Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toodeploy hello nytt virtuellt nätverk med hjälp av hello föregående mall och parametern filer som du hämtade och ändrade.</span><span class="sxs-lookup"><span data-stu-id="cc79f-182">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toodeploy hello new virtual network by using hello preceding template and parameter files you downloaded and modified.</span></span>
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a><span data-ttu-id="cc79f-183">Distribuera hello Azure Resource Manager-mall med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cc79f-183">Deploy hello Azure Resource Manager template by using hello Azure CLI</span></span>

<span data-ttu-id="cc79f-184">toodeploy hello Azure Resource Manager-mall som du hämtade med Azure CLI, så hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc79f-184">toodeploy hello Azure Resource Manager template you downloaded by using Azure CLI, follow hello following steps:</span></span>

1. <span data-ttu-id="cc79f-185">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cc79f-185">If you have never used Azure CLI, see [Install and configure hello Azure CLI](/cli/azure/install-azure-cli) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>

1. <span data-ttu-id="cc79f-186">Om nödvändigt kör hello `az group create` kommandot toocreate en resursgrupp, enligt följande kodavsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="cc79f-186">If necessary, run hello `az group create` command toocreate a resource group, as shown in hello following code snippet.</span></span> <span data-ttu-id="cc79f-187">Observera hello hello kommandots utdata.</span><span class="sxs-lookup"><span data-stu-id="cc79f-187">Notice hello output of hello command.</span></span> <span data-ttu-id="cc79f-188">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="cc79f-188">hello list shown after hello output explains hello parameters used.</span></span> <span data-ttu-id="cc79f-189">Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cc79f-189">For more information about resource groups, visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    <span data-ttu-id="cc79f-190">**-n (eller --name)**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-190">**-n (or --name)**.</span></span> <span data-ttu-id="cc79f-191">Namn för hello ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="cc79f-191">Name for hello new resource group.</span></span> <span data-ttu-id="cc79f-192">I vårt scenario är det *appgatewayRG*.</span><span class="sxs-lookup"><span data-stu-id="cc79f-192">For our scenario, it's *appgatewayRG*.</span></span>
    
    <span data-ttu-id="cc79f-193">**-l (eller --location)**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-193">**-l (or --location)**.</span></span> <span data-ttu-id="cc79f-194">Azure-region där hello ny resursgrupp skapas.</span><span class="sxs-lookup"><span data-stu-id="cc79f-194">Azure region where hello new resource group is created.</span></span> <span data-ttu-id="cc79f-195">I vårt scenario, den har *westus*.</span><span class="sxs-lookup"><span data-stu-id="cc79f-195">For our scenario, it's *westus*.</span></span>

1. <span data-ttu-id="cc79f-196">Kör hello `az group deployment create` cmdlet toodeploy hello nytt virtuellt nätverk med hjälp av hello mallen och parametern filer du hämtade och ändrade i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="cc79f-196">Run hello `az group deployment create` cmdlet toodeploy hello new virtual network by using hello template and parameter files you downloaded and modified in hello preceding step.</span></span> <span data-ttu-id="cc79f-197">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="cc79f-197">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a><span data-ttu-id="cc79f-198">Distribuera hello Azure Resource Manager-mallen med Klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="cc79f-198">Deploy hello Azure Resource Manager template by using click-to-deploy</span></span>

<span data-ttu-id="cc79f-199">Klicka för att distribuera är ett annat sätt toouse Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="cc79f-199">Click-to-deploy is another way toouse Azure Resource Manager templates.</span></span> <span data-ttu-id="cc79f-200">Det är ett enkelt sätt toouse mallar med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-200">It's an easy way toouse templates with hello Azure portal.</span></span>

1. <span data-ttu-id="cc79f-201">Gå för[skapa en Programgateway med Brandvägg för webbaserade program](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span><span class="sxs-lookup"><span data-stu-id="cc79f-201">Go too[Create an application gateway with web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span></span>

1. <span data-ttu-id="cc79f-202">Klicka på **distribuera tooAzure**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-202">Click **Deploy tooAzure**.</span></span>

    ![Distribuera tooAzure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. <span data-ttu-id="cc79f-204">Fyll i hello parametrar för hello Distributionsmall på hello-portalen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-204">Fill out hello parameters for hello deployment template on hello portal and click **OK**.</span></span>

    ![Parametrar](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. <span data-ttu-id="cc79f-206">Välj **acceptera toohello villkoren ovan** och på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-206">Select **I agree toohello terms and conditions stated above** and click **Purchase**.</span></span>

1. <span data-ttu-id="cc79f-207">På hello anpassad distribution bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="cc79f-207">On hello Custom deployment blade, click **Create**.</span></span>

## <a name="providing-certificate-data-tooresource-manager-templates"></a><span data-ttu-id="cc79f-208">Att tillhandahålla certifikat data tooResource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="cc79f-208">Providing certificate data tooResource Manager templates</span></span>

<span data-ttu-id="cc79f-209">När du använder SSL med en mall måste hello certifikat toobe som anges i en base64-sträng i stället för som överförs.</span><span class="sxs-lookup"><span data-stu-id="cc79f-209">When using SSL with a template, hello certificate needs toobe provided in a base64 string instead of being uploaded.</span></span> <span data-ttu-id="cc79f-210">tooconvert en PFX- eller .cer tooa base64-sträng med någon av följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="cc79f-210">tooconvert a .pfx or .cer tooa base64 string use one of hello following commands.</span></span> <span data-ttu-id="cc79f-211">hello konvertera följande kommandon hello certifikat tooa base64-sträng, vilket kan anges toohello mall.</span><span class="sxs-lookup"><span data-stu-id="cc79f-211">hello following commands convert hello certificate tooa base64 string, which can be provided toohello template.</span></span> <span data-ttu-id="cc79f-212">hello förväntade utdata är en sträng som kan lagras i en variabel och klistras in i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="cc79f-212">hello expected output is a string that can be stored in a variable and pasted in hello template.</span></span>

### <a name="macos"></a><span data-ttu-id="cc79f-213">macOS</span><span class="sxs-lookup"><span data-stu-id="cc79f-213">macOS</span></span>
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a><span data-ttu-id="cc79f-214">Windows</span><span class="sxs-lookup"><span data-stu-id="cc79f-214">Windows</span></span>
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a><span data-ttu-id="cc79f-215">Ta bort alla resurser</span><span class="sxs-lookup"><span data-stu-id="cc79f-215">Delete all resources</span></span>

<span data-ttu-id="cc79f-216">toodelete alla resurser skapas i den här artikeln, någon av följande hello:</span><span class="sxs-lookup"><span data-stu-id="cc79f-216">toodelete all resources created in this article, complete one of hello following steps:</span></span>

### <a name="powershell"></a><span data-ttu-id="cc79f-217">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cc79f-217">PowerShell</span></span>

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a><span data-ttu-id="cc79f-218">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cc79f-218">Azure CLI</span></span>

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a><span data-ttu-id="cc79f-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc79f-219">Next steps</span></span>

<span data-ttu-id="cc79f-220">Om du vill tooconfigure SSL-avlastning, besök: [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="cc79f-220">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="cc79f-221">Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare: [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="cc79f-221">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="cc79f-222">Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:</span><span class="sxs-lookup"><span data-stu-id="cc79f-222">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="cc79f-223">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="cc79f-223">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="cc79f-224">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cc79f-224">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

