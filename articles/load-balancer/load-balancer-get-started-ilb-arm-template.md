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
# <a name="create-an-internal-load-balancer-using-a-template"></a>Skapa en intern belastningsutjämnare med hjälp av en mall

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Distribuera hello mallen med Klicka toodeploy

hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan. toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.

## <a name="deploy-hello-template-by-using-powershell"></a>Distribuera hello-mallen med hjälp av PowerShell

toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.

1. Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.
2. Hämta hello parametrar filen tooyour lokal disk.
3. Redigera hello-filen och spara den.
4. Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toocreate en resurs med hello mallen.

    ```azurecli
    New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
        -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
        -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'
    ```

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Distribuera hello mallen med hello Azure CLI

toodeploy hello mall med hjälp av hello Azure CLI, följ hello stegen nedan.

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure config mode** kommandot tooswitch tooResource Manager-läge enligt nedan.

    ```azurecli
    azure config mode arm
    ```

    Här är förväntat hello utdata för hello ovanstående kommando:

        info:    New mode is arm

3. Öppna hello parameterfil, Välj dess innehåll och spara den tooa filen på datorn. Det här exemplet, vi sparat hello parameterfilen för*parameters.json*.
4. Kör hello **azure-gruppdistributionen skapa** toodeploy hello ny intern belastningsutjämning med hjälp av hello mallen och parametern kommandofiler du hämtade och ändrade ovan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.

    ```azurecli
    azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json
    ```

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare med hjälp av käll-IP-tilldelning](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

