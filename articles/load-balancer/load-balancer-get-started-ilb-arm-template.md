---
title: "Skapa en intern belastningsutjämnare – Azure-mall | Microsoft Docs"
description: "Lär dig hur du skapar en intern belastningsutjämnare med hjälp av en mall i Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 5e0278cf5c605298932d6ac55d147a1c43fd9d23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-using-a-template"></a><span data-ttu-id="bc639-103">Skapa en intern belastningsutjämnare med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="bc639-103">Create an internal load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bc639-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bc639-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="bc639-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bc639-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="bc639-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bc639-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="bc639-107">Mall</span><span class="sxs-lookup"><span data-stu-id="bc639-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="bc639-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bc639-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="bc639-109">Den här artikeln beskriver Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för [den klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="bc639-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a><span data-ttu-id="bc639-110">Distribuera mallen genom att klicka för att distribuera</span><span class="sxs-lookup"><span data-stu-id="bc639-110">Deploy the template by using click to deploy</span></span>

<span data-ttu-id="bc639-111">Exempelmallen som är tillgänglig i den offentliga databasen använder en parameterfil som innehåller standardvärdena som används för att generera scenariot som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="bc639-111">The sample template available in the public repository uses a parameter file containing the default values used to generate the scenario described above.</span></span> <span data-ttu-id="bc639-112">Om du vill distribuera den här mallen genom att klicka för att distribuera följer du [den här länken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klickar på **Deploy to Azure** (Distribuera till Azure), ersätter standardparametervärdena om det behövs och följer anvisningarna på portalen.</span><span class="sxs-lookup"><span data-stu-id="bc639-112">To deploy this template using click to deploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="deploy-the-template-by-using-powershell"></a><span data-ttu-id="bc639-113">Distribuera mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="bc639-113">Deploy the template by using PowerShell</span></span>

<span data-ttu-id="bc639-114">Följ stegen nedan om du vill distribuera mallen som du hämtat med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc639-114">To deploy the template you downloaded by using PowerShell, follow the steps below.</span></span>

1. <span data-ttu-id="bc639-115">Om du aldrig använt Azure PowerShell tidigare, se [Installera och konfigurera Azure PowerShell](/powershell/azure/overview) och följ instruktionerna till slutet för att logga in på Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc639-115">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="bc639-116">Hämta parameterfilen till din lokala disk.</span><span class="sxs-lookup"><span data-stu-id="bc639-116">Download the parameters file to your local disk.</span></span>
3. <span data-ttu-id="bc639-117">Redigera filen och spara den.</span><span class="sxs-lookup"><span data-stu-id="bc639-117">Edit the file and save it.</span></span>
4. <span data-ttu-id="bc639-118">Kör cmdleten **New-AzureRmResourceGroupDeployment** för att skapa en resursgrupp med hjälp av mallen.</span><span class="sxs-lookup"><span data-stu-id="bc639-118">Run the **New-AzureRmResourceGroupDeployment** cmdlet to create a resource group using the template.</span></span>

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a><span data-ttu-id="bc639-119">Distribuera mallen med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bc639-119">Deploy the template by using the Azure CLI</span></span>

<span data-ttu-id="bc639-120">Följ stegen nedan om du vill distribuera mallen med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bc639-120">To deploy the template by using the Azure CLI, follow the steps below.</span></span>

1. <span data-ttu-id="bc639-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera Azure CLI](../cli-install-nodejs.md) och följ instruktionerna upp till den punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bc639-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="bc639-122">Kör kommandot **azure config mode** för att växla till Resource Manager-läge, som det visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bc639-122">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="bc639-123">Följande utdata förväntas från kommandot ovan:</span><span class="sxs-lookup"><span data-stu-id="bc639-123">Here is the expected output for the command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="bc639-124">Öppna parameterfilen, markerar innehållet och spara det till en fil på din dator.</span><span class="sxs-lookup"><span data-stu-id="bc639-124">Open the parameter file, select its contents, and save it to a file in your computer.</span></span> <span data-ttu-id="bc639-125">I det här exemplet sparade vi parameterfilen till *parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="bc639-125">For this example, we saved the parameters file to *parameters.json*.</span></span>
4. <span data-ttu-id="bc639-126">Kör kommandot **azure group distribution create** för att distribuera den nya interna belastningsutjämnaren med hjälp av mall- och parameterfilerna som du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="bc639-126">Run the **azure group deployment create** command to deploy the new internal load balancer by using the template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="bc639-127">Listan som visas efter alla utdata förklarar parametrarna som använts.</span><span class="sxs-lookup"><span data-stu-id="bc639-127">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a><span data-ttu-id="bc639-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc639-128">Next steps</span></span>

[<span data-ttu-id="bc639-129">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="bc639-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="bc639-130">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="bc639-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

