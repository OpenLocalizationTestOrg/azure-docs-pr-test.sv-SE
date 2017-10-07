---
title: "aaaCreate en intern belastningsutjämnare - Azure-mall | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare använder en mall i Resource Manager"
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
ms.openlocfilehash: 3ffa8178b863367cd79e2bc2b7ce4e45b23267e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-a-template"></a><span data-ttu-id="f0698-103">Skapa en intern belastningsutjämnare med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="f0698-103">Create an internal load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0698-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0698-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="f0698-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0698-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="f0698-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f0698-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="f0698-107">Mall</span><span class="sxs-lookup"><span data-stu-id="f0698-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="f0698-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f0698-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="f0698-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="f0698-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="f0698-110">Distribuera hello mallen med Klicka toodeploy</span><span class="sxs-lookup"><span data-stu-id="f0698-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="f0698-111">hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="f0698-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="f0698-112">toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f0698-112">toodeploy this template using click toodeploy, follow [this link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="f0698-113">Distribuera hello-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0698-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="f0698-114">toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="f0698-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="f0698-115">Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f0698-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="f0698-116">Hämta hello parametrar filen tooyour lokal disk.</span><span class="sxs-lookup"><span data-stu-id="f0698-116">Download hello parameters file tooyour local disk.</span></span>
3. <span data-ttu-id="f0698-117">Redigera hello-filen och spara den.</span><span class="sxs-lookup"><span data-stu-id="f0698-117">Edit hello file and save it.</span></span>
4. <span data-ttu-id="f0698-118">Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toocreate en resurs med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="f0698-118">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="f0698-119">Distribuera hello mallen med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f0698-119">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="f0698-120">toodeploy hello mall med hjälp av hello Azure CLI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="f0698-120">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="f0698-121">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f0698-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="f0698-122">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="f0698-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="f0698-123">Här är förväntat hello utdata för hello ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="f0698-123">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="f0698-124">Öppna hello parameterfil, Välj dess innehåll och spara den tooa filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f0698-124">Open hello parameter file, select its contents, and save it tooa file in your computer.</span></span> <span data-ttu-id="f0698-125">Det här exemplet, vi sparat hello parameterfilen för*parameters.json*.</span><span class="sxs-lookup"><span data-stu-id="f0698-125">For this example, we saved hello parameters file too*parameters.json*.</span></span>
4. <span data-ttu-id="f0698-126">Kör hello **azure-gruppdistributionen skapa** toodeploy hello ny intern belastningsutjämning med hjälp av hello mallen och parametern kommandofiler du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="f0698-126">Run hello **azure group deployment create** command toodeploy hello new internal load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="f0698-127">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="f0698-127">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a><span data-ttu-id="f0698-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0698-128">Next steps</span></span>

[<span data-ttu-id="f0698-129">Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning</span><span class="sxs-lookup"><span data-stu-id="f0698-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="f0698-130">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f0698-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

