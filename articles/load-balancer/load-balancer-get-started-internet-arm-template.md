---
title: "Skapa en Internetaktiverad belastningsutjämnare – Azure-mall | Microsoft Docs"
description: "Lär dig hur du skapar en Internetuppkopplad belastningsutjämnare i Resource Manager med hjälp av en mall"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: b24f4729-4559-4458-8527-71009d242647
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d829000e63515814b192f3f8256e3b8637bb3a34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="7a525-103">Skapa en Internetuppkopplad belastningsutjämnare med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="7a525-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7a525-104">Portal</span><span class="sxs-lookup"><span data-stu-id="7a525-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="7a525-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a525-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="7a525-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7a525-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="7a525-107">Mall</span><span class="sxs-lookup"><span data-stu-id="7a525-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="7a525-108">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7a525-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="7a525-109">Du kan också läsa om [hur du skapar en Internetuppkopplad belastningsutjämnare med hjälp av den klassiska distributionsmodellen](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7a525-109">You can also [Learn how to create an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="7a525-110">Distribuera mallen genom att klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="7a525-110">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="7a525-111">Exempelmallen som är tillgänglig i den offentliga databasen använder en parameterfil som innehåller standardvärdena som används för att generera scenariot som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="7a525-111">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="7a525-112">Om du vill distribuera den här mallen genom att klicka för att distribuera följer du [den här länken](http://go.microsoft.com/fwlink/?LinkId=544801), klickar på **Deploy to Azure** (Distribuera till Azure), ersätter standardparametervärdena om det behövs och följer anvisningarna på portalen.</span><span class="sxs-lookup"><span data-stu-id="7a525-112">To deploy this template using click to deploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="7a525-113">Distribuera mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a525-113">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="7a525-114">Följ stegen nedan om du vill distribuera mallen som du hämtat med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7a525-114">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="7a525-115">Om du aldrig använt Azure PowerShell tidigare, se [Installera och konfigurera Azure PowerShell](/powershell/azure/overview) och följ instruktionerna till slutet för att logga in på Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a525-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="7a525-116">Kör cmdleten **New-AzureRmResourceGroupDeployment** för att skapa en resursgrupp med hjälp av mallen.</span><span class="sxs-lookup"><span data-stu-id="7a525-116">Run the **New-AzureRmResourceGroupDeployment** cmdlet to create a resource group using the template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="7a525-117">Distribuera mallen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7a525-117">Deploy the template by using the Azure CLI</span></span>

<span data-ttu-id="7a525-118">Följ stegen nedan om du vill distribuera mallen med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7a525-118">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="7a525-119">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a525-119">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7a525-120">Kör kommandot **azure config mode** för att växla till Resource Manager-läge, som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7a525-120">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7a525-121">Följande utdata förväntas från kommandot ovan:</span><span class="sxs-lookup"><span data-stu-id="7a525-121">Here is the expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="7a525-122">Navigera till [snabbstartsmallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules) i webbläsaren, kopiera innehållet i JSON-filen och klistra in det i en ny fil på din dator.</span><span class="sxs-lookup"><span data-stu-id="7a525-122">From your browser, navigate to [the Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy the contents of the json file and paste into a new file in your computer.</span></span> <span data-ttu-id="7a525-123">I det här scenariot kopierar du värdena nedan till en fil med namnet **c:\lb\azuredeploy.parameters.json**.</span><span class="sxs-lookup"><span data-stu-id="7a525-123">For this scenario, you would be copying the values below to a file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="7a525-124">Kör cmdleten **azure group deployment create** för att distribuera den nya belastningsutjämnaren med hjälp av mall- och parameterfilerna som du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="7a525-124">Run the **azure group deployment create** cmdlet to deploy the new load balancer by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="7a525-125">Listan som visas efter alla utdata förklarar parametrarna som använts.</span><span class="sxs-lookup"><span data-stu-id="7a525-125">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="7a525-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a525-126">Next steps</span></span>

[<span data-ttu-id="7a525-127">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7a525-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="7a525-128">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7a525-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="7a525-129">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7a525-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
