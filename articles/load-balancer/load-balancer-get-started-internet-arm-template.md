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
# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Skapa en Internetuppkopplad belastningsutjämnare med hjälp av en mall

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med klassiska distributionsmodellen](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Distribuera hello mallen med Klicka toodeploy

hello exempelmall tillgängliga i hello offentliga databasen använder en parameterfil som innehåller hello standard värden som används för toogenerate hello scenario som beskrivs ovan. toodeploy den här mallen med hjälp av klickar du på toodeploy, Följ [länken](http://go.microsoft.com/fwlink/?LinkId=544801), klickar du på **distribuera tooAzure**, ersätta hello Standardparametervärden om det behövs och följ instruktionerna för hello hello-portalen.

## <a name="deploy-hello-template-by-using-powershell"></a>Distribuera hello-mallen med hjälp av PowerShell

toodeploy hello mallen som du hämtade med PowerShell, följ hello stegen nedan.

1. Om du aldrig har använt Azure PowerShell, se [hur tooInstall och konfigurera Azure PowerShell](/powershell/azure/overview) och följer instruktionerna för hello alla hello sätt toohello avslutas toosign till Azure och välja din prenumeration.
2. Kör hello **ny AzureRmResourceGroupDeployment** cmdlet toocreate en resurs med hello mallen.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
        -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
        -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
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

3. Från din webbläsare, navigerar för[hello Quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), kopiera hello innehållet i hello json-fil och klistra in i en ny fil i datorn. I det här scenariot kan du kopiera hello värden under tooa fil med namnet **c:\lb\azuredeploy.parameters.json**.
4. Kör hello **azure-gruppdistributionen skapa** cmdlet toodeploy hello nya belastningsutjämning med hjälp av hello mallen och parametern filer du hämtade och ändrade ovan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.

    ```azurecli
    azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
