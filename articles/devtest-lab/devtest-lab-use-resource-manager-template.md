---
title: "aaaView och använda en virtuell dators Azure Resource Manager-mall | Microsoft Docs"
description: "Lär dig hur hello toouse Azure Resource Manager-mall från en virtuell dator toocreate andra virtuella datorer"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: tarcher
ms.openlocfilehash: b79f7eb4171793681a0b654e6e72a83191c76bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-virtual-machines-azure-resource-manager-template"></a>Använd Azure Resource Manager-mall för en virtuell dator

När du skapar en virtuell dator (VM) i DevTest Labs via hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040), du kan visa hello Azure Resource Manager-mallen innan du sparar hello VM. hello mall kan sedan användas som en bas-toocreate mer lab virtuella datorer med hello samma inställningar.

Den här artikeln beskriver hur tooview hello Resource Manager-mall när du skapar en virtuell dator och hur toodeploy den senare tooautomate skapa hello samma virtuella dator.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Flera Virtuella kontra enskild VM Resource Manager-mallar
Det finns två sätt toocreate virtuella datorer för i DevTest Labs med hjälp av en Resource Manager-mall: etablera hello Microsoft.DevTestLab/labs/virtualmachines resurs eller etablera hello Microsoft.Commpute/virtualmachines resurs. Varje används i olika scenarier och kräver olika behörigheter.

- Resource Manager-mallar som använder en resurstyp i Microsoft.DevTestLab/labs/virtualmachines (som deklarerats i hello ”” resursegenskap i hello mall) kan etablera enskilda lab virtuella datorer. Varje virtuell dator visas sedan som ett enskilt objekt i listan för hello DevTest Labs virtuella datorer:

   ![Lista över virtuella datorer som enda objekt i listan för hello DevTest Labs virtuella datorer](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Den här typen av Resource Manager-mall kan etableras via hello Azure PowerShell-kommandot **ny AzureRmResourceGroupDeployment** eller via hello Azure CLI kommandot **az distribution skapa** . Det krävs administratörsbehörighet, så att användare som är tilldelade till en DevTest Labs användarroll inte kan utföra hello-distribution. 

- Resource Manager-mallar som använder en Microsoft.Compute/virtualmachines resurstyp kan tillhandahålla flera virtuella datorer som en enda miljö i lista över hello DevTest Labs virtuella datorer:

   ![Lista över virtuella datorer som enda objekt i listan för hello DevTest Labs virtuella datorer](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuella datorer i samma miljö kan hanteras tillsammans hello och dela hello samma livscykel. Användare som är tilldelade till en användarroll för DevTest Labs kan skapa miljöer med hjälp av dessa mallar så länge Hej administratör har konfigurerat hello lab sätt.

hello resten av den här artikeln beskrivs Resource Manager-mallar som använder Mirosoft.DevTestLab/labs/virtualmachines. Dessa används av lab administratörer tooautomate lab skapa en virtuell dator (till exempel claimable virtuella datorer) eller generering av gyllene bild (till exempel bild factory).

[Bästa praxis för att skapa mallar för Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) erbjuder många riktlinjer och förslag toohelp du skapa Azure Resource Manager-mallar som är tillförlitlig och enkel toouse.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Visa och spara Resource Manager-mall för en virtuell dator
1. Följ anvisningarna för hello i [skapar din första virtuella datorn i ett labb](devtest-lab-create-first-vm.md) toobegin skapar en virtuell dator.
1. Ange information om hello som krävs för den virtuella datorn och Lägg till alla artefakter som du vill använda för den här virtuella datorn.
1. Längst ned hello hello konfigurera inställningar för fönstret, Välj **visa ARM-mallen**.

   ![Visa knappen för ARM-mall](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Kopiera och spara hello Resource Manager-mall toouse senare toocreate en annan virtuell dator.

   ![Resource manager-mall toosave för senare användning](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

När du har sparat hello Resource Manager-mall, måste du uppdatera hello parametrar avsnitt i hello mall innan du kan använda den. Du kan skapa en parameter.json som anpassar bara hello parametrar utanför hello faktiska Resource Manager-mall. 

![Anpassa parametrar med hjälp av en JSON-fil](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

## <a name="deploy-a-resource-manager-template-toocreate-a-vm"></a>Distribuera en virtuell dator för toocreate en Resource Manager-mall
När du har sparat en Resource Manager-mall och anpassas efter dina behov kan använda du den tooautomate skapa en virtuell dator. [Distribuera resurser med Resource Manager-mallar och Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) beskriver hur toouse Azure PowerShell med Resource Manager mallar toodeploy tooAzure dina resurser. [Distribuera resurser med Resource Manager-mallar och Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) beskriver hur toouse Azure CLI med Resource Manager mallar toodeploy tooAzure dina resurser.

> [!NOTE]
> Endast en användare med lab ägare behörighet kan skapa virtuella datorer från en Resource Manager-mall med hjälp av Azure PowerShell. Om du vill skapa en virtuell dator tooautomate med hjälp av en Resource Manager-mall och du har bara användarbehörigheter, kan du använda hello [ **az lab vm skapa** i hello CLI](https://docs.microsoft.com/cli/azure/lab/vm#create).

### <a name="next-steps"></a>Nästa steg
* Lär dig hur för[skapa flera Virtuella miljöer med Resource Manager-mallar](devtest-lab-create-environment-from-arm.md).
* Utforska mer Snabbkurs Resource Manager-mallar för DevTest Labs automation från hello [offentliga DevTest Labs GitHub-repo-](https://github.com/Azure/azure-quickstart-templates).
