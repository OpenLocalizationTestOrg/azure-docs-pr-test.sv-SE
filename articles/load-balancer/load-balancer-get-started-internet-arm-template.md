---
title: "belastningsutjämnaren aaaCreate mot en Internet - Azure-mall | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsfördelning i Resource Manager med hjälp av en mall"
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
ms.openlocfilehash: 2bce8cb87303838f3bc732d51228ab46d8015552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a><span data-ttu-id="7b890-103">Skapa en Internetuppkopplad belastningsutjämnare med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="7b890-103">Creating an Internet facing load balancer using a template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="7b890-104">Portal</span><span class="sxs-lookup"><span data-stu-id="7b890-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="7b890-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b890-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="7b890-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7b890-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="7b890-107">Mall</span><span class="sxs-lookup"><span data-stu-id="7b890-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="7b890-108">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="7b890-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="7b890-109">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med klassiska distributionsmodellen](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="7b890-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment model](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="7b890-110">Distribuera hello mallen med Klicka toodeploy</span><span class="sxs-lookup"><span data-stu-id="7b890-110">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="7b890-111">hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="7b890-111">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="7b890-112">toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](http://go.microsoft.com/fwlink/?LinkId=544801), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7b890-112">toodeploy this template using click toodeploy, follow [this link](http://go.microsoft.com/fwlink/?LinkId=544801), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="7b890-113">Distribuera hello-mallen med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7b890-113">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="7b890-114">toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="7b890-114">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="7b890-115">Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7b890-115">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="7b890-116">Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toocreate en resurs med hello mallen.</span><span class="sxs-lookup"><span data-stu-id="7b890-116">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="7b890-117">Distribuera hello mallen med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7b890-117">Deploy hello template by using hello Azure CLI</span></span>

<span data-ttu-id="7b890-118">toodeploy hello mall med hjälp av hello Azure CLI, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="7b890-118">toodeploy hello template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="7b890-119">Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7b890-119">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="7b890-120">Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="7b890-120">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="7b890-121">Här är förväntat hello utdata för hello ovanstående kommando:</span><span class="sxs-lookup"><span data-stu-id="7b890-121">Here is hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="7b890-122">Från din webbläsare, navigerar för[hello Quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), kopiera hello innehållet i hello json-fil och klistra in i en ny fil i datorn.</span><span class="sxs-lookup"><span data-stu-id="7b890-122">From your browser, navigate too[hello Quickstart Template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), copy hello contents of hello json file and paste into a new file in your computer.</span></span> <span data-ttu-id="7b890-123">I det här scenariot kan du kopiera hello värden under tooa fil med namnet **c:\lb\azuredeploy.parameters.json**.</span><span class="sxs-lookup"><span data-stu-id="7b890-123">For this scenario, you would be copying hello values below tooa file named **c:\lb\azuredeploy.parameters.json**.</span></span>
4. <span data-ttu-id="7b890-124">Kör hello **azure-gruppdistributionen skapa** cmdlet toodeploy hello nya belastningsutjämning med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan.</span><span class="sxs-lookup"><span data-stu-id="7b890-124">Run hello **azure group deployment create** cmdlet toodeploy hello new load balancer by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="7b890-125">hello-listan som visas efter hello utdata förklarar hello parametrar som används.</span><span class="sxs-lookup"><span data-stu-id="7b890-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a><span data-ttu-id="7b890-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b890-126">Next steps</span></span>

[<span data-ttu-id="7b890-127">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7b890-127">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="7b890-128">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7b890-128">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="7b890-129">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="7b890-129">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
